We define $R_{PROD}$ on a Boolean Hypercube as follows:

$$R_{PROD} = \{((s, [[f_1]], [[f_2]]); f_1, f_2) : \prod_{x \in B_\mu} f'(x) = s \}$$
$$~~where~~ f' = f_1/f_2 ~~and~~ f_1, f_2 \in \mathcal{F}_\mu^{\leq d} ~~and~~ f_2(b) \neq 0 \, \forall b \in B_\mu\}
$$

(Note: In the case that $f_2 = c$ is a constant polynomial, we directly set $f := f_1/c$ and write $(x;w) = ((s, [[f]]); f)$)

> [!Idea]
> The ProductCheck protocol is designed by constructing a "magic" polynomial $\tilde{v}$ that has a recursive definition. It acts as an accumulator for the product of $f'(x)$ evaluations. The verifier then relies on the [[Zero Check PIOP]] to ensure the correctness of $\tilde{v}$ and queries a single specific point to verify that the accumulated total product matches $s$.
# Protocol
uses [[Zero Check PIOP]]
- The prover sends an oracle $\tilde{v} \in \mathbb{F}_{\mu+1}^{\leq 1}$ such that for all $x \in B_\mu$, $\tilde{v}(0,x) = f'(x)$ and $\tilde{v}(1,x) = \tilde{v}(x, 0) \cdot \tilde{v}(x, 1)$.
- Define $\hat{h} := \text{merge}(\hat{f}, \hat{g}) \in \mathbb{F}_{\mu+1}^{\leq \max(2, d+1)}$ where $\hat{f}(X) := \tilde{v}(1,X) - \tilde{v}(X, 0) \cdot \tilde{v}(X, 1)$, and $\hat{g}(X) := f_2(X) \cdot \tilde{v}(0,X) - f_1(X)$. 
- P and V run a [[Zero Check PIOP]] for $([[\hat{h}]]; \hat{h}) \in R_{ZERO}$ to prove that the polynomial $\tilde{v}$ is computed correctly.
- V queries $[[\tilde{v}]]$ at the point $(1, \dots, 1, 0) \in \mathbb{F}^{\mu+1}$, and checks that the evaluation is exactly $s$.
# For intuition:
## Intuition for $\tilde{v}$
The protocol extends the $\mu$-variable hypercube to $\mu+1$ dimensions using the virtual polynomial $\tilde{v}$. Its definition is recursive. The definition $\tilde{v}(0,x) = f'(x)$ can be considered as the base case. The definition $\tilde{v}(1,x) = \tilde{v}(x, 0) \cdot \tilde{v}(x, 1)$ acts as a recursive step to multiply pairs of evaluations together, accumulating the product at the $1$ level of the new dimension. The idea is that when $\tilde{v}$ is evaluated at $(1^\mu,0)$ it will return the required product i.e. $\prod_{x \in B_\mu} f'(x)$. 
> [!idea] Example
> Consider $\tilde{v}(1,0)$. Its evaluation is $\tilde{v}(0, 0) \cdot \tilde{v}(0, 1)$ which by definition is $f'(0) \cdot f'(1)$. Similarly $\tilde{v}(110)$'s evaluation is $\tilde{v}(0, 00) \cdot \tilde{v}(0, 01) \cdot \tilde{v}(0, 10) \cdot \tilde{v}(0, 11)$, which is $f'(00) \cdot f'(01) \cdot f'(10) \cdot f'(11)$. This extends to $\tilde{v}(1^\mu,0)$.
## Intuition for $\hat{h}$
The merge looks like $\hat{h(X_0...X_\mu)} = (1-X_0)\hat{f}(X_1...X_\mu) + X_0\hat{g}(X_1...X_\mu)$. On the boolean hypercube, $\hat{h}$ mimics $\hat{f}$ when $X_0=0$ and $\hat{g}$ when $X_0=1$. When we verify if $\hat{h}$ is identically zero, we essentially confirm if $\hat{f}=0$ and $\hat{g}=0$ 
## Intuition for $\hat{f}$ and $\hat{g}$
Notice carefully that $\hat{f} = 0$ and $\hat{g} = 0$ ensure that the committed $\tilde{v}$ is generated correctly.

Finally we evaluate $\tilde{v}$ at $1^\mu0$ and check if it equals s.

# Complexity Analysis
Ref [[HyperPlonk]] pg 21
- Prover time: $\mathcal{O}(d \log^2 d \cdot 2^\mu)$ field operations (the same time required to run the underlying ZeroCheck, plus $\mathcal{O}(2^\mu)$ to compute the product polynomial $\tilde{v}$). 
- Verifier time: $\mathcal{O}(\mu)$
- Query complexity: $\mu+2$ (the $\mu+1$ queries inherited from the ZeroCheck, plus the $1$ additional query for $\tilde{v}(1, \dots, 1, 0)$)
- Round complexity: $\mu+1$ 
- Proof oracles size: $\mathcal{O}(2^\mu)$ 
- Witness size: $\mathcal{O}(2^\mu)$