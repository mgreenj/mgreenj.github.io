---
layout: post
title:  "The Rust Programming Language: Will Rust replace C/C++?"
summary: "Learn why Rust may or may not replace older programming languages, such as C and C++."
author: Maurice
date: '2024-09-11 12:29:00 +0530'
category: ['rust', 'programming', 'security', 'opinion']
tags: programming
thumbnail: /assets/img/posts/rust.png
keywords: rust, programming, security, opinion
usemathjax: false
permalink: /blog/rust-vs-cpp/
---

I'm writing this article to share my opinion on the Rust vs C/C++ debate. This is my opinion, and it's not backed by research. Admittedly, I'm biased toward C/C++; as a systems enthusiast, I appreciate the aspect of memory management and prefer being responsible for it. Nevertheless, I will be as objective as possible.  The list includes reasons why I don't think Rust will replace C/C++ within the next ten years.

# Reasons Why Rust may not replace C/C++

## Legacy Code

One of the primary inhibitors to replacing a language is the existence of legacy code. Many legacy programs are written in C/C++, and many are poorly documented, making them challenging for engineers to replace. Especially in Telecommunications and Banking, people avoid touching functional systems running legacy code if those systems are vital to business continuity. Replacing legacy code can be time-consuming and expensive and may require adjusting how systems integrate, further complicating the matter.

## Operating Systems

Furthermore, the most popular operating system kernels (Windows NT, XNU, and Linux) are written in C/C++, which precludes either language from becoming obsolete. *(Nearly every operating system is written in C/C++).* OS Development is less common but never stagnates; each operating system requires continuous development to meet changing demands and bug fixes. The relationship between compiler, operating system, and architecture is strong. The approaching end of Moore's Law has led researchers to explore software-hardware co-design to achieve further performance improvements. The operating system cannot be (fully) circumvented to achieve co-design goals. (Note: There is an increasing amount of support for Rust in the Linux Kernel)\[[1](https://docs.kernel.org/rust/),[2](https://rust-for-linux.com/rust-reference-drivers)\].

## Industry Adoption

Industries will continue using antiquated languages, provided the tradeoffs between performance, security, and portability are reasonable. Consider Scientific and High-Performance Computing, especially in physics and meteorology, where Fortran remains a prevalent language because it efficiently handles large and computationally expensive models. For a programming language, including Rust, to replace C/C++ in an industrial context, it must significantly outperform them in their specific applications, rendering any negative tradeoffs negligible.

