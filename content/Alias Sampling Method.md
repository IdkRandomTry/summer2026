![[Pasted image 20260710144538.png]]
The algorithm allows discrete sampling with reasonable table size and incredible precision.

> [!idea]
> Consider you have $N$ possible outcomes, each with a different probability. If you represent these probabilities as bars on a chart, they have different heights. The average height is exactly $1/N$.
> The Alias Method flattens this chart by taking from the "rich" (bars taller than $1/N$) and giving to the "poor" (bars shorter than $1/N$).  You can always perfectly pack the probabilities into exactly $N$ equal-sized buckets, such that **no bucket contains more than two items**.

# Setup
The setup is a static table with $N$ rows (one for each bucket). Each row contains three values:
1. **Primary ($P_i$):** The original item assigned to this bucket.
2. **Alias ($A_i$):** The alternate item used to fill up this bucket.
3. **Threshold ($T_i$):** The probability of choosing $P_i$.
# Sampling
To draw a single random sample, you only need two sources of randomness:
1. Generate a random integer $i \in [0, N-1]$ - this chooses the bucket
2. Generate a random float $U \in [0, 1)$ - this is then compared with the threshold:
   - If $U < T_i$, return the Primary item ($P_i$). 
   - If $U \ge T_i$, return the Alias item ($A_i$).
