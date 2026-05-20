At a high level, there are two basic desiderata for any encryption scheme:
- it must be feasible to generate c given m and k, but 
- it must be hard to recover (part or full) m and/or k given only c.

# Worst Case One-Way Functions
A function $f : \{0,1\}^* \to \{0,1\}^*$ is **worst-case *one-way*** if:

1. **Easy to compute.** There is a p.p.t. $\mathcal{C}$ that computes $f(x)$ on all inputs $x \in \{0,1\}^*$, and
2. **Hard to Invert.** There is no adversary $\mathcal{A}$ such that 
$$\forall x \ \Pr[\mathcal{A}(f(x)) \in f^{-1}(f(x))] = 1$$

Issue: This includes $f$ where inverting is possible for most/many x but not all. Hence we need to introduce a stronger notion for this

> [!note] Negligible function
> A function $\varepsilon(n)$ is **negligible** if for every $c$, there exists some $n_0$ such that for all $n > n_0$,
> $$\varepsilon(n) \le \frac{1}{n^c}.$$
> 
> **Intuitively, a negligible function is asymptotically smaller than the inverse of any fixed polynomial.** 
> 
> Examples of negligible functions include: $2^{-n}$ and $n^{-\log \log n}$.
> 
> We say that a function $t(n)$ is **non-negligible** if there exists some constant $c$ such that for infinitely many points $\{n_0, n_1, \dots\}$, $t(n_i) > n_i^{-c}$. This notion becomes important in proofs that work by contradiction.
> 
> Non-Negligible Examples: $\frac{1}{2}$ (constants), $\frac{1}{n^2}$, $\frac{1}{\log n}$, or oscillating functions like $t(n) = \frac{1}{2}$ for even $n$ and $2^{-n}$ for odd $n$ (exceeds an inverse polynomial infinitely often).

# Strong One-Way Function
A function mapping strings to strings $f : \{0,1\}^* \to \{0,1\}^*$ is a *strong one-way function* if it satisfies the following two conditions:

1. **Easy to compute:** There is a p.p.t. $\mathcal{C}$ that computes $f(x)$ on all inputs $x \in \{0,1\}^*$, and
2. **Hard to invert:** Any efficient attempt to invert $f$ on random input succeeds with only negligible probability. Formally, for any adversary $\mathcal{A}$, there exists a negligible function $\epsilon$ such that for any input length $n \in \mathbb{N}$,
$$\Pr [x \leftarrow \{0,1\}^n; y \leftarrow f(x) : f(\mathcal{A}(1^n, y)) = y] \le \epsilon(n).$$
However, not many natural candidates for one-way functions meet the Strong criteria, and hence we introduce the notion of Weak One-Way Function
# Weak One-Way Function

> [!idea] Idea
> Weak One-Way Function is a relaxed version of Strong One-Way Function. It only requires that all efficient attempts at inverting will fail with some non-negligible probability. It only guarantees that a part of the function is hard to compute. 


A function mapping strings to strings $f : \{0,1\}^* \rightarrow \{0,1\}^*$ is a *weak one-way function* if it satisfies the following two conditions.

1. **Easy to compute.** (Same as that for a strong one-way function.)
2. **Hard to invert.** There exists a polynomial function $q : \mathbb{N} \rightarrow \mathbb{N}$ such that for any adversary $\mathcal{A}$, for sufficiently large $n \in \mathbb{N}$,
$$
\begin{aligned}
\Pr \left[ x \leftarrow \{0,1\}^n; \, y \leftarrow f(x) : f(\mathcal{A}(1^n, y)) = y \right] &\le 1 - \frac{1}{q(n)}
\\
\Pr \left[ Adversary~ Wins \right] &\le 1 - \frac{1}{q(n)}
\\
\Pr \left[ Adversary~ Loses \right] &\ge \frac{1}{q(n)}
\\
\end{aligned}
$$

# Primes

## Factoring Assumption
For every adversary $\mathcal{A}$, there exists a negligible function $\epsilon$ such that

$$\Pr \left[ p \leftarrow \Pi_n; \, q \leftarrow \Pi_n; \, N \leftarrow pq : \mathcal{A}(N) \in \{p,q\} \right] < \epsilon(n) ~~where~~ \Pi_n = \{ q|q<2^n ~ and ~~q~is~prime \} $$
This is well studied and is grounded for practical purposes

## Number of Primes
$$\pi(x) = number~of~primes \leq x$$
![[Pasted image 20260520114042.png]]

### Chebyshev Theorem
 $$For~~x>1,~~ \pi(x)>\frac{x}{2log(x)}$$If we think in terms of fraction of numbers being primes, we can say that atleast $\dfrac{1}{2log(x)}$ fraction of the first $x$ numbers are prime 
If we think in terms of bits, for numbers given by a $n$-bit binary, atleast $\dfrac{1}{2n}$ fraction is prime. This means $f_{mult}$ is weak OWF (particularly secure for product of  primes)

