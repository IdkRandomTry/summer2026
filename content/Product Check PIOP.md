We define $R_{PROD}$ on a Boolean Hypercube as follows:

$$R_{PROD} = \{((s, [[f_1]], [[f_2]]); f_1, f_2) : \prod_{x \in B_\mu} f'(x) = s \}$$
$$~~where~~ f' = f_1/f_2 ~~and~~ f_1, f_2 \in \mathcal{F}_\mu^{\leq d} ~~and~~ f_2(b) \neq 0 \, \forall b \in B_\mu\}
$$

(Note: In the case that $f_2 = c$ is a constant polynomial, we directly set $f := f_1/c$ and write $(x;w) = ((s, [[f]]); f)$)

> [!Idea]
> The ProductCheck protocol is constructed using an instance of the [[Zero Check PIOP]] PolyIOP extended to $\mu+1$ variables. The prover constructs an auxiliary polynomial $\tilde{v}$ that acts as an accumulator for the product of $f'(x)$ evaluations. The verifier then relies on the ZeroCheck to ensure the correctness of $\tilde{v}$ and queries a single specific point to verify that the accumulated total product matches $s$.

# Protocol
uses [[Zero Check PIOP]]
- The prover sends an oracle $\tilde{v} \in \mathbb{F}_{\mu+1}^{\leq 1}$ such that for all $x \in B_\mu$, $\tilde{v}(0,x) = f'(x)$ and $\tilde{v}(1,x) = \tilde{v}(x, 0) \cdot \tilde{v}(x, 1)$.
- Define $\hat{h} := \text{merge}(\hat{f}, \hat{g}) \in \mathbb{F}_{\mu+1}^{\leq \max(2, d+1)}$ where $\hat{f}(X) := \tilde{v}(1,X) - \tilde{v}(X, 0) \cdot \tilde{v}(X, 1)$, and $\hat{g}(X) := f_2(X) \cdot \tilde{v}(0,X) - f_1(X)$. 
- P and V run a ZeroCheck PolyIOP for $([[\hat{h}]]; \hat{h}) \in R_{ZERO}$ to prove that the polynomial $\tilde{v}$ is computed correctly.
- V queries $[[\tilde{v}]]$ at the point $(1, \dots, 1, 0) \in \mathbb{F}^{\mu+1}$, and checks that the evaluation is exactly $s$.

# For intuition:
The protocol extends the $\mu$-variable hypercube to $\mu+1$ dimensions using the virtual polynomial $\tilde{v}$. 
The definition $\tilde{v}(0,x) = f'(x)$ embeds the base evaluations of the rational polynomial $f'$ at the $0$ level of the new variable. The definition $\tilde{v}(1,x) = \tilde{v}(x, 0) \cdot \tilde{v}(x, 1)$ acts as a recursive step to multiply pairs of evaluations together, accumulating the product at the $1$ level of the new dimension.

The ZeroCheck on $\hat{h}$ provides a way to simultaneously enforce both of these structural constraints across the entire hypercube (making sure $\tilde{v}$ correctly represents $f_1/f_2$ without division by zero errors, and correctly computes the recursive products).

Finally, if $\tilde{v}$ is honestly generated, the overall product of all evaluations on $B_\mu$ folds down into the single point $\tilde{v}(1, \dots, 1, 0)$. Because of this, the verifier only has to make one query to confirm the total product equals $s$.

Complexity Analysis
Ref [[HyperPlonk]] pg 21
- Prover time: $\mathcal{O}(d \log^2 d \cdot 2^\mu)$ field operations (the same time required to run the underlying ZeroCheck, plus $\mathcal{O}(2^\mu)$ to compute the product polynomial $\tilde{v}$). 
- Verifier time: $\mathcal{O}(\mu)$
- Query complexity: $\mu+2$ (the $\mu+1$ queries inherited from the ZeroCheck, plus the $1$ additional query for $\tilde{v}(1, \dots, 1, 0)$)
- Round complexity: $\mu+1$ 
- Proof oracles size: $\mathcal{O}(2^\mu)$ 
- Witness size: $\mathcal{O}(2^\mu)$