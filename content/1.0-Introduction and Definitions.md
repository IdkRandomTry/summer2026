Cryptography stems from the two Greek words kryptos and grafein meaning “hidden” and “to write” respectively. So cryptography is the task of "Hidden Writing"

# Setup
Alice wants to communicate with Bob with adversaries trying to learn the messages.

# Plaintext
The message which has to be communicated.
$\mathcal{M}$ - Message Space

# Encryption Algorithm
A method to Encrypt plaintext using a [[#Key]] which maintains privacy against adversaries. 
`ENC`-Encrypts
`DEC`-Decrypts

# Key
An input to Encryption algorithm. May be public, private, shared. 
`GEN`-Generates Key
$\mathcal{K}$- Key Space

# Ciphertext
Plaintext + Key ---Encryption Algorithm---> Ciphertext

# Adversaries
- Eve: 
	- Knows the ciphertext
	- Wishes to learn the plaintext
- Malory:
	- Knows the ciphertext
	- Wishes to establish malicious connection with either Alice or Bob

