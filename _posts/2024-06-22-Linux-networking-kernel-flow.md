---
layout: post
title:  "Packet flow in Linux Kernel"
summary: "How the Linux kernel processes packets."
author: maurice
date: '2024-06-22 21:14:00 +0530'
category: ['linux', 'operating systems', 'kernel', 'hpc', 'networking', 'systems']
tags: linux
thumbnail: /assets/img/posts/skb_layout.png
keywords: arm, cpu, architecture, isa
usemathjax: false
permalink: /blog/linux-kernel-flow/
---

# Introduction
While working on a current research project, I started thinking about socket API calls such as `send()` and `recv()` and their primitive form.  I decided to investigate how the Linux kernel processes network data when the `send()` method is called.  In this post, I will describe the control flow for a TCP packet.

This post assumes that the reader has a strong understanding of TCP/IP networking.

## System Call send()
To begin, the `send` system call is registered, and simply calls the function `__send_sendto()` :

### send() syscall registration
```c
SYSCALL_DEFINE4(send, int, fd, void __user *, buff, size_t, len,
		unsigned int, flags)
{
	return __sys_sendto(fd, buff, len, flags, NULL, 0);
}
```

### __sys_sendto( ... ) call
```c
int __sys_sendto(int fd, void __user *buff, size_t len, unsigned int flags,
		 struct sockaddr __user *addr,  int addr_len)
{
	struct socket *sock;
	struct sockaddr_storage address;
	int err;
	struct msghdr msg;
	int fput_needed;

	err = import_ubuf(ITER_SOURCE, buff, len, &msg.msg_iter);
	if (unlikely(err))
		return err;
	sock = sockfd_lookup_light(fd, &err, &fput_needed);
	if (!sock)
		goto out;

	msg.msg_name = NULL;
	msg.msg_control = NULL;
	msg.msg_controllen = 0;
	msg.msg_namelen = 0;
	msg.msg_ubuf = NULL;
	if (addr) {
		err = move_addr_to_kernel(addr, addr_len, &address);
		if (err < 0)
			goto out_put;
		msg.msg_name = (struct sockaddr *)&address;
		msg.msg_namelen = addr_len;
	}
	flags &= ~MSG_INTERNAL_SENDMSG_FLAGS;
	if (sock->file->f_flags & O_NONBLOCK)
		flags |= MSG_DONTWAIT;
	msg.msg_flags = flags;
	err = __sock_sendmsg(sock, &msg);

out_put:
	fput_light(sock->file, fput_needed);
out:
	return err;
}
```

### __sock_sendmsg() -> sock_sendmsg_noec()

