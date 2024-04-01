---
title: Get Started
layout: page
permalink: /current-trends/
---

# Trending topics in Cybersecurity


## Want to stay up to date on current security threats ?

This page will provide a brief description and link to recent security bugs.  Occasionally, I may include links to non security related trends.



## Apple M-Series Chip Vulnerability "GoFetch"

The GoFetch vulnerability allows an attacker to steal secret keys from cryptographic applications.  The exploit targets a CPU feature known as data memory-dependent prefetcher.  CPU prefetching is very important optimization that improves CPU performance.  The CPU can make a prediction on what the computer will soon need and prefetch instructions.  The DMP can observe data values in memory.  In the Apple m-series chip, the DMP will dereference any data that resembles a pointer, which violates constant-time programming.  If an attacker can correctly guess bits of a cryptographic key, they can reassemble the entire key.

<iframe width="560" height="315" src="https://www.youtube.com/embed/wpDXpmOxR1Y" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>


To read more about this vulnerability, visit the official [GoFetch information page](https://gofetch.fail/).
