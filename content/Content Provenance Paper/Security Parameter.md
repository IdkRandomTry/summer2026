# Security Parameter ($\lambda$)
The foundational "dial" in cryptography used to measure and tune the computational complexity of both operating and breaking a cryptographic scheme. 

We analyze as $\lambda \to \infty$ to separate honest users from attackers based on complexity theory:
- **Honest Parties:** The time required to encrypt, decrypt, or sign must scale polynomial-ly ($poly(\lambda$)
- **Adversary:** The probability that a computationally bounded (polynomial-time) attacker can break the scheme must be negligible ( [[5-One Way Functions#^59b5e7]] ) i.e. $\dfrac{1}{poly(\lambda)}$ 