A brief note about INDIRECT_CALL_INET, [indirect calls](https://www.gnu.org/software/gawk/manual/html_node/Indirect-Calls.html), in this context, they are used as an alternative to if/else conditionals to determine the function that the function pointer points to.  Presumably, this method is preferred to avoid using branch instructions (and potential misprediction) and the overhead.  The sock->ops->sendmsg is a function pointer; for INET messages, the inet_sendmsg routine will be selected.

```c
static inline int sock_sendmsg_nosec(struct socket *sock, struct msghdr *msg)
{
	int ret = INDIRECT_CALL_INET(READ_ONCE(sock->ops)->sendmsg, inet6_sendmsg,
				     inet_sendmsg, sock, msg,
				     msg_data_left(msg));
	BUG_ON(ret == -EIOCBQUEUED);

	if (trace_sock_send_length_enabled())
		call_trace_sock_send_length(sock->sk, ret, 0);
	return ret;
}
```

The function pointer is defined in linux/include/linux/net.h and takes the following arguments.
```c
	int		(*sendmsg)   (struct socket *sock, struct msghdr *m,
				      size_t total_len);
```

If you'd like to understand how the indirect call is used, please visit the [indirect_call_wrapper.h](https://github.com/torvalds/linux/blob/master/include/linux/indirect_call_wrapper.h#L57) header.



### inet_sendmsg() -> Indirect_call_2(tcp or udp)
The inet_sendmsg routine uses another indirect call; the function pointer will point to the TCP or UDP sendmsg implementation.  The TCP sendmsg routine calls tcp_sendmsg_lock().  The socket is `locked` before data is transmitted.

```c
int inet_sendmsg(struct socket *sock, struct msghdr *msg, size_t size)
{
	struct sock *sk = sock->sk;

	if (unlikely(inet_send_prepare(sk)))
		return -EAGAIN;

	return INDIRECT_CALL_2(sk->sk_prot->sendmsg, tcp_sendmsg, udp_sendmsg,
			       sk, msg, size);
}
```

```c
int tcp_sendmsg(struct sock *sk, struct msghdr *msg, size_t size)
{
	int ret;

	lock_sock(sk);
	ret = tcp_sendmsg_locked(sk, msg, size);
	release_sock(sk);

	return ret;
}
```

### Moving TCP to IP Layer
The implementation of `tcp_sendmsg_locked` includes a call to [__tcp_push_pending_frames](https://github.com/torvalds/linux/blob/master/net/ipv4/tcp.c#L1297), and this is where things get interesting.  Pushing page frames involves invoking tcp_write_xmit, which traverses the write queue that belongs to the socket and clones the packet.

> [!Note]
> You'll notice several function calls that are wrapped with likely() or [unlikley()](https://github.com/torvalds/linux/blob/master/net/ipv4/tcp_output.c#L3011).  This is a classification used with ELF binaries to improve locality.  See my note about this in the post on [Linkers](https://www.thecodeguardian.dev/blog/learn-about-linkers/#/) in the section `GNU Linker Script Explanation`.


### tcp_write_xmit() -> tcp_transmit_skb()

Next, the function tcp_transmit_skb is called, which builds the TCP header.  Once complete, the `queue_xmit` function pointer is called which invokes IPv4 or IPv6 processing.  The respecitve INET routine will encapsulate the buffered TCP (or UDP) data.

```c
static int __tcp_transmit_skb(struct sock *sk, struct sk_buff *skb,
			      int clone_it, gfp_t gfp_mask, u32 rcv_nxt)
{

    ////////////////////////////
	.... ( LINES OMITTED ) ...

    ///////////////////////////

	err = INDIRECT_CALL_INET(icsk->icsk_af_ops->queue_xmit,
				 inet6_csk_xmit, ip_queue_xmit,
				 sk, skb, &inet->cork.fl);

	if (unlikely(err > 0)) {
		tcp_enter_cwr(sk);
		err = net_xmit_eval(err);
	}
	if (!err && oskb) {
		tcp_update_skb_after_send(sk, oskb, prior_wstamp);
		tcp_rate_skb_sent(sk, oskb);
	}
	return err;
}

```

A similar process follows for each of the lower layers.

## Understanding the SKB Data structure
It's important that you understand the SKB data structure, which is used to represent network packets.  There are several sources online that discuss SKB; however, I found the official [Linux kernel documentation](https://docs.kernel.org/networking/skbuff.html) to be the most helpful.

## Why is this important

Current research in High-Performance Computing (which typically defines the initiaives of network developers) is exploring alternative ways to efficiently transmit large amounts of data.  Zero-copy networking is a popular strategy that involves bypassing the kernel networking stack (every I explainned above), to allow data movement from user-space buffers directly to the NIC.  Furthermore, cloud platforms like Azure, have adopted the use of RDMA to support buffer-to-buffer transmission.

Understanding how currently implementations of TCP/IP work is crucial for discovering improvements.  I was recently introduced to DPDK (Data plane Development Kit); a frameware for implementing zero-copy networking and an abstraction layer in the networking model.  Other interesting projects include [Named Data Networking](), which is one of the [five initiatives sponsered by the National Science Foundation](http://www.nets-fia.net/) for future architecutre of the internet.