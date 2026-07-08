Paper: [[DP-Pix.pdf]]

> [!Quote] **Problem Setting**
> We consider the problem setting where a data owner wishes to share one or more images with a wide range of untrusted recipients, e.g., researchers or the greater public. The data owner must sanitize the image data prior to its publication, in order to protect the privacy of individuals or objects captured in the images. 

# Notation and Preliminaries
Image $I : M \times N$  
Author considers Grayscale $I$ and mentions extension by considering each channel separately :D

> [!note] Pixelation
> The pixelization technique renders the source image using larger blocks. It is achieved by partitioning the image using a two-dimensional grid, and the average pixel value is released for each grid cell. Similar to \[13], we adopt a “square” grid where the pixel width is equal to the pixel height in the grid cells, i.e., each grid cell contains b x b pixels. In general, a smaller b value yields better approximation and visual quality,
> ![[Pasted image 20260615120026.png]]

# Extending Differential Privacy to Images
## Neighbouring Images
**m-Neighbourhood:** 2 images $I_1$ and $I_2$ are neighboring if they differ by atmost m pixels (same dimensions). Allowing up to m pixels to differ enables us to protect the presence or absence of any object, text, or person, represented by those pixels in an image.  The privacy of any sensitive information represented by at most m pixels, can thus be protected. The m-Neighborhood notion can also be applied to protect features of an object or person such as the eyebrows, eyes, etc.

**We assume that removing those pixels is sufficient to protect the privacy of the underlying information, by definition of differential privacy**

## Main Idea
Uses [[Laplace Noise for DP]] from the paper [[Laplace for DP.pdf]] 

**Differentially Private Pixelization (Pix)**
In a nutshell, DP Pix algorithm first performs pixelization on an input image, and applies Laplace perturbation to the pixelized image. Specifically, let $c_k$ denote the $k$-th grid cell over an $M \times N$ image. As shown in Figure 3, there are $\lceil \frac{M}{b} \rceil \lceil \frac{N}{b} \rceil$ cells in total. Let $K = \lceil \frac{M}{b} \rceil \lceil \frac{N}{b} \rceil$. The pixelization of an image $I$ can be denoted as a vector of length $K$, i.e.:

$$P_b(I) = \left\{ \frac{1}{b^2} \sum_{(x,y) \in c_1} I(x, y), \frac{1}{b^2} \sum_{(x,y) \in c_2} I(x, y), \dots, \frac{1}{b^2} \sum_{(x,y) \in c_K} I(x, y) \right\}$$

The [[Laplace Noise for DP#Global Sensitivity|Global Sensitivity]] of $P_b$ is thus:

$$\Delta P_b = \max_{I_1, I_2} |P_b(I_1) - P_b(I_2)| = \frac{255m}{b^2}$$

This is because the difference between any two pixels is at most 255 and up to $m$ pixels can differ between any neighboring images $I_1$ and $I_2$. Let $\tilde{N} = \{\tilde{N}_1, \tilde{N}_2, \dots, \tilde{N}_K\}$ and each $\tilde{N}_k$ ($k \in \{1, \dots, K\}$) is randomly drawn from a Laplace distribution with mean 0 and scale $\frac{255m}{b^2\epsilon}$. The following theorem states the privacy guarantee of the $\tilde{P}_b$ algorithm, where $\tilde{P}_b(I) = P_b(I) + \tilde{N}$, $\forall I$.

*Algorithm $\tilde{P}_b$ satisfies $\epsilon$-differential privacy.*
*Proof.* Since $\Delta P_b = \frac{255m}{b^2}$, by definition applying the [[Laplace Noise for DP|Laplace mechanism]] to $P_b$ achieves differential privacy. Note that each pixel in $\tilde{P}_b(I)$ is truncated to the range of $[0, 255]$. This post-processing of $\tilde{P}_b$ does not affect its privacy guarantee. 