If we have enough inputs (x1,y1;x2,y2...) such that F(x1,y1;x2,y2...) = (f(x1,y1), f(x2,y2)), ...) one pair will be of primes making that particular xi,yi hard to invert and get! Hence F is strong OWF. This core idea is formalized in [[5-One Way Functions#Hardness Amplification]] and the calculation with this example is done in textbook pg 31 onwards.
# Hardness Amplification
We take a **Weak OWF** which an adversary can fail to invert with some non-negligible probability and transform it into a **Strong OWF** where the adversary's success rate is negligible.

Let $f$ be a weak one-way function. By definition, there is some polynomial $q(n)$ such that an adversary's probability of inverting $f$ is bounded by:
$$\Pr[\text{A succeeds }] \le 1 - \frac{1}{q(n)}$$

To create a Strong OWF, $F$, we run $f$ multiple times ($k$ times) on **completely independent, random inputs** and concatenate the results:
$$F(x_1, x_2, \dots, x_k) = (f(x_1), f(x_2), \dots, f(x_k))$$

### The Intuitive Calculation
*Note: This calculation provides my intuition behind the construction, not the rigorous concrete proof*

To successfully invert the new function $F$, an adversary must find a valid preimage for **every single piece** of the output. Because the inputs are chosen independently, we can multiply the probabilities of the adversary succeeding on each instance.

If we repeat the function $k = n \cdot q(n)$ times, the probability of the adversary successfully inverting $F$ becomes:
$$\Pr[\text{A succeeds}] \le \left( 1 - \frac{1}{q(n)} \right)^{n \cdot q(n)}$$

Using the limit $(1 - \frac{1}{x})^x \approx e^{-1}$, this evaluates to:
$$\Pr[\text{A Succeeds }] \approx e^{-n}$$
Notice $e^{-n}$  is a negligible function! The weak function has been amplified into a strong one.

> [!idea] Intuition
> Why does this work fundamentally? This method depends on **probabilistic guarantees** to ensure that we hit a "secure input" from the weak one-way function's domain. A weak OWF is easily inverted on *most* inputs, but it guarantees that a small fraction of inputs are hard. By repeating the function on thousands of independent random inputs, the adversary is virtually guaranteed to get stuck on at least one of those hard inputs, rendering the entire combined function unbreakable.

# Collection of One-Way Functions
While standard Strong OWFs are highly secure, their definition is mathematically rigid: they must accept raw, completely unstructured random coin flips (e.g., $x \leftarrow \{0,1\}^n$) as input. 

A Collection of OWFs relaxes the *domain* of the function to be practical, without sacrificing any of the strong security.

A Collection of OWF is a family of functions $\mathcal{F} = \{f_i : \mathcal{D}_i \rightarrow \mathcal{R}_i\}$ where:
1. Function Sampling: It is easy to randomly sample an index $i$ (analogous to generating public parameters).
2. Domain Sampling: Given $i$, it is easy to sample a valid, structured input $x$ uniformly from its specific domain $\mathcal{D}_i$. 
3. Evaluation: It is easy to compute $y = f_i(x)$.
4. Hardness: Given the index $i$ and the output $y$, it is computationally infeasible for a p.p.t. adversary to find $x$. Formally, for any adversary $\mathcal{A}$, there exists a negligible function $\epsilon$ such that for any input length $n \in \mathbb{N}$,
$$\Pr [i \leftarrow Gen;x \leftarrow D_i; y \leftarrow f_i(x) : f(\mathcal{A}(1^n, y)) = y] \le \epsilon(n).$$

> [!note] Why do we need a family of functions?
> A single function with a restricted domain is a static. A **Collection of OWFs** uses the index $i$ as a dynamic element. It counters precomputation, adversaries cannot spend years precomputing a reverse-lookup dictionary. They don't know which specific function $f_i$ they must invert until it is generated.

Despite the relaxations this is still equivalent to Strong OWF
## Collection of OWF $\iff$ Strong OWF
If we are given a Collection $\mathcal{F}$, we can construct a single, standard Strong OWF $g$ that accepts raw random bits. The trick is to take the raw random input bits and split them into two halves: $r_1$ and $r_2$.

* Use $r_1$ as the randomness to generate the index $i$.
* Use $r_2$ as the randomness to sample the structured input $x$ from $\mathcal{D}_i$.

We define our single function as:
$$g(r_1, r_2) = (i, f_i(x))$$

Function $g$ acts as a standard OWF wrapper around the collection. If an adversary could successfully invert $g$, they would be able to look at the output $(i, y)$ and figure out the random bits $(r_1, r_2)$ that created them. If they can do that, they have found the input $x$ and index $i$ for the function $f_i$ (solely dependent on $r_1$ $r_2$). Because the Collection is secure and hard to invert, $g$ must therefore be a secure Strong OWF. 

Since Collection of OWF is more structurally restrictive than Strong OWF, the backward direction is easy to prove, (have only the given Strong OWF in the family).