# Internship Sheet 10 - Ethereum

## Task 1 - Ethereum addresses
### (1a) How are Ethereum addresses formed? Describe the formation of an Ethereum address step by step.
* A random private key is created (64 (hex) characters / 32 bytes)
* The public key is derived from the private key  using the Elliptic Curve     Digital Signature Algorithm.
* The address is then formed from the public key by taking the last 20 bytes of the Keccak-256 hash of the public key and adding 0x as a prefix.
### (1b) What is the relationship between the hash algorithms SHA256 and Keccak?
* Keccak-256, a cryptographic function, is part of Solidity (SHA-3 Family) while SHA-256 is the implementation of the SHA-2 standard with a 256 bits key.
* Ethereum uses Keccak-256 in a consensus engine called Ethash.
* The Bitcoin blockchain makes extensive use of SHA-256, including when identifying transaction hashes and when miners are performing proof-of-work mining.
### (1c) What is an Ethereum Black Hole Address?
The Ethereum address 0x000000000000000000000000000000000000. It is the Ethereum community's equivalent of a black hole, that receives all tokens supplied to it without remorse and never returning any of them. It is the Ethereum network's genesis address, and roughly $520 million worth of ether and ERC20 tokens have been sent there by error or intentionally.
