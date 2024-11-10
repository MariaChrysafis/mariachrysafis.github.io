---
layout: post
title: Programmable Cryptography
---
<!-- # Programmable Cryptography: Three Easy Steps -->
These are my notes from the book _Programmable Cryptography: Three Easy Steps_. 

# Chapter 2: Two-Party Computation
The name "Two Party Computation" comes from the fact that two parties (Alice and Bob) want to perform some computation. In particular,
- Alice and Bob have some secret values $a$ and $b$ respectively.
- They would like to compute $F(a, b)$ for some known function $F$, without revealing their secret to each other (i.e. without Alice figuring out $b$ or Bob figuring out $a$).
<div class="example">
Alice and Bob want to find out which of the two of them are richer, but they do not want to disclose their incomes to each other, nor do they want to disclose their incomes to third parties. In this context, Alice's secret $a$ is her income; Bob's secret $b$ is his income, and the function they want to collectively compute takes the form
\begin{align}
F(a, b) = \begin{cases}
-1 & a < b \\ 
0 & a = b \\
+1 & a > b
\end{cases}.
\end{align}
If $F(a, b) = -1$, then Bob makes more; if $F(a, b) = 0$, then they are both equally wealthy; and if $F(a, b) = +1$, then Alice is the richer of the two of them. 
</div>

This is a test. i guess
