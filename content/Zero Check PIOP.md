We define $R_{ZERO}$ on a Boolean Hypercube as follows:

$$R_{ZERO} = \{(([[f]]); f) : f(x) = 0 \, \forall x \in B_\mu ~~where~~ f \in \mathcal{F}_\mu^{\leq d}\}$$

> [!Idea]
> The ZeroCheck protocol is constructed by reducing the problem of checking if a multivariate polynomial evaluates to zero everywhere on the boolean hypercube to a standard [[Sum Check PIOP]]. It leverages a random verifier challenge and an equality polynomial ($eq$) to bundle all the hypercube evaluations into a single sum.

# Protocol
Uses [[Sum Check PIOP]]
Given a tuple $(x;w) = (([[f]]); f)$:
- The verifier sends the prover a random vector $r \leftarrow \mathbb{F}^\mu$.
- Define the polynomial $\hat{f}(X) := f(X) \cdot eq(X, r)$ where $eq(x,y) := \prod_{i=1}^\mu (x_i y_i + (1-x_i)(1-y_i))$.
- P and V run a [[Sum Check PIOP]] to convince the verifier that $((0, [[\hat{f}]]); \hat{f}) \in R_{SUM}$.

# For intuition
The core idea relies on the auxiliary polynomial $g(Y) := \sum_{x \in B_\mu} f(x) \cdot eq(x, Y)$. Because the $eq(x,y)$ function evaluates to $1$ when $x=y$ and $0$ otherwise on the boolean hypercube, $g(y)$ perfectly extracts $f(y)$. 
If the prover is dishonest and $f$ is not zero everywhere on $B_\mu$, then $g(Y)$ is a non-zero polynomial. By the [[Schwartz Zippel Lemma]], evaluating this non-zero polynomial $g$ at a completely random challenge point $r$ will yield a non-zero value with overwhelming probability. 
The underlying [[Sum Check PIOP]] computes the sum of $\hat{f}(x)$ over the hypercube, which is exactly $g(r) = \sum_{x \in B_\mu} f(x) \cdot eq(x, r)$. The verifier checks that this sum is exactly $0$, probabilistically guaranteeing $f(x)=0$ across the entire hypercube.

Complexity Analysis
Ref HyperPlonk.pdf pg 20
- Prover time: $\mathcal{O}(d \log^2 d \cdot 2^\mu)$ field operations (the same time required to run the SumCheck on $\hat{f}$).
- Verifier time: $\mathcal{O}(\mu)$.
- Query complexity: $\mu+1$ (inherited directly from the underlying SumCheck).
- Round complexity: $\mu$.
- Proof oracles size: $d\mu$.
- Witness size: $\mathcal{O}(2^\mu)$.
