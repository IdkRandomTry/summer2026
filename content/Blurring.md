Traditional blurring methods (gaussian blur and avg-pixelation) are already public algorithms. There is little to hide in T other than the location of blur and some parameters used such as padding and size of convolution kernels, radius of gaussian blur, etc. [This (non-scientific) blog](https://medium.com/@gonced8/can-you-recover-a-blurred-image-61bbcaa969d5) which claims difficulty in "de-blurring" when kernel is secret. However, I do not think there is credit to it as the author may have not used brute force efficiently.
#### Deblurring is reasonably easy
Reading more about deblurring, it seems like deblurring images which use standard blurring algorithms is reasonably efficient making it an inherent security risk, irrespective of whether T is public or not.

> [!quote]
> However, recent studies have shown that pixelization \[13], blurring \[13], and the P3 system \[7] are not effective in privacy preservation. Given sufficient training data and the obfuscation technique, various models can be built to associate the obfuscated images to the ground truth, which can be used to decode redacted documents \[13], and to re-identify faces and handwritten digits \[14]. Therefore, we are in need of image obfuscation methods that can provide rigorous privacy guarantees.
> ~ [[DP-Pix.pdf]]
#### Better Blurring
[[DP Pixelation]] proposed a more secure pixelation method which involves adding noise to achieve differential privacy for images. DP-Pixelation is not reversible due to inherent randomness in the transformation. Hence a public $T$ is catastrophic as it reveals the random noise added. 

When we map to the affine transformations as used in [[HyperVerITAS]] : $I_t = L \cdot I \cdot R + E$ , intuitively, the $L$ is from the natural pixelation algorithm whereas the $E$ is where the random noise is seen. Revealing this will reveal the randomness used resulting in loss of security. This is good motivation for extending the [[HyperVerITAS]] system to handle DP-Pixelation where $E$ is kept private.
