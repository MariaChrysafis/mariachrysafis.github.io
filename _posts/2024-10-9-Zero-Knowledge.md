---
layout: post
title: Zero Knowledge
---

# Intro to Zero Knowledge Proofs
Suppose I want to convince you that I have a certain type of information, but I don't want to reveal to you the information itself. This is the essence of a zero knowledge proof: the prover is able to convince the verifier of some fact, without leaking any knowledge. We give three examples of zero knowledge proof protocols.
## Red-Blue Balls
To give a concrete example, suppose that there are two balls, one red and one blue, and that the prover wants to prove to the verifier that he can tell the difference between those two balls. The verifier is colorblind and cannot distinguish between the two balls. One way for the prover to show the verifier that he can tell the difference between the two balls is as follows:
- The verifier hides one ball from the prover, only showing him one ball at a time.
- At each iteration:
    - Randomly, with $\frac{1}{2}$ probability, the verifier swaps the balls without the prover realizing it. That is, he shows a different ball to the prover. 
    - The prover tells the verifier if he swapped balls.

In this case, the verifier knows whether or not he swapped balls, so he can tell if the prover is lying. The prover also can't bluff: if he can't distinguish between the two balls, then there is a $\frac{1}{2}$ probability that he gets caught at each iteration, so after $n$ iterations, the probability he is still not caught is $1 - \frac{1}{2^n}.$ With sufficiently many iterations $n$, the verifier can become convinced that the prover knows the difference between the red and blue balls. Yet, crucially--and this is the zero knowledge part--the verifier never learns the colors of the balls.
## $\Delta$-coloring
Suppose Alice (the prover) wants to convince Bob that she knows a valid $\Delta$-coloring of some graph $G = (V, E)$. However, she does not want to reveal the coloring scheme. One protocol is as follows:
- Do the following repeatedly
    - Alice picks a random permutation $p$ from $[\Delta] \to [\Delta]$, where $[\Delta]$ represents the set of all colors, i.e. $ \\{ 1, 2, 3, \ldots \Delta \\}$, which she uses to shuffle her coloring scheme. More formally, if her original coloring scheme was $c: V \to [\Delta]$, her new coloring scheme is $c' = c \circ p$.
    - Bob picks two nodes $u$ and $v$ from the graph. Bob verifies that $c'(u) = c'(v)$. If this is not the case, then Bob knows Alice is lying.

Notice that at each iteration, the probability Alice gets caught is $\frac{1}{\|E\|}$, so after sufficiently many iterations, she will eventually get caught if she lies.

## Discrete Logarithm
Let's give a realistic example. Suppose that Alice (the prover) wants to convince Bob (the verifier) that she knows a solution to the discrete logarithm problem, i.e. an $x$ for which
\begin{align}
    a^x \equiv y \pmod{p}
\end{align}
for some integer $a$ and prime $p$. One protocol is as follows:
- Do the following repeatedly:
    - Alice picks a random integer $r \in [0, p - 2]$; she sends Bob the value of $a^r \pmod{p}.$
    - Bob picks a random boolean, i.e. an integer $b$ that is either $0$ or $1$. He sends that bit to Alice.
    - Alice tells Bob the value of $r + x \cdot b\pmod{p - 1}.$
    - Bob verifies if Alice is telling the truth. If $b = 0$, he verifies that
    \begin{align}
    a^{r + x \cdot b} \equiv a^{r} \pmod{p}
    \end{align} 
    If $b = 1$, he verifies that
    \begin{align}
    a^{r + x \cdot b} \equiv a^{r} \cdot a^{x}  \equiv y \cdot a^{r} \pmod{p}.\end{align}

Crucially, if Alice does not know such a value of $x$, she cannot cheat. Her best strategy would be to always send $r$, since she will pass if $b = 0$. However, she fails half the time, when $b = 1$, so eventually, she will get caught.
# Zero Knowledge Proofs of Arithmetic Circuits
Ultimately, we'd like to apply ZK principles to arithmetic circuits, but this is quite a challenging task. Let's break it up.
## Arithmetic Circuits $\to$ R1CS
Here, we explain how to go from an arithmetic circuit to a rank-1 constraint system (R1CS). A R1CS is nothing more than a constraint system of the form $(s \cdot a) \times (s \cdot b) = s \cdot c$, where $a$, $b$, and $c$ are constant vectors and $s$ is our solution vector. To see how this works in practice, suppose we have some arithmetic circuit as follows:
\begin{align}
    x \times x \times x + y \times y = 57 \newline
    x \times y = 14
\end{align}
for $x, y \in \mathbb{Z}$. This is quite a challenging problem, significantly more involved than our previous discrete-logarithm example. One thing that is perhaps particularly challenging is that our system has products of products and sums of sums. To make things more uniform, we introduce auxiliary variables so that every expression contains two variables/constants on the left and one variable/constant on the right.
\begin{align}
    x \times x &= s_1 \newline
    y \times y &= s_2 \newline
    s_1 \times x &= s_3 \newline
    x \times y &= 14 \newline
    s_1 + s_2 &= 57
