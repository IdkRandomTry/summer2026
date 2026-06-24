We define $R_{dpat}$ (diff private affine transformation) for $I_T = L\cdot I\cdot R + E$ (where noise is captured in E)
$$
R_{dpat} = \left\{ 
\begin{align} 
	&((I_T, com_I, com_N, com_{seed-priv},L,R,Seed_{public}); I, E, N, X,  seed_{priv})~~~\\
	\\
	1)&~com_I = Commit(pp,I)\\
	2)&~com_N = Commit(pp,N)\\
	3)&~I_T = LIR+E\\
	&3.1)~where~E = Truncated(N)\\
	4)&~N = BinomialNoise(X)\\
	&4.1)~where~X = PRNG(seed)\\
	&4.2)~and~seed = HASH(Seed_{priv}||Seed_{public})
\end{align} 
\right\}
$$

# Challenges
- 1), 2) and 3) from Secret Affine Transformation from [[HyperVerITAS]] - It works and I understand
- 3.1) is an *unsolved challenge* ⚠️: We must truncate $N$ dynamically so that $LIR+E$ stays within $[0,255]$ - Studying [[Plonk.pdf]] may help
- 4) and 4.1) is discussed in [[verifiable dp.pdf]] - It works for 4) but I am yet to understand. for 4.1) it discusses a multiparty method which is also computationally expensive - need to explore alternatives
- A alternative for 4.1) can be shown by [[Sponge Functions]] - It *should* work but I am yet to understand.
- ⚠️ NEW ISSUE IN 4.1) - I believed that $X=PRNG(seed)$ is enough but we also have to show $seed$ is chosen randomly! This is to counter "Grinding Attacks"
	- This can be done by a "public beacon"
		- Get a public random number $S_{beacon}$ from public beacon, commit to a private random number ($S_{priv}$). Prove $X = HASH(S_{beacon}||S_{priv})$.
	- The multiparty thing in [[verifiable dp.pdf]] also will work - but will result in terribly expensive interactive protocol for verification - so skip
	  Seems like the best idea is to use q "Randomness Beacon" (Drand) once for seed generation and then use Poseidon sponge function to generate the following random numbers.