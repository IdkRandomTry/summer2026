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

# The Algorithm
* Input: An array of probabilities $P = [p_0, p_1, \dots, p_{N-1}]$ where $\sum P = 1.0$, and the length $N$.
* Output: 
	- Threshold: An array of length $N$ (floats between $0.0$ and $1.0$).
	- Alias: An array of length $N$ (integers representing indices).

```
Algorithm Build_Buckets(Items, N):
	Define Bucket_Capacity = 1.0 / N
    POOR_ITEMS = []
    RICH_ITEMS = []
       
	For each item in Items:
		If item.Probability < Bucket_Capacity:
			Add item to POOR_ITEMS
		Else:
			Add item to RICH_ITEMS
		   
    BUCKETS = []
    
    // The Robin Hood Loop: Take from the rich to top off the poor
    While POOR_ITEMS is not empty AND RICH_ITEMS is not empty:
		poor = POOR_ITEMS.pop()
		rich = RICH_ITEMS.pop()
           
		// Create a new bucket and give the poor item its space
		new_bucket = Bucket()
		new_bucket.Primary_Item = poor.Name
		new_bucket.Threshold = poor.Probability
		
		// The rich item fills the exact remaining void in the bucket
		new_bucket.Alias_Item = rich.Name
		Space_To_Fill = Bucket_Capacity - new_bucket.Threshold
		rich.Probability = rich.Probability - Space_To_Fill
		
		BUCKETS.add(new_bucket)
		
		// Re-evaluate where the rich item belongs now.
		If rich.Probability < Bucket_Capacity:
			POOR_ITEMS.add(rich)
		Else:
			RICH_ITEMS.add(rich)
        
        // Handle RICH_ITEMS with 1/N probability
		While RICH_ITEMS is not empty:
			leftover = RICH_ITEMS.pop(item.Probability = Bucket_Capacity)
			new_bucket = Bucket()
			new_bucket.Primary_Item = leftover.Name
			new_bucket.Alias_Item = NONE
			new_bucket.Threshold = Bucket_Capacity
			BUCKETS.add(new_bucket)
		
	Return BUCKETS
```
