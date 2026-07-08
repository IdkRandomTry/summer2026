Paper: [[HyperVerITAS.pdf]]

![[Pasted image 20260704160728.png]]

HyperVerITAS proposes an efficient modular system for image verification. Its efficiency boost relies on modelling image transformations as Affine-like Transformation on the raw image vector of form $I_T = L\cdot I\cdot R + E$ where $I\in \mathbb{F}^{n\times 3}$. Intuitively, $L$ is responsible for crop, scaling, resizing, redacting etc.; $R$ is responsible for gray-scaling and other colour-based transformations; and $E$ is responsible for redaction and managing rounding errors.
# Relation
HyperVerITAS shows a ZKP for the following relation

 $R(pp, vk, A)$ is parameterized by the public parameters $pp$, the verification key $vk$ of the digital signature scheme $\Sigma$, and a hash function $A$. It is defined as follows:

$$R=\left\{((I_t, com_I, H, \sigma, T); I) : com_I = \text{Commit}(pp, I) \land I_t = T(I) \land H = A \odot I \land I \in [256]^{n \times 3} \land \text{Vrfy}(vk, H, \sigma) = 1\right\}$$

This relation is highly modular and broken down into four main checks:
* **Commitment check** ($com_I = \text{Commit}(pp, I)$): Ensures that the commitment is correctly generated.
* **Transformation check** ($I_t = T(I)$): Verifies that the transformed image is valid.
* **Pre-image check** ($H = A \odot I \land I \in [256]^{n \times 3}$): Checks that the prover knows a low-norm pre-image of the hash.
* **Signature check** ($\text{Vrfy}(vk, H, \sigma) = 1$): Ensures the signature is valid with respect to the message, verification key, and hash. Notice that the camera signs on the hash of the image (which is computed inside the camera itself) which is public. Read [[Digital Signature Schemes]] for more details on how the verification works.
## Public Affine Relation

$R_{par}$ handles any image transformation that can be modeled as an affine transformation. It is defined as follows:

$$R_{par} = \left\{((I_t, com_I, L, R, E); I) : com_I = \text{Commit}(pp, I) \land I_t = L \odot I \odot R + E\right\}$$
**Public Input:** $(I_T, com_I, L, R, E)$
**Private Witness:** $I$
### Protocol
We use [[Inner Product PIOP]]
1. The Prover ($P$) calculates commitments to each column of $I \odot R$, denoted as $com(I \odot R)_j$ for $j \in [3]$.
2. The Verifier ($V$) samples a random challenge vector $r \leftarrow \mathbb{F}^m$ and sends $r$ to $P$.
3. $P$ and $V$ compute: $c_j := \langle r, (I_T - E)_j \rangle$ for $j \in [3]$.
4. $P$ and $V$ compute: $v_0 := r \odot L$.
5. $P$ and $V$ engage in an Inner Product Protocol for the relation: $((v_0, com(I \odot R)_j, c_j); (I \odot R)_j) \in R_{ipE}$ for $j \in [3]$.
6. $V$ accepts if they accept all three inner product arguments.

