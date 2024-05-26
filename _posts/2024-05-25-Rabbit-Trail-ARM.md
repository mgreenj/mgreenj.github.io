---
layout: post
title:  "ARM: Going down a rabbit trail in ARM"
summary: "Learn about ARM AAPCS and THUMB ISA."
author: maurice
date: '2024-05-25 23:45:00 +0530'
category: ['arm', 'architecture', 'cpu']
tags: architecture
thumbnail: /assets/img/posts/arm-cpu.png
keywords: arm, cpu, architecture, isa
usemathjax: false
permalink: /blog/arm-rabbit-trail/
---

# How it started
Earlier today I as looking at some code that I wrote on [godbolt.org](https://godbolt.org/) and noticed an interesting symbol named `__aeabi_idivmod`.  The code included an implementation of gcd(a, b), following the Euclidean (non-extended) algorithm, which includes modulo operation.  The assembly label that included the reference to the previous symbol is included below.

```ArmAsm
.L31:
        mov     r1, r4
        bl      __aeabi_idivmod
        mov     r0, r4
        cmp     r1, #0
        bne     .L40
        cmp     r4, #1
        beq     .L41
```

Curiosity prompted an investigation into `__aeabi_idivmod`; I was able to find the [ARM assembler file](https://codebrowser.dev/llvm/compiler-rt/lib/builtins/arm/aeabi_idivmod.S.html) that includes the definition for this modulo division.  This is an obvious reference to an ABI and I found some [helpful documentation](https://github.com/ARM-software/abi-aa/blob/main/rtabi32/rtabi32.rst#other-c-and-assembly-language-helper-functions) on GitHub that provides some interesting information.  One thing in particular stood out:

> Implementations of idiv, uidiv, idivmod, and uidivmod have full [AAPCS32](https://github.com/ARM-software/abi-aa/releases) privileges and may corrupt any register an AAPCS-conforming call may corrupt.

## What is AAPCS32?
Full documentation on AACS32 can be found [here](https://github.com/ARM-software/abi-aa/releases/download/2023Q3/aapcs32.pdf).  Notably, the order of function parameter matters.  According to the AACS32 documentation, the following general purpose registers are relevant: r0, r1, r2, and r3.  (For additional information on ARM registers, please refer to my [GitBook Reverse Engineering page](https://docs.thecodeguardian.dev/v/reverse-engineering/arm-registers)).  Note that the majority of my notes are included in the [Low Level Computing](https://docs.thecodeguardian.dev/) page and not the Reverse Engineering page.

According to the AACS32, registers r0 & r1 are scratch registers for results, while r2 & r3 are scratch registers for other use.  Crucially, AACS32 defines different behavior for datatypes that exceed 32-bits.

The first four registers (r0, r1, r2, and r3) are used to pass argument values into a subroutine and return a result.  Something else worth mentioning, is that AAPCS32 seems to be linked to the THUMB architecture.

> The AAPCS requires that all sub-routine call and return sequences support inter-working between Arm and Thumb states....

In the snippet above, you'll notice that the instruction is `bl` or branch with link; an instruction that, according to the AAPCS32, is used to call a subroutine. Remember that general purpose register r14 on an ARM architecture is used for link; that is, r14 will hold the address to return when a subroutine call completes.

```cpp
int gcd(int a, int b) {
    if (b == 0)
        return a;
    return gcd(b, a % b);
}
```

Looking at the function above, the subroutine call to `__aeabi_idivmod` is implementing gcd(b, a % b).  The assembly shows that a value in the register r4 is moved in r1, which is a scratch register used for arguments (along with r3).  The `bl` instruction calls the subroutine and stores the return address in `r14` the Link register.  The quotient is stored in `r0` and the remainder in `r1`.

I believe the `mov r0, r4` instruction is moving the value for b into `r0` for a comparison, `if (b == 0)`.  The instructoin  `cmp r1, #0` compares the remainder (int `r1`) to 0

The remaining assmebly is self-explanatory.

## Why did I investigate this?
Learning about the internals of a CPU (or anything in general) is always beneficial in cybersecurity.  The more you know, the capable you are as a security researcher.

Additionally, I find this topic interesting, and enjoy learning about the internals of a CPU and the compiler.