# **Exact Definition:**
For every function $f: \mathbb{B}_\mu \to \mathbb{F}$, there is a unique multilinear polynomial $\hat{f} \in \mathbb{F}[X_1, \dots, X_\mu]$ such that $\hat{f}(\mathbf{b}) = f(\mathbf{b})$ for all $\mathbf{b} \in \mathbb{B}_\mu$. We call $\hat{f}$ the multilinear extension of $f$, and $\hat{f}$ can be expressed as:

$$\hat{f}(\mathbf{X}) = \sum_{\mathbf{b} \in \mathbb{B}_\mu} f(\mathbf{b}) \cdot eq(\mathbf{b}, \mathbf{X})$$

$$\text{where }eq(\mathbf{b}, \mathbf{X}) := \prod_{i=1}^\mu (\mathbf{b}_i \cdot X_i + (1 - \mathbf{b}_i)(1 - X_i))$$
# Intuition
An MLE $\hat{f}$ mathematically "upgrades" a discrete boolean function $f$ into a continuous algebraic polynomial over a larger field $\mathbb{F}$. 

*  $\hat{f}$ perfectly mimics the original function $f$ on valid boolean inputs (the domain of $f$), but behaves arbitrarily for all other inputs in the field.
* No individual variable of $\hat{f}$ has a degree greater than 1.

# Properties
- Each term in $eq(b,X)$ has degree 1. $\mu$ many such terms are multiplied together. Hence the degree of the $eq(b,X)$ is $\mu$. 
- Since $\hat{f}(X)$ is just a weighted sum of $eq(b,X)$, it degree is also $\mu$

This unlocks [[Schwartz Zippel Lemma]] and as $d = \mu$ for MLE,
$$Pr[\hat{f}(X) = 0] = \dfrac{\mu}{|\mathbb{F}|}$$
