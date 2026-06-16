Attempt to prove source of image while maintaining privacy of sensitive data which is blurred via [[DP-Pixelation]].

# Stuff in order
- DP Pixelation is naturally an affine operation (linear averaging plus a noise offset). This maps perfectly to [[HyperVerITAS]] proof system
- Standard (non-private) pixelation can easily be reversed using deep learning. DP Pixelation significantly reduces the success rate of CNN-based re-identification attacks **provided the noise matrix (E) remains private**

# Stuff which needs work
- Alternative ways to add differential privacy to explore potential outside of Laplace Noise
	- The Geometric Mechanism (Discrete Laplace)
	- The Binomial Mechanism (Difference of Binomials)
- If using Laplace Noise:
	- Need to read Laplace Mechanism (Discrete)
	- ZK Proof for Hidden Random Noise / Seed
	- ZK Proof for Truncation to 255
	- Efficiency of ZK Proof Proof