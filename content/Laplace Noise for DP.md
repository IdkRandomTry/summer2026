# Laplacian Noise guarantees DP
Adding Noise sampled from a particular Laplace Distribution to a dataset guarantees $\epsilon$-[[Differential Privacy (DP)]] for its elements.
## Global Sensitivity
The global sensitivity of a query function $f$, denoted as $\Delta f$, is the maximum amount that the function's output can change when applied to any two neighboring datasets (datasets differing by at most one element). It represents the maximum possible influence a single individual's data can have on the output.

Mathematically, for any two neighboring datasets $x$ and $x'$, the $L_1$ global sensitivity is defined as:

$$\Delta f = \max_{x, x'} ||f(x) - f(x')||_1$$
## Statement
For any given function $f : \mathcal{D} \rightarrow \mathbb{R}^k$ mapping a dataset to a vector of $k$ real numbers, the Laplace Mechanism $\mathcal{M}_L$ is defined as $\text{San}_f(x)$:

$$\mathcal{M}_L(x, f, \epsilon) = f(x) + (Y_1, Y_2, \dots, Y_k)$$

where each $Y_i$ is a random variable independently drawn from a Laplace distribution with mean $0$ and scale $\frac{\Delta f}{\epsilon}$ (i.e., $Y_i \sim \text{Lap}(\frac{\Delta f}{\epsilon})$). 

**Theorem:** The mechanism $\mathcal{M}_L(x, f, \epsilon)$ satisfies $\epsilon$-differential privacy.
## Proof
*(Theorem 1: Calibrating Noise to Sensitivity in Private Data Analysis \[Paper: [[Laplace for DP.pdf]]\]* )
### The Core Setup
Suppose a user makes a sequence of queries, resulting in a transcript of answers $t = [t_1, t_2, \dots, t_d]$.
For a database $x$, the true answer to the $i$-th query is $f_t(x)_i$.
To protect privacy, the mechanism adds random noise $y$ drawn from a Laplace distribution to the true answer. Let this be denoted by
$$t_i = f_t(x)_i + y$$
Therefore, the exact amount of noise required to produce a specific, published answer $t_i$ is $y = t_i - f_t(x)_i$. 

### The Laplace Distribution
The Laplace distribution with scale parameter $\lambda$ has a probability density function proportional to the exponential of the absolute value of the noise divided by $\lambda$:
$$h(y) \propto \exp\left(\frac{-|y|}{\lambda}\right)$$
By substituting our required noise $y$, the probability of the mechanism returning $t_i$ given database $x$ is:
$$\Pr[\text{San}_f(x)_i = t_i] \propto \exp\left(\frac{-|t_i - f_t(x)_i|}{\lambda}\right)$$

### Bounding the Individual Query
Differential privacy requires the ratio of probabilities between two neighboring databases (differing by one row), $x$ and $x'$, to be bounded. We set up the ratio of the probability for $x$ by the probability for $x'$:
$$\frac{\Pr[\text{San}_f(x)_i = t_i]}{\Pr[\text{San}_f(x')_i = t_i]} = \frac{\exp\left(\frac{-|t_i - f_t(x)_i|}{\lambda}\right)}{\exp\left(\frac{-|t_i - f_t(x')_i|}{\lambda}\right)}$$

Using standard exponent rules ($\frac{e^A}{e^B} = e^{A-B}$), we combine them:
$$= \exp\left(\frac{-|t_i - f_t(x)_i| + |t_i - f_t(x')_i|}{\lambda}\right)$$

Next, we simplify the numerator using a variation of the triangle inequality ($|A| - |B| \leq |A - B|$). Let $A = t_i - f_t(x')_i$ and $B = t_i - f_t(x)_i$:
$$|t_i - f_t(x')_i| - |t_i - f_t(x)_i| \leq |(t_i - f_t(x')_i) - (t_i - f_t(x)_i)|$$

The $t_i$ terms perfectly cancel each other out:
$$= |-f_t(x')_i + f_t(x)_i| = |f_t(x)_i - f_t(x')_i|$$

Substituting this simplified numerator back gives the bound for a single query:
$$\leq \exp\left(\frac{|f_t(x)_i - f_t(x')_i|}{\lambda}\right)$$

### Bounding the Entire Transcript
Because queries can be adaptive, the proof uses the law of conditional probability to evaluate the entire sequence. We multiply the bounded conditional probabilities for every query $i$ in the transcript:

$$\frac{\Pr[\text{San}_f(x) = t]}{\Pr[\text{San}_f(x') = t]} = \prod_i \frac{\Pr[\text{San}_f(x)_i = t_i | t_1, \dots, t_{i-1}]}{\Pr[\text{San}_f(x')_i = t_i | t_1, \dots, t_{i-1}]}$$

Substituting our single-query bound from [[#Bounding the Individual Query]]:

$$\leq \prod_i \exp\left(\frac{|f_t(x)_i - f_t(x')_i|}{\lambda}\right)$$

When multiplying exponents with the same base, you add the powers. The sum of the absolute differences across all $i$ queries is simply the $L_1$ norm:

$$= \exp\left(\frac{\|f_t(x) - f_t(x')\|_1}{\lambda}\right)$$

### Applying the Sensitivity ($S(f)$ aka $\Delta f$)
By definition, the maximum amount the sum of outputs can change between two neighboring databases is the sensitivity $S(f_t)$. Therefore:

$$\|f_t(x) - f_t(x')\|_1 \leq S(f_t)$$

If we deliberately calibrate our Laplace noise scale parameter $\lambda$ so that it matches the ratio of sensitivity to our privacy budget ($\lambda = \frac{S(f_t)}{\epsilon}$), we can substitute both the sensitivity and lambda into our exponent:

$$\leq \exp\left(\frac{S(f_t)}{S(f_t)/\epsilon}\right) = \exp(\epsilon)$$

This mathematically proves that the mechanism is $\epsilon$-indistinguishable (meaning it satisfies strict $\epsilon$-Differential Privacy).