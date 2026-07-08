
ZK-SNARKs are **Z**ero-**K**nowledge **S**uccinct **N**on-Interactive **AR**guments of **K**nowledge.

They allow one party (the Prover) to prove to another party (the Verifier) that a specific statement is true, without revealing *any* information beyond the validity of the statement itself.

Here is a breakdown of what the acronym actually means:
	*Insert breakdance gif* - no sorry:
	
- ZK:  Zero-Knowledge - The proof leaks absolutely no private data. For example, you can prove you are over 18 without revealing your exact birthdate, or prove you know the password to an account without revealing the password itself.
* S - Succinct - The resulting proof is small (often just a few kilobytes) and can be verified by a standard computer in milliseconds.
* N - Non-Interactive - The Prover and Verifier do not need to be online at the same time to have an interaction. The Prover generates a single, standalone proof and hands it to the Verifier.
* ARK - Argument of Knowledge - It is mathematically guaranteed that a Prover could not have constructed the proof unless P actually knew the underlying secret information.