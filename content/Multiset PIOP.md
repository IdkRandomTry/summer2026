We define $R_{MSET}$ on a Boolean Hypercube as follows:

$$
R_{MSET} = \{(([[f]], [[g]]); f, g) : \{f(x)\}_{x \in B_\mu} = \{g(x)\}_{x \in B_\mu} ~~and~~ f, g \in \mathcal{F}_\mu^{\leq d}\}
$$

> [!Idea]
> The Multiset Check protocol verifies that two polynomials $f$ and $g$ evaluate to the exact same multiset of values over the Boolean hypercube $B_\mu$. This is reduced to a [[Product Check PIOP]] by using a random challenge $r$ to shift the evaluations. According to the [[Schwartz Zippel Lemma]], if the multisets of evaluations are equal, the product of their shifted evaluations will be exactly equal, meaning their overall ratio evaluates to $1$.

# Protocol
Given a tuple $(x;w) = (([[f]], [[g]]); f, g)$:
- The verifier samples a random challenge $r \leftarrow \mathbb{F}$ and sends $r$ to the prover.
- P and V define new virtual polynomials $f'(X) := r + f(X)$ and $g'(X) := r + g(X)$.
- P and V run a [[Product Check PIOP]] to convince the verifier that the ratio of the products of these shifted polynomials over the hypercube equals $1$. Specifically, they check $((1, [[f']], [[g']]); f', g') \in R_{PROD}$.

## For intuition:
The protocol relies on the algebraic fact that if two multisets $A$ and $B$ are equal, the polynomials $P_A(Y) = \prod_{a \in A}(Y + a)$ and $P_B(Y) = \prod_{b \in B}(Y + b)$ are identical polynomials in $Y$. 

By choosing a random challenge $r$ from a large finite field, evaluating $P_A(r)$ and $P_B(r)$ allows the verifier to check if $A = B$ with high probability (if the sets were different, the polynomials would disagree at a random point). 

By defining $f'(X) = r + f(X)$ and $g'(X) = r + g(X)$, taking their product over the hypercube exactly mimics evaluating these characteristic polynomials at the challenge point $r$. The underlying ProductCheck seamlessly verifies that $\prod_{x \in B_\mu} f'(x) / \prod_{x \in B_\mu} g'(x) = 1$, effectively confirming that both sets of evaluations are identical up to permutation.

# Complexity Analysis
Ref [[HyperPlonk.pdf]] pg 22
- Prover time: $\mathcal{O}(d \log^2 d \cdot 2^\mu)$ field operations (the same time required to run the underlying ProductCheck).
- Verifier time: $\mathcal{O}(\mu)$.
- Query complexity: $\mu+2$ (inherited directly from the underlying ProductCheck on $f'/g'$).
- Round complexity: $\mu+1$.
- Proof oracles size: $\mathcal{O}(2^\mu)$.
- Witness size: $\mathcal{O}(2^\mu)$.