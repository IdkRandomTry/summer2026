# Requirements
Modern Cryptography requires:
- Definitions of Security
- Precise Assumptions - Axioms, Mathematically hard computation
- Proof of Security, under these assumptions
We also expand from communication to joint computations and more
# Secure 2-party computation

> [!idea]
> Consider a situation where Alice and Bob have private information $a$ and $b$ and wish to compute $f(a,b)$. However they don't trust each other and wish to maintain both privacy (of info $a$ and $b$) and also ensure correctness of $f(a,b)$. We must also consider the situations where Alice and Bob my deviate in malicious ways from the agreed computations.
> 
> Searching a protocol for such computations is the way to secure 2-party computation

Explanation through example: Matchmaking Game

![[Pastedimage20260301221213.png]]

![[Pastedimage20260301221146.png]]

This is then followed both parties making a random cut. Note that a party may choose to disobey and do an intentional cut or no cut at all.

Final Output will be like:
![[Pastedimage20260301221727.png]]

And "cuts" basically do cyclic shifts, hence even with parties disobeying, no party can know the choice of the other party.

# Secrecy
To Formalize secrecy, we define:
1) [[3.1-Shannon's Secrecy]]
2) [[3.2-Perfect Secrecy]]

## Shannon's Secrecy $\Leftrightarrow$ Perfectly Secret
Perfect security is syntactically simpler than Shannon security, and thus easier to work with. Fortunately,  Shannon Secrecy and Perfect Secrecy are equivalent notions !
###### Theorem: A private-key encryption scheme is perfectly secret if and only if it is Shannon secret.
Proof. \[Done in Notebook, page 1]

An example of Perfect Secrecy is [[3.3-One Time Pad]]
## Key Size for Shannon's Secret
###### Theorem: If scheme (M, K, Gen, Enc, Dec) is a perfectly secret private-key encryption scheme, then |K| ≥ |M|
Proof. \[Notebook page 2]

> [!note] Explicit attack for |K| < |M|: 
> For any key k, there is pair {m1,m2} such that for c <- Enc_k(m1), m2 not in DEC(c)
> For scenario where alice pick randomly from m1 m2, we define attack as:
> - if m2 in DEC(c) -> guess randomly
> - if m2 not in DEC(c) -> guess m1
> this gives success of 1/2 + eps/4 where eps is the proportion of keys where m2 not in DEC(c)

> [!note] What if eps is small?
> Shannon showed that with |K| = |M|-1,
> eps is 1/2 which gives success prob of 5/8
> We also show:
> 
> $$\Pr \left[ k \leftarrow \mathcal{K}; \text{Enc}_k(m_1) = c : m_2 \in \mathbf{Dec}(c) \right] \le \epsilon$$

> PREVIOUS: [[2-Private Key Encryption]]
> NEXT: [[4-Efficient Computing]]

