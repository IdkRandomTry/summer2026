---
draft: true
---

# Isolated sampling. 

[[DP Pixelation Provenance#Proposed Method 2]] had an interesting outlook for sampling, which was to construct a large table $t$ which mimicked a probability distribution, permute it / scramble it in a **reliable way** to generate a private table $t'$ of the probability distribution. Following this, use public randomness $r_i$ to sample $t'[r_i]$: a private value which is guaranteed to follow the discrete sampling guarantees. 

It is important to avoid the pitfall of showing $(t,t') \in \text{Multiset Equality}$ as although it shows $t'$ is a valid table for discrete sampling from intended distribution, it does not verify if $t'$ is shuffled properly. Another trap to avoid is giving full control of the permutation used (which is private). The permutation must be generated based on some randomness. It is also important that prover is not entrusted with the entire seed of randomness to prevent seed grinding attack. After dodging all this, we must show that for $((t,[[t']],[[\sigma]]); t', \sigma)$, $t' = \\sigma(t)$.

Note that this particular method, even if made feasible, is not practical in the current scope due to the large table size, but can be of independent interest?