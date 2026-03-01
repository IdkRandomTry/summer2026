Prereq: [[1-Introduction and Definitions]]
# Idea
We assume that only Key is private. 
`GEN`, `ENC`, `DEC` is public i.e. known to [[1-Introduction and Definitions#Adversaries|adversaries]].

# Formally
refer [[1-Introduction and Definitions|definitions]] wherever needed

$(\mathcal{M}, \mathcal{K}, GEN, ENC, DEC)$ is a private-key encryption scheme if
1) `GEN` is a randomized algorithm that returns a key k such that $k \in \mathcal{K}$. 
   $k \leftarrow GEN$ the process of generating a key k.
2) `ENC` is a potentially randomized algorithm that on input a key $k \in \mathcal{K}$ and a message $m \in \mathcal{M}$, outputs a ciphertext c. 
   We denote by $c \leftarrow Enc_k(m)$ 
3) `DEC` is a deterministic algorithm that on input a key k and a ciphertext c outputs a message $m \in \mathcal{M}~ \cup \perp$  where $\perp$ represents an error/invalid ciphertext
4) For all $m$, for all $k$, $Dec_k(Enc_k(m)) = m$
