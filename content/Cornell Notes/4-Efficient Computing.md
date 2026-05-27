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

## Function Computation in Randomized Algorithms
A randomized algorithm $\mathcal{A}$ computes a function $f : \{0,1\}^* \to \{0,1\}^*$ if for all $x \in \{0,1\}^*$, $\mathcal{A}$ on input $x$, outputs $f(x)$ with probability 1. Notice that the probability is taken over the uniform distribution of choices on the random tape of $\mathcal{A}$.

## Class BPP 
Again, chk CCT Notes for Details
### Probability Amplification (The Majority Rule)
If an efficient (polynomial-time) randomized algorithm $\mathcal{A}$ only achieves a weak success probability (just above 1/2) it can be amplified to near-certainty.

* **Weak Success:** $\Pr[\mathcal{A}(x) = f(x)] \ge \frac{1}{2} + \frac{1}{\text{poly}(n)}$
* **Strong Success:** $\Pr[\mathcal{A}'(x) = f(x)] \ge 1 - 2^{-n}$

### The Mechanism
To build the amplified machine $\mathcal{A}'$:
1. Run the weak algorithm $\mathcal{A}$ independently a polynomial number of times on the same input $x$.
2. Take a **majority vote** of the results. 

By applying the **Chernoff Bound**, the probability that the incorrect output wins the majority vote drops exponentially with the number of repetitions. The Chernoff calc is in notebook page 2.

> [!note] PPT in current context
> In this context, a polynomial-time randomized algorithm that allows for bounded error is called a **probabilistic polynomial-time Turing machine (p.p.t.)** or an **efficient randomized algorithm**. We assume that such an algorithm will not result in error.

# Efficient Private-Key Encryption

## Definition 24.7 (Efficient Private-key Encryption)
A $(\text{Gen}, \text{Enc}, \text{Dec})$ triplet is called an *efficient private-key encryption scheme* if the following holds:

1. $k \leftarrow \text{Gen}(1^n)$ is a p.p.t. such that for every $n \in \mathbb{N}$, it samples a key $k$.
2. $c \leftarrow \text{Enc}_k(m)$ is a p.p.t. that given $k$ and $m \in \{0,1\}^n$ produces a ciphertext $c$.
3. $m \leftarrow \text{Dec}_k(c)$ is a p.p.t. that given a ciphertext $c$ and key $k$ produces a message $m \in \{0,1\}^n \cup \{\bot\}$.
4. For all $n \in \mathbb{N}$, $m \in \{0,1\}^n$,
$$\Pr \left[ k \leftarrow \text{Gen}(1^n) : \text{Dec}_k(\text{Enc}_k(m)) = m \right] = 1$$

# Efficient Adversaries
When modeling adversaries, we use a more relaxed notion of efficient computation which makes sense as we wish to consider adversaries with more power. In particular, instead of requiring the adversary to be a machine with constant-sized description, we allow the size of the adversary’s program to increase polynomially with the input length, i.e., we allow the adversary to be *non-uniform PPT.* 
Intuitively, we allow different reasonably sized attacking algorithms for different size of ciphertexts. Similar to circuits families..?

> [!abstract] NU PPTs
A non-uniform probabilistic polynomial-time machine (abbreviated n.u. p.p.t.) A is a sequence of probabilistic machines A = {A1, A2, . . .} for which there exists a polynomial d such that the description size of |Ai| < d(i) and the running time of Ai is also less than d(i). We write A(x) to denote the distribution obtained by running $A_{|x|}(x)$.

> [!note] Note:
>" Alternatively, a non-uniform p.p.t. machine can also be defined as a uniform p.p.t. machine A that receives an advice string for each input length. In the rest of this text, any adversarial algorithm A will implicitly be a non-uniform PPT. "

> PREVIOUS: [[3-Modern Cryptography]]
> NEXT: [[5-One Way Functions]]