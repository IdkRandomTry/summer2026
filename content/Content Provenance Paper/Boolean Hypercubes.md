# Boolean Hypercube ($Q_n$)

$Q_n = (V, E)$ where the vertex set $V = \{0, 1\}^n$ represents all binary strings of length $n$. 
- **Size:** $|V| = 2^n$ vertices.
- **E:** Two vertices $u, v \in \{0, 1\}^n$ are adjacent iff they differ in exactly one coordinate (Hamming distance $d_H(u, v) = 1$).
- **Degree:** $Q_n$ is $n$-regular (every vertex has exactly $n$ neighbors, corresponding to flipping one of its $n$ bits).
- **Total Edges:** $|E| = \frac{n \cdot 2^n}{2} = n 2^{n-1}$.

![[Pasted image 20260525155729.png]]
