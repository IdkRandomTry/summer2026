
> [!quote] Advancing digital content transparency and authenticity
> 
> The **Coalition for Content Provenance and Authenticity**, or C2PA, provides an open technical standard for publishers, creators and consumers to establish the origin and edits of digital content. It’s called Content Credentials, and it ensures content complies with standards as the digital ecosystem evolves
> ~ https://c2pa.org/

C2PA - basics: https://youtu.be/hA0ZjqakEF8?si=6QXk-juMFQK73ccq

C2PA currently proposes a relatively straightforward system to implement [[Content Provenance]].  Just like how the raw image was signed by the camera, we append to it a series of edits made in a C2PA-compliant software using the allowed tools and sign those sequence of edits to provide authentication. Then anyone can use the public & signed raw image **followed** by the public & signed edit sequence to get a verified/trusted transformed image. In other words it considers the editing software as a trusted party.

C2PA is backed by several industry-leading companies: including camera companies like SONY and editing software companies like Adobe which supports implementation of proposed system.
# Problems with C2PA's current system
C2PA - the weird angle: https://youtu.be/saqAYgWanwg?si=1585pyxkRpqf11Yk

It may seem at the surface that this system is exactly what we want. However it is not without its flaws.
- **Privacy of Original Image:** The original raw image needs to be public for verification. In cases where original image has sensitive data which is cropped out, redacted or blurred (which is common in several crime cases), verification would fail. It would be of interest to provide Zero Knowledge Proof of Source while keeping the raw image private.
- **Verifiers Burden:** Redoing all the edits at the verifiers end will require considerable compute (and time). It would be of interest to reduce the verification time.
- **Cost of C2PA trusted software:** Certain open-source, free software may not be considered C2PA compliant, although being legitimate and worthy of trust. Hence there is motivation to treat the editing software as an untrusted party when providing content provenance. This reduces the trusted base (we only trust the camera) and makes the technology more accessible.

This has given rise to [[Privacy Preserving Content Provenance]]

---
Suggested next page: [[Privacy Preserving Content Provenance]]