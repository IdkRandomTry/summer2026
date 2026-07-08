The **Short Integer Solution (SIS) problem** is a foundational mathematical problem in lattice-based cryptography. Introduced by Miklós Ajtai in 1996, it is widely used to build secure cryptographic systems because it is believed to be computationally hard to solve, even for quantum computers.
# The Problem Statement
Given a large, uniformly random matrix $A$ (with dimensions $n \times m$) containing integers modulo a prime $q$, the goal is to find a non-zero vector $x$ such that:
1. **$Ax \equiv 0 \pmod q$**: The matrix multiplied by the vector yields a zero vector (modulo $q$).
2. **$||x|| \le \beta$**: The vector $x$ is "short," meaning its values (or its Euclidean norm) are bounded by a small threshold $\beta$.
## Importance
If there were no length restriction on $x$, solving the equation $Ax \equiv 0 \pmod q$ would be trivial using standard linear algebra (like Gaussian elimination). The requirement that the vector $x$ must be composed of *short* integers is what makes the problem incredibly difficult. 

SIS is mathematically related to finding the shortest vector in a high-dimensional lattice. Its most remarkable property is its worst-case to average-case hardness. Ajtai proved that if you can solve random instances of the SIS problem, you can solve the hardest possible instances of certain lattice problems. Today, SIS forms the backbone of post-quantum cryptographic primitives, particularly collision-resistant hash functions and digital signatures.