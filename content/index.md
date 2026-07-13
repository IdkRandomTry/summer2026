# Summary
Hello,
These notes are populated during my studies in Summer 2026 under the guidance of Prof. Arvind and Garrett. The notes contain what I learnt, studied and also a proposal for something slightly novel. The overarching domain of studies was **Content Provenance**, under which I particularly studied *privacy-preserving image provenance*. The thought-process behind the studies are driven by practical motivations, aiming towards impactful research which solves real-world problems. 

Happy reading !
# How to Navigate the Notes
The notes use internal linking to avoid repetition and offer good access to notes which explain pre-requisites or relevant concepts when reading about a particular topic. Hovering on a link gives you a sneak-peak into what the linked-note holds. If you click to go to a particular linked-note, use the **Backlinks** (a section at right-bottom or bottom of page) to come back. You can also use the **Graph View** to check out related notes. Hovering on a node in the graph displays its title. To come back to this page ([[content/Index|Index]]), click on **Summer 2026** (left top or top of page).

Following are what I consider good starting points for iving into the notes (in order):
- **Content Provenance** attempts to give verifiable information regarding the authenticity of a media, often involving its source/origin and potentially involving the edits which were made. To learn more - [[Content Provenance]].
- **HyperVerITAS** proposes an efficient, scalable and modular system for image verification. Its efficiency boost relies on modelling image transformations as Affine-like Transformations. To learn more - [[HyperVerITAS]].
- Traditional blurring methods are not secure! To learn about the issues and proposed solutions check out [[Blurring]] and [[DP Pixelation]].
- **HyperPlonk** advanced technology for fundamental tools used cryptography. It leverages modelling problems as evaluation Boolean Hypercube for efficiency. To learn more - [[HyperPlonk]]
---
DP-Pixelation uses secret noise to protect the privacy of features being blurred. Scalable privacy-preserving image provenance systems like HyperVerITAS utilize a public edit-transformation to increase there scalability and efficiency. However, if the secret noise of DP-Pixelation is made public, it will lose its privacy guarantees. [[DP Pixelation Provenance]] records my attempts to solve this. It includes failed attempts, in-efficient solutions, optimization attempts which culminatedto what I feel is a *reasonable* solution. 

---
# Disclaimer
This "vault" is still "work-in-progress". I am currently attempting to understand and fill the notes for the following concepts. 
- [[Poseidon as a Sponge Function]]
- (some other notes may also need a re-work or finishing touches)

---
Some rough ideas which I considered interesting enough to be noted but not formulated enough to make it into the notes can be found on the [[Whiteboard]].