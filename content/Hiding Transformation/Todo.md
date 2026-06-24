
- [x] Find a good motivating example - [[Idea#Better Blurring]] and [[DP-Pix Provenance]]
	- [ ] Find more use cases
- [x] Read and Understand DP Pix - [[DP-Pixelation]]
	- [x] Understand DP - [[DP-Pixelation#Differential Privacy]]
- [x] Understand why Laplacian noise Guarantees DP - [[Laplacian Noise#Laplacian Noise guarantees DP]]
- [ ] Explore other Methods to add DP noise
	- [ ] The Geometric Mechanism (Discrete Laplace) - skipped
	- [ ] The Binomial Mechanism (Difference of Binomials) - Gives epsilon-delta privacy
		- [x] Conceptual understanding
		- [ ] Proof understanding
	- [ ] The Gaussian Mechanism - skipped
- [ ] Explore existing ZK Proof Systems for verifying Random Seed
	- [ ] Poseidon Hash - [[Poseidon]]
	- [ ] Sponge Hashes: A system to (mathematically) convert Hashing to PRNG - [[cryptographic sponge functions.pdf]]
	- [ ] folding to handle multiple instances - [[Nova (folding).pdf]]
	- [x] Discuss with GG for advice here as well.
- [ ] Explore existing ZK Proof Systems for showing $X \leftarrow Distribution$
	- [ ] for Laplace - skipped
	- [ ] Geometric - skipped
	- [ ] Binomial
	- [ ] Gaussian - skipped
- [ ] Find a method for truncation before adding noise.
	- [ ] Read and understand [[Plonk.pdf]] may help
	- [ ] Read Folding as well maybe [[Nova (folding).pdf]]

- [x] Discuss with GG which approach is best - Need to do hw regarding the methods for this discussion.
- [ ] Discuss with GG about  potential generalization.
- [ ] Discuss with GG if we need experimental result data and how to achieve.

# Todo before next GG meet:
- [ ] Solution for Truncation
- [ ] Understand Poseidon in depth
- [ ] Understand the proof of why Binomial Sampling gives epsilon-delta DP