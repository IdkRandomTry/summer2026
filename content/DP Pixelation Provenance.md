# Motivation
Efficient and scalable systems of Image Provenance which treat editors as untrusted entities such as [[VerITAS.pdf]] and [[HyperVerITAS]] currently publish and use the transformation/edit made on the image to prove its authenticity/source. This can be a security risk when the transformation contains sensitive information. An example of this is [[DP Pixelation]], where the original image is pixelated and noise is added to it to ensure differential privacy to protect sensitive parts of the original image. The noise which is added needs to be private to maintain the privacy guarantees. Hence we find motivation to construct a proof-system which enables image provenance to verify source of pixelated images without revealing the noise added to maintain [[Differential Privacy (DP)]].
# DP-Pix Recap
Traditional [[Blurring]] methods suffer from efficient deblurring attacks revealing private sensitive information. To combat this, [[DP Pixelation]] proposes adding noise sampled from a Laplace distribution with mean 0 and scale $\frac{255m}{b^2\epsilon}$ to guarantee $\epsilon-$Differential Privacy for features which take $\leq m-$pixels in the original image. $b$ is block size used for pixelation.

[[DP Pixelation]] is a natural candidate to extend [[HyperVerITAS]] proof system to as it can be represented as an linear transformation with ease. $I_{pix} = LI+E$ where $L$ is responsible for local averaging and $E$ is responsible for Laplacian noise and rounding error.  We attempt to hide $E$ to prevent noise from leaking, and provide a zero-knowledge proof the well-formedness of $E$

# Relation
The broad level relation we wish to identify is as follows:

$R(pp, vk, A)$ is parameterized by the public parameters $pp$, the verification key $vk$ of the digital signature scheme $\Sigma$, and a hash function $A$. It is defined as follows:

$$
R = \left\{ ((\mathbf{I}_T, \text{com}_{\mathbf{I}}, \mathbf{H}, \sigma, \mathbf{T}, \mathbf{A}); \mathbf{I}) : 
\begin{aligned}
&\text{com}_{\mathbf{I}} = \text{Commit}(\text{pp}, \mathbf{I}) \\
&\land \text{I}_T = T(\mathbf{I}) \\
&\land \mathbf{H} = \mathbf{A} \odot  \mathbf{I} ~~\land~~ \mathbf{I} \in [255]^{n\times3}\\
&\land \text{Vrfy}(vk, \mathbf{H}, \sigma) = 1
\end{aligned} \right\}
$$

## Blurring Transformation
To particularly prove the correctness of the DP-Pixelation transformation we define the following relation:

$$
R_{\text{dp-pix}} = \left\{ ((\mathbf{I}_T, \text{com}_{\mathbf{I}}, \mathbf{L}, \mathbf{R}); \mathbf{I}, \mathbf{E}) : 
\begin{aligned}
&\text{com}_{\mathbf{I}} = \text{Commit}(\text{pp}, \mathbf{I}) \\
&\land \text{com}_{\mathbf{E_1}} = \text{Commit}(\text{pp}, \mathbf{E_1}) \\
&\land \text{com}_{\mathbf{E_2}} = \text{Commit}(\text{pp}, \mathbf{E_2}) \\
&\land \text{com}_{\mathbf{E}} = \text{Commit}(\text{pp}, \mathbf{E}) \\
&\land \mathbf{I} = \mathbf{L} \odot \mathbf{I} \odot \mathbf{R} + \mathbf{E}\\
&\land \mathbf{E} = \mathbf{E_1} + \mathbf{E_2}\\
&\land \mathbf{E_1} \leftarrow \text{Laplace}(0,\frac{255m}{b^2\epsilon})\\
&\land \mathbf{E_2} \in [-0.5, 0.5])
\end{aligned} \right\}
$$

