[[HyperVerITAS]] ends up publishing the transformation T to prove that the $I_t = T(I)$. But in certain cases this can leak information which can be a security risk. We had trouble pinpointing the Transformation where this is an issue

# Effects of Public T
## Cropping
For cropping having a public T reveals the location of $I_T$ in $I$. 
This is largely harmless. For low entropy images such as maybe a cropped image of an ID Card, it may reveal the structure of the ID Card (eg. The Image is on the left-side in a legit ID Card or the name is above the ID for a legit ID Card, etc)
This seemed too specific: tailor fitting the industrial problem to the theoretical problem we wish to tackle.

# Redaction
The public T reveals the location of the redaction, which is nothing more than what is already revealed in the image. There are corner cases where we "redact" using the bg color and donot  wish to reveal that we have redacted. 
This to seemed to specific

# Blurring
Traditional blurring methods (gaussian blur and avg-pixelation) are already public algorithms. There is little to hide in T other than the location of blur and some parameters used such as padding and size for convolution kernels, radius of gaussian blur, etc. [This (non-scientific) blog](https://medium.com/@gonced8/can-you-recover-a-blurred-image-61bbcaa969d5) which claims difficulty in "de-blurring" when kernel is secret. However, I donot think there is credit to it as the author did not brute force efficiently.
## Deblurring is reasonably easy
Reading more and more about deblurring, it seems like deblurring standard blurring algorithms is reasonably efficient making it an inherent security risk, irrespective of whether T is public or not.

> [!quote]
> However, recent studies have shown that pixelization \[13], blurring \[13], and the P3 system \[7] are not effective in privacy preservation. Given sufficient training data and the obfuscation technique, various models can be built to associate the obfuscated images to the ground truth, which can be used to decode redacted documents \[13], and to re-identify faces and handwritten digits \[14]. Therefore, we are in need of image obfuscation methods that can provide rigorous privacy guarantees.
> (From [[DP-Pix.pdf]])
## Better Blurring
[[DP-Pix.pdf]] proposed a more secure blurring method which involves adding noise to achieve differential privacy for images. Now here the blurring is not reversible due to inherent randomness in the transformation. Here a public T is catastrophic, it reveals the random noise added. When we map to the affine transformations as used in [[HyperVerITAS]] : $I_t = L \cdot I \cdot R + E$ , intuitively, the $L$ is from the natural pixelation algorithm whereas the E is where the randomness shows up. Revealing this will reveal the randomness used resulting in loss of security. This is good motivation for building a custom proof system which treats T as private.

# Encryption
There is a line of research which involves blurring based on an encryption key, allowing those with the key to decrypt it and access the image. This also seems like a natural candidate to motivate hiding T. But note: Those with the Key can simply decrypt and use the [[HyperVerITAS]] or some other existing proof system to verify. However having T in public is insecure without a doubt. In such cases we attempt to prove that an encrypted image came from a particular source image (plaintext). This case is not that common...? 
## Homomorphic Encryption
There is some credit to it when we consider "Homomorphic Encryption"...? because it may be of use to have proofs that the encryption is in fact from a "credible source". (Consider Medical Imaging which is signed by the Medical Instrument used). But this is likely to be already covered as it should not be any different than encryption of any other data/content. Attaching for Ref: [[Fully Homomorphic Image Processing.pdf]]

Notice that to develop a custom proof system for Image Encryption, we must specifically focus on encryption algorithms which are targeted at images. This include [[Chaos-based-enc.pdf]] and methods mentioned in [[LitReview-Enc.pdf]]

It may also be important to note that: In practice, people make 2 copies of the image containing sensitive data, encrypt the original and redact the sensitive information in the publicly available copy. This seems to provide similar (if not better) guarantees.