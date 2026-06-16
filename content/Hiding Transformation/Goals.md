Based on the [[Idea]], we attempt to formalize our goals:
# Broad Level Goal
is to form custom proof system which are efficient to progress the field of content provenance. We limit ourselves to images. We consider the scenario where the camera/image generator is the root of trust and the editing software is not trusted. Existing systems attempt to protect the original image $I$ and give a ZK proof for the transformed image $I_t$ to be a valid edit of $I$. However they publish (and use) the transformation $T$ for the proof. Only exception being the case of gray-scaling in [[HyperVerITAS]] where they hide part of $T$ which may leak the color. We attempt to extend this to specific cases of blurring and perhaps encryption where we keep $T$ or part of $T$ private but show the legitimacy of the transformations via efficient ZK-Proofs

# Targeted Goal
The current system is robust enough to handle standardized blurring and pixelation algorithms with reasonable security guarantees. However as discussed in [[Idea#Deblurring is reasonably easy]], we can argue that standardized blurring is intrinsically not secure. *Link to some attack papers perhaps*. This has motivated cryptographically secure algorithms mentioned in [[Idea#Better Blurring]]. We look particularly at [[DP-Pix.pdf]] algorithm (as it is the most cited).

> [!abstract]
>We aim to propose an efficient ZK proof system for Differentially Private Pixelation of credibly sourced Images without trusting the editing software
## Subgoal 1: Understanding DP Pixelation
[[DP-Pixelation]]