Crucially, performance is not measured uniformly, and a preference to optimize one performance aspect can inadvertently degrade another. Consider this [performance benchmark comparing C++ to Rust](https://benchmarksgame-team.pages.debian.net/benchmarksgame/fastest/rust-gpp.html). As I mentioned earlier, the relationship between compiler, OS, and architecture matters; the data included in this post illustrates how the compiler and form of programming matter, and the results, in most cases, are comparable. The relevancy of non-marginal differences between C++ and Rust is constrained to context. This underscores my original point about industry adoption, where the *why* matters.

## Standardization & Community

System complexity, especially systems with 3rd party integrations, may discourage product owners from adopting Rust as a replacement for C++. To date, Rust does not have its own ABI; rather, it provides compatibility against the C ABI. Furthermore, C++ is more popular and has a larger community of support. The lack of broad community and standardization may make the complete adoption of Rust unfavorable in contexts demanding high availability and with low fault tolerance. Despite not having an official Rust ABI, Rust appears to be a predominantly dynamically linked language that can take advantage of Link-Time optimizations.

## Learning Barrier

Learning Rust can be challenging! As stated in an [efinancialcareers.com article](https://www.efinancialcareers.com/news/rust-vs-c-plus-plus-financial-services-low-latency), an incentive (most likely money) is likely a prerequisite to learning Rust well. My experience working in engineering in a few FinTech firms is that Quants and Traders like to make code contributions; this is good. For traders, writing code, in most cases, is a voluntary aspect of their job and, in some cases, a preference. Therefore, the incentive to learn Rust well is low, albeit those who contribute to the C++ code base are typically Full-Time engineers or Quants. To reiterate an earlier point, if the performance improvements when using Rust are marginal for the specific applications in which C++ is currently used, the incentive to replace C++ with Rust is low.

## Other Languages

The common motif of this post is that context matters. One industry where C++ remains popular is FinTech. After thinking about Rust replacing C++ in the financial industry, I was reminded of another language, OCaml, which has grown in popularity at a famous firm known as Jane Street. Although I've explored the OCaml compiler in the past, I don't feel qualified to compare it with Rust; however, I was able to find [another individual who did](https://blog.darklang.com/first-thoughts-on-rust-vs-ocaml/). Other programming languages are gaining popularity, most notably the Julia programming language. Julia is marketed as a high-performance programming language ideal for environments demanding parallelism and speed. A [post by the University of ](https://guides.libraries.uc.edu/julia#:~:text=Julia%20is%20a%20high%2Dlevel,easy%20to%20use%20as%20Python)Cincinnati states that it's "designed to give users the speed of C/C++ while remaining as easy to use as Python."  Some engineering teams may choose to incorporate Julia to replace C++ code. It's also possible that organizations may choose to continue developing applications in C++ in tandem with other robust and performant languages like Rust or Julia.

# When is C++ ideal?

Briefly, C++ is excellent for GPU programming, High-Performance applications, such as those used for real-time processing, and gamedev (game development). C++ has been around for a while, so you can search for anything you want.

# When is Rust Ideal?

Rust is excellent for low-level operations, and I wouldn't be surprised if it eventually becomes the primary language in embedded systems development \[[1](https://www.rust-lang.org/what/embedded)\]. If Rust is suitable for embedded, it may also have an opportunity to become a mainstream mobile operating system. A language-based operating system named [RustOS](https://github.com/ryanra/RustOS) has grown in popularity. If that's not convincing enough, Google is developing an operating system called [Fuchsia](https://fuchsia.dev/), a "general purpose OS, designed to power a diverse ecosystem of software and hardware."  Google has a successful track record of turning in-house applications and tools into widely adopted commercial products: ChromeOS, Android, Kubernetes, and Google Cloud Platform, to name a few. Though not fully developed in Rust, much of the code is written in Rust.

# What Can Accelerate Adoption of Rust?

Feel free to enumerate this list to explore each idea further if interested. 

* University Computer Science and Cybersecurity programs must incorporate Rust into the curriculum.
   * Students always remember their first, and the first language learned and used typically remains the favorite. My university's CS department preferred C and C++, mainly C++. Programming assignments were almost exclusively written in C/C++. This is not surprising for systems-based courses like Computer Architecture, Operating Systems, Systems Programming, Computer Networking, and Compilers & Runtime; however, it remains true for DS&A, Intro to Programming, etc. Although I use and appreciate Python, I rarely use Java, C#/.NET, Go, or any other language. Being a systems guy, I am undoubtedly biased, but if my assignments were written in Java, I'd like it more. 
* University and industry Rust hackathons.  
   * Hackathons incentivize students and professionals to learn Rust well.
   * Builds the Rust community, which leads to broader support for libraries, integration, and migration (from legacy code to Rust).
   * Analogous to bug bounty programs, organizations can outsource the development of secure solutions to professionals and hobbyists worldwide.
* Application-specific texts for learning Rust.
   * Books that converse Fundamentals
   * Book for Systems programmers
   * Book for Compiler Engineers
   * Book for Financial Engineers
   * Book for Scientists
* Accessible and Structured Course for learning Rust
   * Should be paid for by the employer
* High-paying Rust jobs
* An Operating System written in Rust
   * As mentioned, there are a few exciting projects under development. A powerful Rust-based operating system for embedded, mobile, enterprise, and consumer computing devices would completely change Rust's trajectory.

On a potentially positive note for Rust, the White House issued a press release in 2024 advocating memory safety and the use of memory-safe programming languages \[[1](https://www.whitehouse.gov/oncd/briefing-room/2024/02/26/press-release-technical-report/), [2](https://www.whitehouse.gov/wp-content/uploads/2024/02/Final-ONCD-Technical-Report.pdf)\].