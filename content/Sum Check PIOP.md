We define $R_{SUM}$ on a [[Boolean Hypercubes]] as follows:

$$R_{SUM} = \{((v,[[f]]);f) : \sum_{b \in B_\mu}f(b) = v ~~and~~f\in \mathcal{F}_\mu^{\leq d}\}$$
> [!idea]
> the protocol runs in $\mu$ rounds where in every round, the prover sends a univariate polynomial of degree at most d to the verifier. The verifier then sends a random challenge point for the univariate polynomial. At the end of the protocol, the verifier checks the consistency between the univariate polynomials and the multi-variate polynomial using a single query to $f$.

# Protocol
- For $i = \mu, \mu - 1, \dots, 1$:
    * The prover computes $r_i(X) := \sum_{\boldsymbol{b} \in B_{i-1}} f(\boldsymbol{b}, X, \alpha_{i+1}, \dots, \alpha_\mu)$ and sends the oracle $[[r_i]]$ to the verifier. $r_i$ is univariate and of degree at most $d$.
    * The verifier checks that $v = r_i(0) + r_i(1)$, samples $\alpha_i \leftarrow \mathbb{F}$, sends $\alpha_i$ to the prover, and sets $v \leftarrow r_i(\alpha_i)$.

* Finally, the verifier accepts if $f(\alpha_1, \dots, \alpha_\mu) = v$.

# For intuition 
$r_\mu(X):= \sum_{\boldsymbol{b} \in B_{\mu-1}} f(\boldsymbol{b}, X)$ and the verifier checks if $r_i(0) + r_i(1)$ is equal to the given $v$. Then it chooses a challenge $\alpha_\mu$ and sends to prover. Note that this is chosen from a larger field (not necessarily 0,1). Now the prover can form $r_{\mu-1}(X) := \sum_{\boldsymbol{b} \in B_{\mu-2}} f(\boldsymbol{b}, X, \alpha_{\mu})$. Notice how the last bit from $f$ is set to $\alpha_\mu$. 

If the prover is honest, $r_{\mu-1}(0) + r_{\mu-1}(1) =r_\mu(\alpha_\mu)$. So we set $v \leftarrow r_i(\alpha_i)$ which signifies the sum to expect in the next iteration.

This protocol is repeated till all the bits are set to random challenges and the sum of the generated univariate polynomial is checked at each step. 

# Complexity Analysis
*Ref [[HyperPlonk.pdf]] pg 19*