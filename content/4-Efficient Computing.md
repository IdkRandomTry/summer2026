We say that an algorithm is efficient if it runs in polynomial time. One may argue about the choice of polynomial-time as a cutoff for efficiency, and indeed if the polynomial involved is large, computation may not be efficient in practice. There are, however, strong arguments to use the polynomial-time definition of efficiency:

- Independent of representation
- Closed under compositions
- trend
- separation is large (poly > exp is a huge jump)

# Hard Problems
## Halting
Undecidable / Uncomputable

## Time-Hierarchy Theorem
There exist languages that are decidable in time o(t(n)) but cannot be decided in time o(t(n) / log t(n)).
Proof uses Diagonalization (It is CCT notes if details needed)

## SAT
SAT is NP-Complete by Cook Levin (CCT notes if needed)
By P not equal to NP => SAT not in P

# Probabilistic Poly-time Turing Machines (PPTs)

## Randomized TM
TM has access to a Random Tape (Tape filled by random sequence of 1s and 0s). We assume oracle access to getting a random bit from this tape, i.e. for defining PPTs we donot worry about the running time (and correctness) of the random number generator algorithm.

## Runtime of RTMs

A randomized Turing machine $\mathcal{A}$ runs in time $T(n)$ if for all $x \in \{0,1\}^*$, and for every random tape, $\mathcal{A}(x)$ halts within $T(|x|)$ steps. *This is worst case wrt to the random string*
$\mathcal{A}$ runs in polynomial time (or is an **efficient randomized algorithm**) if there exists a constant $c$ such that $\mathcal{A}$ runs in time $T(n) < n^c+k$.


> PREVIOUS: [[3-Modern Cryptography]]
> NEXT: