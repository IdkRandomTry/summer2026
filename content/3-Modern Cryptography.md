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

![[../Attachments/Pasted image 20260301221213.png]]

![[../Attachments/Pasted image 20260301221146.png]]

This is then followed both parties making a random cut. Note that a party may choose to disobey and do an intentional cut or no cut at all.

Final Output will be like:
![[../Attachments/Pasted image 20260301221727.png]]

And "cuts" basically do cyclic shifts, hence even with parties disobeying, no party can know the choice of the other party.
