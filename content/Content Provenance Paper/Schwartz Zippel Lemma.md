# Exact Definition
Let $f \in \mathbb{F}[X_1, \dots, X_\mu]$ be a non-zero polynomial of total degree $d$ over a finite field $\mathbb{F}$.
$S \subseteq \mathbb{F}$

$$\Pr[f(r_1, \dots, r_\mu) = 0] \le \frac{d}{|S|}$$

where $(r_1, \dots, r_\mu) \leftarrow random(S)$.

---
## Intuition
Polynomial $f$ of degree $d$ has d-many roots where $f$ evaluates to $0$. Hence Probability of that happening is $\dfrac{d}{|S|}$. (Recall PIT).