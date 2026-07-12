Paper: [[HyperPlonk.pdf]]

HyperPlonk is an advanced adaptation of an earlier system Plonk.
# The Boolean Hypercube vs. Cyclic Subgroups
Standard Plonk encodes the execution trace of a circuit as univariate polynomials evaluated over a multiplicative cyclic subgroup of a finite field. HyperPlonk encodes the trace as *multilinear polynomials* evaluated over a [[Boolean Hypercubes]] $\{0, 1\}^\mu$.

# PIOP - Polynomial Interactive Oracle Proof
Polynomial Interactive Oracle Proofs (PIOPs) serve as a modular cryptographic abstraction that decouple high-level proof logic (such as Sum-Check or multiset checks) from the underlying commitment schemes. This allows swaping backend Polynomial Commitment Schemes (PCS) like KZG or Brakedown without altering circuit logic. A Prover commit to polynomial oracles that the Verifier queries at random challenge points.

The paper elaborates on several important PIOPs such as:
- [[Sum Check PIOP]] used for [[Inner Product PIOP]]
- [[Zero Check PIOP]]
- [[Lookup PIOP]]
- [[Multiset PIOP]]
which are of relevance.