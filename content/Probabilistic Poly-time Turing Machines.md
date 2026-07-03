# Randomized TM
TM has access to a Random Tape (Tape filled by random sequence of 1s and 0s). We assume oracle access to getting a random bit from this tape, i.e. for defining PPTs we donot worry about the running time (and correctness) of the random number generator algorithm.
## Runtime of RTMs
A randomized Turing machine $\mathcal{A}$ runs in time $T(n)$ if for all $x \in \{0,1\}^*$, and for every random tape, $\mathcal{A}(x)$ halts within $T(|x|)$ steps. *This is worst case wrt to the random string*
$\mathcal{A}$ runs in polynomial time (or is an **efficient randomized algorithm**) if there exists a constant $c$ such that $\mathcal{A}$ runs in time $T(n) < n^c+k$.
## Function Computation in Randomized Algorithms
A randomized algorithm $\mathcal{A}$ computes a function $f : \{0,1\}^* \to \{0,1\}^*$ if for all $x \in \{0,1\}^*$, $\mathcal{A}$ on input $x$, outputs $f(x)$ with probability 1. Notice that the probability is taken over the uniform distribution of choices on the random tape of $\mathcal{A}$.
# Class BPP (Bounded-error Probabilistic Polynomial time) 
A language $L \subseteq \{0,1\}^*$ is in **BPP** if there exists a probabilistic polynomial-time algorithm $\mathcal{A}$ such that for every input $x \in \{0,1\}^*$: 
- **Completeness:** If $x \in L$, then $\Pr[\mathcal{A}(x) = 1] \ge \frac{2}{3}$ 
- **Soundness:** If $x \notin L$, then $\Pr[\mathcal{A}(x) = 1] \le \frac{1}{3}$
## Probability Amplification (The Majority Rule)
If an efficient (polynomial-time) randomized algorithm $\mathcal{A}$ only achieves a weak success probability (just above 1/2) it can be amplified to near-certainty.

* **Weak Success:** $\Pr[\mathcal{A}(x) = f(x)] \ge \frac{1}{2} + \frac{1}{\text{poly}(n)}$
* **Strong Success:** $\Pr[\mathcal{A}'(x) = f(x)] \ge 1 - 2^{-n}$

### The Mechanism
To build the amplified machine $\mathcal{A}'$:
1. Run the weak algorithm $\mathcal{A}$ independently a polynomial number of times on the same input $x$.
2. Take a **majority vote** of the results. 

By applying the **Chernoff Bound**, the probability that the incorrect output wins the majority vote drops exponentially with the number of repetitions. (Calc in notebook page 2 for ref).

> [!note] PPT in current context
> In this context, a polynomial-time randomized algorithm that allows for bounded error is called a **probabilistic polynomial-time Turing machine (p.p.t.)** or an **efficient randomized algorithm**. We assume that such an algorithm will not result in error.
