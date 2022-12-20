# Internship Sheet 10 - Ethereum

## Task 1 - Ethereum addresses
### (1a.) How are Ethereum addresses formed? Describe the formation of an Ethereum address step by step.
* A random private key is created (64 (hex) characters / 32 bytes)
* The public key is derived from the private key  using the Elliptic Curve     Digital Signature Algorithm.
* The address is then formed from the public key by taking the last 20 bytes of the Keccak-256 hash of the public key and adding 0x as a prefix.