\end{align}
We do have more variables than before, but each equation is more maneagble, since it is of the form $\text{var}_1 \text{ op } \text{var}_2 = \text{var}_3$, where for our purposes, $\text{ op }$ is either multiplication ($\cdot$) or addition ($+$). Why is this convenient? Suppose we are trying to verify if the solution vector $s = (s_1, s_2, s_3, x, y, 1)$ satisfies the system of equations. But now, we can transform each equation into one of the form $(s \cdot a) \times (s \cdot b) = c$ for some vectors $a, b, c$, where $\times$ denotes scalar multiplication and $\cdot$ denotes the dot product. Let's see how this works.

The first equation $x \times x = s_1$ can be rewritten as 
\begin{align}
(s \cdot (0, 0, 0, 1, 0, 0)^T) \times (s \cdot  (0, 0, 0, 1, 0, 0)^T) = s \cdot (1, 0, 0, 0, 0, 0)^T.
\end{align}
The second equation can be rewritten as 
\begin{align}
(s \cdot (0, 0, 0, 0, 1, 0)^T) \times (s \cdot  (0, 0, 0, 0, 1, 0)^T) = s \cdot (0, 1, 0, 0, 0, 0)^T
\end{align}
The third equation can be rewritten as 
\begin{align}
(s \cdot (1, 0, 0, 0, 0, 0)^T) \times (s \cdot  (0, 0, 0, 1, 0, 0)^T) = s \cdot (0, 0, 1, 0, 0, 0)^T.
\end{align}
The fourth equation can be rewritten as 
\begin{align}
(s \cdot (0, 0, 0, 1, 0, 0)^T) \times (s \cdot  (0, 0, 0, 0, 1, 0)^T) = s \cdot (0, 0, 0, 0, 0, 14)^T.
\end{align}
The fourth equation is straight forward; the last equation, the only one with the addition operaiton, can be expressed as
\begin{align}
(s \cdot (0, 0, 0, 0, 0, 1)^T) \times (s \cdot  (1, 1, 0, 0, 0, 0)^T) = s \cdot (0, 0, 0, 0, 0, 57)^T.
\end{align}
So we have reduces the system of arithmetic equations to a list of expressions of the form $(s \cdot a) \times (s \cdot b) = s \cdot c$. But how does this help with our ultimate goal of generating ZK proofs for arithmetic circuits? It's not immediately clear yet, we still have some more steps.
## R1CS $\to$ QAP
We need another intermediate step before we apply ZK. We will turn our R1CS into a quadratic arithmetic program (QAP); very crudely speaking, a QAP polynomializes the R1CS. How does this work? 

Before discussing this, it may be helpful to to consider the list of $a$ values as a matrix:
\begin{align}
\begin{bmatrix}
    0 & 0 & 0 & 1 & 0 & 0 \newline
    0 & 0 & 0 & 0 & 1 & 0 \newline
    1 & 0 & 0 & 0 & 0 & 0 \newline
    0 & 0 & 0 & 1 & 0 & 0 \newline
    0 & 0 & 0 & 0 & 0 & 1
\end{bmatrix}
\end{align}
. Now, we can look at each column vector and imagine it as a set of points. For instance, we imagine the first column vector as a set of points $(0, 0), (1, 0), (2, 1), (3, 0), (4, 0)$, which we can in turn interpolate ino a polynomial, in this case $\frac{x^4}{4} - 2 x^3 + \frac{19 x^2}{4} - 3 x$. Then, each column vector represents a polynomial, so we can think of $A$ as 
\begin{align}
a'(x) = 
\begin{bmatrix}
A_1(x) & A_2(x) & A_3(x) & A_4(x) & A_5(x) & A_6(x)
\end{bmatrix},
\end{align}
where as in our example, $A_1(x) = \frac{x^4}{4} - 2 x^3 + \frac{19 x^2}{4} - 3 x$. We do the same thing for $b$s and $c$s.
\begin{align}
b'(x) = 
\begin{bmatrix}
B_1(x) & B_2(x) & B_3(x) & B_4(x) & B_5(x) & B_6(x)
\end{bmatrix} \newline
c'(x) = \begin{bmatrix}
C_1(x) & C_2(x) & C_3(x) & C_4(x) & C_5(x) & C_6(x)
\end{bmatrix}
\end{align}
Now, to verify if $s$ is a solution to our arithmetic circuit, we just have to verify that the function $f(x)$
\begin{align}
f(x) = (a'(x) \cdot s) \times (b'(x) \cdot s) - c'(x) \cdot s
\end{align}
has roots at $0, 1, 2, 3, 4$, or that $f(x)$ can be rewritten as
\begin{align}
f(x) = t(x) \cdot h(x)
\end{align}
for some polynomial $h(x)$, where $t(x) = x \cdot (x - 1) \cdot (x - 2) \cdot (x -3 ) \cdot (x - 4)$.