### Secret Affine Transformation
protocol is similar to [[HyperVerITAS#Secret Affine Relation]]

$$
R_{\text{sar}} = \left\{ ((\mathbf{I}_t, \text{com}_{\mathbf{I}}, \text{com}_{\mathbf{E}}, B, \mathbf{L}, \mathbf{R}); \mathbf{I}, \mathbf{E}) : 
\begin{aligned}
&\text{com}_{\mathbf{I}} = \text{Commit}(\text{pp}, \mathbf{I}) \\
&\land \text{com}_{\mathbf{E}} = \text{Commit}(\text{pp}, \mathbf{E}) \\
&\land \mathbf{I}_t = \mathbf{L} \odot \mathbf{I} \odot \mathbf{R} + \mathbf{E}
\end{aligned} \right\}
$$
#### Protocol
1. $\mathcal{P}$ calculates commitments to each column of $\mathbf{I} \odot \mathbf{R}$, denote these as $\text{com}_{(\mathbf{I} \odot \mathbf{R})_j}$ for $j \in [3]$.
2. $\mathcal{P}$ calculates commitments to each column of $(\mathbf{I}_t - \mathbf{E})$, denote these as $\text{com}_{(\mathbf{I}_t - \mathbf{E})_j}$ for $j \in [3]$.
3. $\mathcal{V}$ samples $\mathbf{r} \leftarrow \mathbb{F}^m$ and sends $\mathbf{r}$ to $\mathcal{P}$.
4. $\mathcal{P}$ computes $c_j := \langle \mathbf{r}, (\mathbf{I}_t - \mathbf{E})_j \rangle$, sends $c_j$ to $\mathcal{V}$, for $j \in [3]$.
5. $\mathcal{P}$ and $\mathcal{V}$ engage in an Inner Product Protocol for relation $((\mathbf{r}, \text{com}_{(\mathbf{I}_t - \mathbf{E})_j}, c_j); (\mathbf{I}_t - \mathbf{E})_j) \in R_{\text{ipE}}$ for $j \in [3]$.
6. $\mathcal{P}$ and $\mathcal{V}$ compute $\mathbf{v}_0 := \mathbf{r} \odot \mathbf{L}$.
7. $\mathcal{P}$ and $\mathcal{V}$ engage in an Inner Product Protocol for relation $((\mathbf{v}_0, \text{com}_{(\mathbf{I} \odot \mathbf{R})_j}, c_j); (\mathbf{I} \odot \mathbf{R})_j) \in R_{\text{ipE}}$ for $j \in [3]$.
8. $\mathcal{V}$ accepts if they accept the six inner product arguments (in steps 5 and 7).

### Laplace Sampling
Core Problem: We wish to sample from a laplace distribution with mean 0 and scale $\dfrac{255m}{b^2\epsilon}$. We then add this noise to the image and truncate it at 255. This post-noise action of truncation does not affect the privacy properties. 

We discretize the Laplace distribution by making a table to mimic the sampling process. The table is designed such that the number of rows mapping to $x$ is proportional to $Pr(x)$ in the Laplace distribution. 

#### Bounding the Distribution
We notice that any $x>255$ has the same effect as $x=255$ (because of truncation). It is a similar case with $x<-255$. Hence we reduce those probabilities to 0 and add them to $x=255$ and $x=-255$ respectively. Explicitly, $Pr(|x|>255) = 0$, $Pr(x=255) = Pr(x=-255) = Pr_L(x=255) + Pr_L(x>255)$ and $\forall |x| < 255, Pr(x) = Pr_L(x)$  where $Pr_L(X)$ is given by distribution $Laplace(0, \dfrac{255m}{b^2\epsilon})$

#### Discretization of Sampling

##### Proposed Method 1
Using Binomial Method to approximate the Laplace Distribution. Consider 2 random $n-$length bitstrings and find the difference of the sum of the bits in the strings. The method requires pixel-by-pixel noise generation which results in a huge overhead. We wish to find a method which can check the validity for the entire vector of noise efficiently.
##### Proposed Method 2
We discretize the Laplace distribution by making a table to mimic the sampling process. The table is designed such that the number of rows mapping to $x$ is proportional to $Pr(x)$ in the Laplace distribution. Let this table be $T_\mathcal{L}$. $T_\mathcal{L}$ allows us to map uniform randomness to Laplacian noise. Recall that this Laplacian Noise should be private. Instead of generating (and proving) private uniform randomness, we instead consider a private permutation of $T_\mathcal{L}: T'_\mathcal{L}$. We prove that the multisets $T_\mathcal{L}$ and $T'_\mathcal{L}$ are equal. This proves that $T'_\mathcal{L}$ also simulates Laplace sampling. This allows us to offload generation of uniform random numbers to the verifier $V$ followed by a proof of $N_i=T'_\mathcal{L}[R_i]$ via a simple lookup-protocol

> [!Note] Do we have to check for random permutation?
> It seems important to verify that $T_L'$ is a random permutation of $T_L$ to ensure that a *lazy* prover didn't just choose $T_L'=T_L$. However, note that this choice only hurts the prover. A malicious prover attempting to cheat the verifier wishes to misuse the noise. However, we believe that the freedom of choosing $T_L'$ does not enable this. It is in an honest prover's best interest to use a well scrambled version of $T_L$ to protect the private noise.

**Size of the table**: The quantization of sampling into a table introduces some error ($\delta$). We can calculate the total variation distance: For each individual noise output, the rounding error is $\Delta = \dfrac{1}{2*|T|}$. The outputs vary from $[-255,255]$. Hence the $\Delta_{TVD} = \dfrac{2^9}{2*|T|}$. As shown in [[#Derivation 1]], $\delta = (1+e^\epsilon)\Delta_{TVD}$. For a reasonable $\delta$ of $10^{-6}$ and $\epsilon = 0.1$, we require $|T| \approxeq 2^{29}$. This is not super practical

##### Proposed Method 3 
We use a append-only public ledger for commitments. Lets call it $\mathcal{F}$. We also assume access to a time-aware randomness beacon.  We also use Poseidon as a verifiable PRNG - the math behind which, I am currently trying to understand - [[Poseidon as a Sponge Function]]
###### Initialization
1. **Private Seed -** Prover generates secret seed $K_s$ in {0, 1}^256. and then Commit to $\mathcal{F}$. Prover computes and publishes commitment $C_{K_s} = Commit(K_s)$. Prover also commits to using the Randomness Beacon pulse at a fixed time in the future.
2. **Public Seed -** Prover waits for the randomness beacon pulse. Beacon broadcasts unpredictable public seed $K_p$ in {0, 1}^256.

> [!note]
> This initialization process, makes it a point to use public randomness, to prevent prover from 'seed-grinding attacks' which are feasible when the seed of a pseudo-random generator is determined by the prover alone. In such attacks, a dishonest prover finds a malicious seed which result in the random numbers which are in favour of the malicious intent of the dishonest prover. Private randomness in the seed is also necessary to not leak the noise generated.
###### **Circuit  Setup**
- **Public Inputs:** $C_K, K_p$
- **Private Witnesses:** $K_s$, $I$
Static Public Tables:
* $T_{alias}$: 512-row lookup table containing tuples (idx, Primary, Alias, Threshold). This is for the [[Alias Sampling Method]].
* $T_{range}$: 256-row lookup table containing values {0 ... 255}.

Initialization Constraints:
1. Verify $Commit(K_s) == C_{K_s}$.
2. Verify $time(C_{K_s}) < time(K_p)$
3. Initialize Poseidon sponge state using $(K_p, K_s)$.

Pixel by Pixel Execution Loop ($i = 1 ... n$)
For each pixel i, enforce the following constraints:
1. Extract scalar $U_i$ from the Poseidon sponge. - Check out [[Poseidon as a Sponge Function]]. These act as the private random number. 
2. Split $U_i$ into $idx$ - top 9 bits and $coin_i$ - bottom 21 bits.
3. Lookup and fetch $(idx, P_i, A_i, Thresh_i)$ in $T_{alias}$.  - (LogUp Query) 
4. Introduce selector $B_i \in \{0, 1\}$. $(B_i = 1 ~~if~~ Thesh_i \geq coin_i, ~~else~~ 0).$ 
5. To check validity of the selector, construct a circuit: $X_i = Thresh_i - coin_i + (1-B_i) \cdot 2^{21}$. Then run a range proof for $2^{21} > X_i \geq 0$. Such range proofs can also be reduced to LogUp Queries. 
6. Compute $N_i = B_i * P_i + (1 - B_i) * A_i$ and add to the pixel value.

> [!note]
> The above protocol avoids the bulky tables, however, this protocol makes use of a public append-only ledger and a trusted third party which is a time-aware randomness beacon. The choice of extending the trust in this particular way is made as both these assumptions have been "realized" through blockchains and beacons like [drand](https://drand.love/) to a reasonable extend. 
> 
> The fundamental problem for this approach is to generate reliable and verifiable private randomness. 

This has a some computing overhead which stem from commitments to $30$ bit random numbers per pixel. [[#Proposed Method 2]] avoided per pixel computations :|

### Handling Rounding off
We want our matrices to be integers. Considering our block size for pixelation is $b$. All the values of $E_2$ (rounding errors) which result from the averaging of neighbouring pixels must be of form $\dfrac{\mathbb{I}}{b}$. Therefore we multiply all elements in $L$, $I$, $R$ and $E$ by $b$ before carrying out the procedure in [[#Secret Affine Transformation]]. 

We also do a range proof to check $\forall b\cdot e_i \in E~~~ -\lfloor\dfrac{b}{2}\rfloor < b\cdot e_i < \lfloor\dfrac{b}{2}\rfloor$ to ensure that $E$ matrix only consists of rounding errors and nothing malicious.
# Appendix
## Derivation 1
Established Context
* $P$: Quantized table distribution.
* $P_{ideal}$: Ideal continuous distribution.
* $\Delta$: Total Variation Distance bounding the error such that $P(S) \le P_{ideal}(S) + \Delta$ and $P_{ideal}(S') \le P(S') + \Delta$.
* Pure $\epsilon$-DP Rule: $P_{ideal}(S) \le e^\epsilon P_{ideal}(S')$.

Substituting the pure $\epsilon$-DP multiplier and the absolute TVD bounds directly into the initial table error probability yields the final transformation:

$$P(S) \le e^\epsilon(P(S') + \Delta) + \Delta$$
$$P(S) \le e^\epsilon P(S') + \Delta(1 + e^\epsilon)$$

Comparing this against the formal $(\epsilon, \delta)$-DP definition $P(S) \le e^\epsilon P(S') + \delta$, the additive slack term resolves strictly to $\delta = \Delta(1 + e^\epsilon)$.