## Secret Affine Relation
Authors note that when using Affine-like transformations for gray-scaling, rounding off to nearest integer is required. This is captured in $E$ matrix. This value can leak information regarding the original color. Hence we introduce [[#Secret Affine Relation]] where we commit to $E$ and prove its well-formedness without revealing it.

$$
R_{\text{sar}} = \left\{ ((\mathbf{I}_t, \text{com}_{\mathbf{I}}, \text{com}_{\mathbf{E}}, B, \mathbf{L}, \mathbf{R}); \mathbf{I}, \mathbf{E}) : 
\begin{align}
&\text{com}_{\mathbf{I}} = \text{Commit}(\text{pp}, \mathbf{I}) \\
&\land \text{com}_{\mathbf{E}} = \text{Commit}(\text{pp}, \mathbf{E}) \\
&\land \|\mathbf{E}\|_\infty \leq B \\
&\land \mathbf{I}_t = \mathbf{L} \odot \mathbf{I} \odot \mathbf{R} + \mathbf{E}
\end{align} \right\}
$$

### Protocol 
We use [[Inner Product PIOP]] and [[Lookup PIOP]]
1. $\mathcal{P}$ calculates commitments to each column of $\mathbf{I} \odot \mathbf{R}$, denote these as $\text{com}_{(\mathbf{I} \odot \mathbf{R})_j}$ for $j \in [3]$.
2. $\mathcal{P}$ calculates commitments to each column of $(\mathbf{I}_t - \mathbf{E})$, denote these as $\text{com}_{(\mathbf{I}_t - \mathbf{E})_j}$ for $j \in [3]$.
3. $\mathcal{V}$ samples $\mathbf{r} \leftarrow \textdollar \mathbb{F}^m$ and sends $\mathbf{r}$ to $\mathcal{P}$.
4. $\mathcal{P}$ computes $c_j := \langle \mathbf{r}, (\mathbf{I}_t - \mathbf{E})_j \rangle$, sends $c_j$ to $\mathcal{V}$, for $j \in [3]$.
5. $\mathcal{P}$ and $\mathcal{V}$ engage in an Inner Product Protocol for relation $((\mathbf{r}, \text{com}_{(\mathbf{I}_t - \mathbf{E})_j}, c_j); (\mathbf{I}_t - \mathbf{E})_j) \in R_{\text{ipE}}$ for $j \in [3]$.
6. $\mathcal{P}$ and $\mathcal{V}$ compute $\mathbf{v}_0 := \mathbf{r} \odot \mathbf{L}$.
7. $\mathcal{P}$ and $\mathcal{V}$ engage in an Inner Product Protocol for relation $((\mathbf{v}_0, \text{com}_{(\mathbf{I} \odot \mathbf{R})_j}, c_j); (\mathbf{I} \odot \mathbf{R})_j) \in R_{\text{ipE}}$ for $j \in [3]$.
8. $\mathcal{P}$ and $\mathcal{V}$ engage in a Lookup Protocol for relation $(([B], \text{com}_{\mathbf{E}}); \mathbf{E}) \in R_{\text{rc}}$.
9. $\mathcal{V}$ accepts if they accept the six inner product arguments (in steps 5 and 7) and the lookup argument (in step 8).

> [!note] Handling Rounding-off
> The relation for gray-scale is 
> $$y := \text{Round}(0.30 \cdot r + 0.59 \cdot g + 0.11 \cdot b)$$
> We want our matrices to be integers. So we multiply throughout by 100 to get
> $$\mathbf{L} = I_{m \times m}, \quad \mathbf{R} = \begin{bmatrix} 30 & 30 & 30 \\ 59 & 59 & 59 \\ 11 & 11 & 11 \end{bmatrix}, \quad \mathbf{E} = 100 \cdot \begin{bmatrix} \mathbf{e}_0 & \mathbf{e}_0 & \mathbf{e}_0 \\ \vdots & \vdots & \vdots \\ \mathbf{e}_{n-1} & \mathbf{e}_{n-1} & \mathbf{e}_{n-1} \end{bmatrix}$$

## Low Norm Linear Hash Pre-Image
We now want to show that $I$ is the Pre-Image of $H$ which is signed by camera. We must show it is a low norm pre-image, since the hash collision resistant only for low norm by [[Short Integer Solution Problem]]. We describe the relation as:

$$R_{\text{lh}} = \left\{ ((\text{com}_{\mathbf{I}}, \mathbf{A}, \mathbf{H}); \mathbf{I}) : 
\begin{align}
&\text{com}_{\mathbf{I}} = \text{Commit}(\text{pp}, \mathbf{I}) \\
&\land \mathbf{H} = \mathbf{A} \odot \mathbf{I} \\
&\land \mathbf{I} \in [256]^{n \times 3}
\end{align} \right\}$$

### Protocol
uses [[Inner Product PIOP]] via [[#Public Affine Relation]] and [[Lookup PIOP]]
We note that $H=A\odot I$ can be considered as an instance of [[#Public Affine Relation]].
1. $\mathcal{P}$ and $\mathcal{V}$ engage in a Public Affine Transformation Protocol for relation $((\mathbf{H}, \text{com}_{\mathbf{I}}, \mathbf{A}, I_{3 \times 3}, [0]^{h \times 3}); \mathbf{I}) \in R_{\text{par}}$.
2. $\mathcal{P}$ and $\mathcal{V}$ engage in a Lookup Protocol for relation $(([256], \text{com}_{(\mathbf{I})_j}); (\mathbf{I})_j) \in R_{\text{rc}}$.
3. $\mathcal{V}$ accepts if they accept the public affine transformation argument and the three lookup arguments.

HyperVerITAS also implement and give experimental results to prove the efficiency of the proposed algorithm. - Github Repo: github.com/glgreiner/HyperVerITAS