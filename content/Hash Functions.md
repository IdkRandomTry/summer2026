# Hash Family
A hash function $\mathcal{H} = (\mathsf{KGen}, \mathsf{Eval})$ is defined for a keyspace $\mathcal{K}$, a message space $\mathcal{M}$, and range $\mathcal{Y}$. The two algorithms of $\mathcal{H}$ are defined as follows:
* $k \leftarrow \mathsf{KGen}(1^\lambda)$: $\lambda$ is the [[Security Parameter]],  output key $k \in \mathcal{K}$.
* $y \leftarrow \mathsf{Eval}(k, m)$: input key $k \in \mathcal{K}$ and message $m \in \mathcal{M}$, and outputs a hash $y \in \mathcal{Y}$.

Note that $\mathsf{Eval}(k, \cdot)$ must be efficiently computable for all $k \in \mathcal{K}$ and  $|\mathcal{Y}| \ll |\mathcal{M}|$. 

# Low-Norm Collision-Resistant Hash Family
For some integer $B > 0$, a hash function $\mathcal{H}$ is $B$-norm collision resistant if for every $\mathsf{PPT}$ adversary $\mathcal{A}$, there exists a negligible function $\epsilon$ such that for all $\lambda \in \mathbb{N}$, we have that:

$$\Pr \left[ \begin{array}{c} m_1 \neq m_2 \land \\ \mathsf{H}(\mathsf{k}, m_1) = \mathsf{H}(\mathsf{k}, m_2) \land \\ ||m_1||_\infty \le B \land ||m_2||_\infty \le B \end{array} \;\middle|\; \begin{array}{c} \mathsf{k} \xleftarrow{\$} \mathcal{H}.\mathsf{KGen}(1^\lambda) \\ (m_1, m_2) \leftarrow \mathcal{A}(\mathsf{k}) \end{array} \right] \le \epsilon(\lambda) .$$
> [!note] Infinity Norm
> $||X||_\infty$ is the largest element in vector $X$

Ref for PPT: [[Probabilistic Poly-time Turing Machines]]
## Intuition
This is the strong collision resistance (birthday problem): the adversary $\mathcal{A}$ wins if it finds *any* two different messages ($m_1 \neq m_2$) that produce the exact same hash output. There is an extra restriction of low norm: To win the game, the adversary must find a collision where both messages are "small".  $||\cdot||_\infty \le B$ means that the absolute value of the largest single coordinate inside the vector $m_1$ (and $m_2$) cannot exceed the boundary $B$. If the adversary finds a collision using massive numbers, it doesn't count as a break.

This is closely related to [[Short Integer Solution Problem]]
