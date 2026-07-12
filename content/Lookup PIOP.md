We define $R_{Lookup}$ on a Boolean Hypercube as follows:

$$
R_{Lookup} = \{((t, [[f]]); f) : f(b) \in t ~~ \forall b \in B_\mu ~~and~~ f \in \mathcal{F}_\mu^{\leq d}\}
$$

> [!Idea]
> The Lookup protocol ensures that all evaluations of $f$ over the Boolean hypercube $B_\mu$ are contained within a predefined public table vector $t$. Because HyperPlonk operates over the boolean hypercube the lookup argument works by using a *multiplicity polynomial* and reducing the subset relationship to a randomized fractional [[Sum Check PIOP]].

**NOTE:** This note describes LogUp, a more efficient method of lookup than the one mentioned in HyperPlonk.
# Protocol
Given a tuple $(x;w) = ((t, [[f]]); f)$:
- The prover constructs a multiplicity polynomial $m \in \mathcal{F}_\mu$ where $m(b)$ represents the exact number of times the public table entry $t(b)$ appears in the set $\{f(b')\}_{b' \in B_\mu}$. The prover sends the oracle $[[m]]$ to the verifier.
- The verifier samples a random challenge $\gamma \leftarrow \mathbb{F}$ and sends it to the prover.
- P and V define a relationship to prove that the evaluations of $f$ match the evaluations of $t$ weighted by $m$. Mathematically:
  $\sum_{b \in B_\mu} \frac{1}{\gamma + f(b)} = \sum_{b \in B_\mu} \frac{m(b)}{\gamma + t(b)}$
- - To avoid computing divisions inside the circuit, the prover modifies the evaluated fractions as two new auxiliary polynomials. P constructs $h_1, h_2 \in \mathcal{F}_\mu$ such that for all $b \in B_\mu$:
  $h_1(b) = \frac{1}{\gamma + f(b)}$
  $h_2(b) = \frac{m(b)}{\gamma + t(b)}$
- P sends the commitments/oracles $[[h_1]]$ and $[[h_2]]$ to the verifier.
- P and V run a [[Zero Check PIOP]] to verify these helper polynomials are strictly correct across the hypercube, successfully converting the division check into a multiplication check:
  $\hat{Z}_1(X) := h_1(X) \cdot (\gamma + f(X)) - 1 = 0$
  $\hat{Z}_2(X) := h_2(X) \cdot (\gamma + t(X)) - m(X) = 0$
- Finally, with the valid construction of $h_1$ and $h_2$ proven, P and V run a standard [[Sum Check PIOP]] on their difference to prove the grand total of the fractional identity perfectly balances:
  $\sum_{b \in B_\mu} (h_1(b) - h_2(b)) = 0$
# For intuition:
If every evaluation of $f$ is truly a valid entry in the public table $t$, we can perfectly map each $f(b)$ to a specific $t(b)$. The multiplicity polynomial $m(b)$ acts as a counter, tracking how many times each table entry was "looked up" during the circuit execution. 

By adding a random challenge $\gamma$ to the evaluations and taking their inverses, we create a distinct cryptographic fingerprint for both sets of data. If the subset relation holds and $m(b)$ was generated honestly, the grand sum of the inverses of the shifted $f(b)$ values will exactly equal the sum of the inverses of the shifted $t(b)$ values multiplied by their respective counts $m(b)$. 

# Complexity Analysis
Ref HyperPlonk.pdf pg 23 (Definition 3.6)
- Prover time: $\mathcal{O}(d \log^2 d \cdot 2^\mu)$ field operations.
- Verifier time: $\mathcal{O}(\mu)$.
- Query complexity: $\mathcal{O}(\mu)$ (inherited directly from the underlying SumCheck/ZeroCheck).
- Round complexity: $\mu + \mathcal{O}(1)$.
- Proof oracles size: $\mathcal{O}(2^\mu)$ (to commit to the multiplicity polynomial $m$ and auxiliary helper polynomials).
- Witness size: $\mathcal{O}(2^\mu)$.