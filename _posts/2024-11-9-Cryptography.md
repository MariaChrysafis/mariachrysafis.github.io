---
layout: post
title: Hello World
---

An introduction to zero-knowledge proofs.

# Zero Knowledge Proofs
What is a Zero Knowledge proof (ZKP)? The essence of a ZKP is that the prover wants to demonstrate to the verifier that they have some information, without revealing that information itself.

To give a concrete example, suppose that there are two balls, one red and one blue, and that the prover wants to prove to the verifier that he can tell the difference between those two balls. The verifier is colorblind and cannot distinguish between the two balls. One way for the prover to show the verifier that he can tell the difference between the two balls is as follows:
- The verifier hides one ball from the prover, only showing him one ball at a time.
- At each iteration:
    - Randomly, with $\frac{1}{2}$ probability, the verifier swaps the balls without the prover realizing it. That is, he shows a different ball to the prover. 
    - The prover tells the verifier if he swapped balls.

In this case, the verifier knows whether or not he swapped balls, so he can tell if the prover is lying. The prover also can't bluff: if he can't distinguish between the two balls, then there is a $\frac{1}{2}$ probability that he gets caught at each iteration, so after $n$ iterations, the probability he is still not caught is $1 - \frac{1}{2^n}.$ With sufficiently many iterations $n$, the verifier can become convinced that the prover knows the difference between the red and blue balls. Yet, crucially--and this is the zero knowledge part--the verifier never learns the colors of the balls.
    