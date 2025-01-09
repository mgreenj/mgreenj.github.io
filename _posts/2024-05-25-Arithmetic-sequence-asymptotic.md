---
layout: post
title:  "Asymptotic Analysis: Arithmetic Sequence"
summary: "Asymptotic analysis of an algorithm using Theta notation and arithmetic sequnce sum of terms"
author: Maurice
date: '2024-05-25 15:45:00 +0530'
category: ['algorithm', 'math', 'programming']
tags: algorithm
thumbnail: /assets/img/posts/asymptotic.png
keywords: algorithm, math, asymptotic, notation, asymptote, analysis, sequence, sum, programming
usemathjax: false
permalink: /blog/asymptotic-analysis-sequence/
---

## Introduction
Recently, a friend who is considering a career shift to software engineering sparked a conversation about algorithms and the analysis of runtime complexity. This friend is naturally curious, often asking "why" to understand new concepts—an admirable habit that many could benefit from adopting. In this post, I'll delve into the runtime of a function, striving to answer as many "why" questions as possible. While I'm not an expert in mathematics, I'll do my best to address these queries, though some may remain unanswered.

### The function
The following snippet shows a nested for loop.  The outer loop starts at 1 and continues through to n - 1.  The inner loop is a little more interesting; it will always start at i + 1, and continue through to n (inclusive).  This means the number of iterations for the inner loop depends on the current value if i.

Observing this routine, it is immediately clear that the Big O time complexity is O(n<sup>2</sup>).  Simarly, Theta notation would also be \(\Theta(n^2)\).

```cpp
void myfunction(int n) {
    int i, j;

    for (i = 1; i < n; i++) {
        for (j = i + 1; j <= n; j++) {
    }
}
```

### Answering the First Why
How do we determine that the runtime of this routine is quadratic?

Remember that asympotic notation to analyze the runtime of an algorithm is looking at the tail behavior of the algorithm.  That is, for a sufficiently large input size n, as n approaches infinity, how does my functions effort grow respective to n.  When working with nested for loops, the key is to count the number of iterations; however, the number of iterations varies, because it depends on the size of n.  In this case, finding the sum requires the use of a little math to find the sum of terms of an arithmetic sequence.  During my original explanation, I immediately provided a well known series to show exactly why this is quadratic.

S<sub>n</sub> = n(a<sub>1</sub> + a<sub>n</sub>)&frasl;2

Where a<sub>1</sub> is the first term in the sequence, a<sub>n</sub> is the last term, and d (not shown above) is the common difference.

Before I could procede, I was once again asked, "why?"  So, before I show how to find the sum using the formula above, I'll show why this formula even works.

#### Derivation of Sum of Arithmetic Series
Consider the sequence 99 + 98 + 97 + 96 + ... + 1.  It is quite easy to see that the common difference is -1, and that the value of a given term a<sub>i</sub> is simply a<sub>n</sub> = a<sub>1</sub> + (n - 1)d.  For example, let n equal 4, then the 4<sup>th</sup> term in this sequence, a<sub>4</sub>, is 96.  This could easily be found by taking n - 1, which is 3 and multiplying it by the common difference of -1 to give -3.  Then, adding this to a<sub>1</sub>, yields 99 + (-3) = 96.

This works well, but how did I know that the n<sup>th</sup> term can be found using the formula a<sub>n</sub> = a<sub>1</sub> + (n - 1)d?  To show this, I will start by defining a few variables:

S<sub>n</sub> = Sum of the sequence for the first n terms.
d = Again, refers to the common difference between any two neighboring numbers in the sequence.
a<sub>1</sub> = The first number in the sequence.
a<sub>n</sub> = The n<sup>th</sup> term in the sequence.

Looking at a sequence, I can develop a pattern that will allow me to build a formula.

S<sub>n</sub> = a + (a + d) + (a + 2d) + (a + 3d) + ... + [a + (n - 1)d]

Notice that the first term a<sub>1</sub> begins with a and the second term increases a by the common difference, d.  Makese perfect sense so far!  The third term, therefore, must be 1 more than the second; however, since I'm only looking to develop a formula and don't know what the second term is, I must use for first term a<sub>1</sub> or a, as a base.  Every other term will calculate its value relative to the base a.  Remember that there is a common difference d between two neighboring numbers in the sequence.  So, the second term is simply (a + d), while the third term would be (a + d + d), or (a + 2d).  That's because it is `d` more than its predecessor (a + d).  This pattern continues until the end of the sequence: (a + 3d), (a + 4d), ..., [a + (n - 1)d].  The final term uses `(n - 1)` instead of an integer because we don't know the size of the sequence.

