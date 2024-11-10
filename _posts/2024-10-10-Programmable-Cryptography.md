---
layout: post
title: Programmable Cryptography
---
<!-- # Programmable Cryptography: Three Easy Steps -->
These are my notes from the book _Programmable Cryptography: Three Easy Steps_. 

# Chapter 2: Two-Party Computation
The premise of two-party computation is as follows:
- Alice and Bob have some secret values $a$ and $b$ respectively.
- They would like to compute $F(a, b)$ for some known function $F$, without revealing their secret to each other (i.e. without Alice figuring out $b$ or Bob figuring out $a$).

A well-known setting of this problem is Yao's millionaire problem, in which Alice and Bob want to learn which of the two of them earns more money, but neither of them are comfortable sharing their exact incomes with each other.


