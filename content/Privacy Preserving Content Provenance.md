There has been a line of research which is motivated to tackle the pain points of the [[C2PA]] standard as mentioned above. 

The research paper [[PhotoProof.pdf]] talks about these problems. Surprisingly it pre-dates the formation of C2PA. [[PhotoProof.pdf]] gives a theoretical solution for proving an image is a transformation of a signed raw image, where the transformation are a sequence of permitted operations. Authors also give a proof-of-concept implementation. However in practice the time and compute required was not ideal and the system could not scale to real world application.

# Scalable & Privacy Preserving Content Provenance
Following [[#Privacy-preserving Content Provenance]], the pursuit has been to optimize the system and make it more scalable for real-world application. [[VerITAS.pdf]] introduced a system which is efficient and scalable. [[HyperVerITAS.pdf]] improved on that system even more. However both papers made a tradeoff. For increase in efficiency, the transformation applied on the image was made public. Note that the original image is still private only the transformation/edits are public. For a concrete example, consider Cropping out the left side of the image. As the transformation is public, the verifier now knows that the left side of the raw image was cropped out, however what was in the left side is still private. This technology is of great use when proving authenticity of images which were edited to hide sensitive information by means of redacting or cropping.

We delve into the details of HyperVerITAS system as it is of relevance to us: [[HyperVerITAS]]
## Dangers of a Public Transform
[[HyperVerITAS]] is scalable, but ends up publishing the transformation T to prove that the $I_t = T(I)$. But in certain cases this can leak information which can be a security risk. [[HyperVerITAS]] itself deals with this issue, where the rounding errors in gray-scaling result in leaking the color of the raw image. The authors deal with this by hiding the error term and giving a proof of its well-formedness i.e. each error term is -0.5<e<0.5. In an abstract sense, they hide the part of transformation which leaks information but show its well-formedness. 

We think of other transformation where a public transformation is a security risk.
### Cropping
For cropping. having a public T reveals the location of $I_T$ in $I$. 
This is largely harmless. For low entropy images such as maybe a cropped image of an ID Card, it may reveal the structure of the ID Card (e.g. The Image is on the left-side in the ID Card or the name is above the ID Number, etc.).
### Redaction
The public T reveals the location of the redaction, which is nothing more than what is already revealed in the image. There are corner cases where we "redact" using the background color and do not  wish to reveal that we have redacted. However this is not prevalent.
### Blurring
Traditional blurring methods (gaussian blur and avg-pixelation) are already public algorithms. There is little to hide in T other than the location of blur and some parameters used such as padding and size of convolution kernels, radius of gaussian blur, etc. [This (non-scientific) blog](https://medium.com/@gonced8/can-you-recover-a-blurred-image-61bbcaa969d5) which claims difficulty in "de-blurring" when kernel is secret. However, I do not think there is credit to it as the author may have not used brute force efficiently.
#### Deblurring is reasonably easy
Reading more about deblurring, it seems like deblurring images which use standard blurring algorithms is reasonably efficient making it an inherent security risk, irrespective of whether T is public or not.

> [!quote]
> However, recent studies have shown that pixelization \[13], blurring \[13], and the P3 system \[7] are not effective in privacy preservation. Given sufficient training data and the obfuscation technique, various models can be built to associate the obfuscated images to the ground truth, which can be used to decode redacted documents \[13], and to re-identify faces and handwritten digits \[14]. Therefore, we are in need of image obfuscation methods that can provide rigorous privacy guarantees.
> ~ [[DP-Pix.pdf]]
#### Better Blurring
[[DP Pixelation]] proposed a more secure pixelation method which involves adding noise to achieve differential privacy for images. DP-Pixelation is not reversible due to inherent randomness in the transformation. Hence a public $T$ is catastrophic as it reveals the random noise added. 

When we map to the affine transformations as used in [[HyperVerITAS]] : $I_t = L \cdot I \cdot R + E$ , intuitively, the $L$ is from the natural pixelation algorithm whereas the $E$ is where the random noise is seen. Revealing this will reveal the randomness used resulting in loss of security. This is good motivation for extending the [[HyperVerITAS]] system to handle DP-Pixelation where $E$ is kept private.

### Encryption
There is a line of research which involves blurring based on an encryption key, allowing those with the key to decrypt it and access the image. This also seems like a natural candidate to motivate hiding T. But note: Those with the Key can simply decrypt and use the [[HyperVerITAS]] or some other existing proof system to verify. However having T in public is insecure without a doubt. In such cases we attempt to prove that an encrypted image came from a particular source image (plaintext). This is the case of verifiable encryption, which may be of some interest. 
#### Homomorphic Encryption
There is some credit to Verifiable Encryption when we consider "Homomorphic Encryption", because it may be of use to have proofs that the encryption is in fact from a "credible source". (Consider Medical Imaging which is signed by the Medical Instrument used). But this is likely to be already covered as it should not be any different than encryption of any other data/content. Attaching for Ref: [[Fully Homomorphic Image Processing.pdf]]

Notice that there is some credit to developing custom verifiable encryption system for encryption algorithms which are targeted at images. This include [[Chaos-based-enc.pdf]] and methods mentioned in [[LitReview-Enc.pdf]]

> [!Note]
> It may also be important to note that: In practice, people make 2 copies of the image containing sensitive data, encrypt the original and redact the sensitive information in the publicly available copy. This seems to provide similar (if not better) guarantees. However it does use twice the storage space.