The next step is to reverse the sum that we just defined above.

Given S<sub>n</sub> = a + (a + d) + (a + 2d) + (a + 3d) + ... + , the reversed sum is

S<sub>n</sub> = [a + (n - 1)d] + [a + (n - 2)d] + [a + (n - 3)d] + ... + a

Finally, we add the two together:

S<sub>n</sub> + S<sub>n</sub> = {a + [a + (n - 1)d]} + {[a + d] + [a + (n - 2)d]} + {[a + 2d] + [a + (n - 3)d]} + {[a + 3d] + [a + (n - 4)d]} + {[a + (n - 1)d] + a}

Simplifying gives:

2S<sub>n</sub> = [a + a + (n - 1)d] + [a + d + a + (n-2)d] + [a + 2d + a + (n - 3)d] + [a + 3d + a + (n - 4)d] + [a + a + (n - 1)d] .

Each term simplifies to [2a + (n - 1)d].  To save time, I won't simplify each one, but I'll show how to simpify the second term, [a + d + a + (n-2)d].

[a + d + a + (n-2)d] can be written as [a + d + a + dn -2d], which becomes [2a + d + dn - 2d].  This simplifies further to [2a + dn - 1d] and factoring yields the same [2a + (n - 1)d].

We now know that each term is [2a + (n - 1)d] and given `n` terms, we are left with the following:

2S<sub>n</sub> = n * [2a + (n - 1)d].  To isolate S, we simply divide by two: S<sub>n</sub> = n * [2a + (n - 1)d]&frasl;2 .

Our formula is as follows:

S<sub>n</sub> = n * [2a + (n - 1)d]&frasl;2

#### What exactly does this mean?
Remember that we want to find the sum of the first to n<sup>th</sup> (or first to last term).  Given the formula above, we consider the first element of the sequence, a<sub>1</sub> to be `a` and the n<sup>th</sup> element, a<sub>n</sub> to be a<sub>n</sub> = a + (n - 1)d.  Therefore,

[2a + (n - 1)d]&frasl;2 = a + [a + (n - 1)d] = a<sub>1</sub> + a<sub>n</sub>

Finally giving, S<sub>n</sub> = n(a<sub>1</sub> + a<sub>n</sub>)&frasl;2

### Applying this to our loop!
Looking at our original loop, we are now able to count the number of iterations for the inner and outer loop.

```cpp
void myfunction(int n) {
    int i, j;

    for (i = 1; i < n; i++) {
        for (j = i + 1; j <= n; j++) {
    }
}
```

This snippet uses n, I don't know when the loop will terminate.  Instead, I can define a series that uses n as a relative point.

| i = 1 | i = 2 | i = 3 | i = 4 | i = ... | i = (n - 1) |
|----------|----------|----------|----------|----------|----------|
| j = 2; (n - 2) + 1 iterations | j = 3; (n - 3) + 1 iterations | j = 4; (n - 4) + 1 iterations | j = 5; (n - 4) + 1 iteratoins | ... | j = n; 1 iteration |

Inner loop sequence: (n - 2) + 1, (n - 3) + 1, (n - 4) + 1, ..., 1 which simiplifies to:

(n - 1), (n - 2), (n - 3), ... , 1

Using the formula defined earlier, we have S<sub>n</sub> = n ((n - 1) + 1)&frasl;2 , which simplifies to n<sup>2</sup>&frasl;2.  The dominant term is clearly n<sup>2</sup>, which is how we know that the nested loop above is O(n<sup>2</sup>) and grows quadratically with `n`.

### Theta Notation
The functional definition of Theta notation is: \Theta(g(n)) = f(n) &exist; c<sub>1</sub>, c<sub>2</sub> >= 0 and n<sub>0</sub> such that 0 <= c<sub>1</sub> * g(n) <= f(n) <= c<sub>2</sub> * g(n) &forall; n >= n<sub>0</sub>.

#### Upper Bound

T(n) = n<sup>2</sup>&frasl;2 <= n<sup>2</sup>&frasl;2 so I can choose 1&frasl;2 for c<sub>2</sub>

#### Lower Bound

T(n) = n<sup>2</sup>&frasl;2 >= n<sup>2</sup>&frasl;4 so I can choose 1&frasl;4 for c<sub>1</sub>.

Finally, I have 1&frasl;4 * n<sup>2</sup> <= f(n) <= 1&frasl;2 * n<sup>2</sup> .  This means that for a sufficiently large n, this routine is \Theta(n<sup>2</sup>).