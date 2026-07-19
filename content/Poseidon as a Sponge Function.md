**Claim:** The Poseidon Hash can be used as a "Sponge Function" making it a viable verifiable PRNG. 

# Motivation
Popular hash functions use bitwise operations such as XOR which are expensive in ZKP framework. Hence a hash function which is based on Finite Field Algebra was of interest. This has led to several "ZK-friendly" hash functions. Poseidon is the latest amongst them. 
# Permutation Function
The hash function is through repeated application of the permutation function. Lets say start state is $[100,200,300]$
## Round Constant
We first add a series of predetermined (public) constants, say $C=[2, 20, 200]$. Now the state is $[102,220,500]$.
## Substitution-box
$S-box$ are one-to-one substitution functions which are typically non-linear. Many-to-one functions are lossy and have collisions making them insecure. In the second step, we apply an $S-Box$ which is basically a power map where we compute $x^\alpha ~(mod~p)$ where $\alpha$ is typically $3,5$ or $7$. It is required that $GCD(\alpha,p-1)=1$ to ensure one-to-one mapping.
## Linear Mixing Layer (MDS Matrix)
To achieve "diffusion" (spreading the influence of a single input bit across the entire state), the state vector is multiplied by a Maximum Distance Separable (MDS) matrix. This guarantees that any change in one element quickly propagates to all other elements in subsequent rounds.
## Full vs. Partial Rounds
To minimize the number of constraints in a Zero-Knowledge circuit, the Poseidon permutation relies on a mix of round types:
* Full Rounds: The S-Box is applied to *every* element in the state vector. These are used at the very beginning and the very end of the permutation.
* Partial Rounds: The S-Box is applied to only a *single* element in the state vector, while the rest pass through linearly. These make up the middle rounds and drastically reduce the algebraic complexity.
## Verifiable PRNG (The Squeeze Phase)
Because Poseidon utilizes the Sponge framework, it naturally functions as a Pseudo-Random Number Generator (PRNG). Once an initial seed is absorbed, the algorithm enters the "Squeeze" phase. By outputting the Rate (r), applying the permutation, and outputting the Rate again, it generates a continuous stream of pseudo-random field elements. Since the entire process uses low-degree algebraic operations, the generation of this random sequence can be efficiently proven inside a zero-knowledge circuit.