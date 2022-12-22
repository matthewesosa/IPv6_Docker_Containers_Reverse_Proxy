# Internship Sheet 10 - Ethereum

## Task 1 - Ethereum addresses
### (1a) How are Ethereum addresses formed? Describe the formation of an Ethereum address step by step.
* A random private key is created (64 (hex) characters / 32 bytes)
* The public key is derived from the private key  using the Elliptic Curve     Digital Signature Algorithm.
* The address is then formed from the public key by taking the last 20 bytes of the Keccak-256 hash of the public key and adding 0x as a prefix.
Reference: [ethereum.stackexchange.com](https://ethereum.stackexchange.com/questions/3542/how-are-ethereum-addresses-generated)
### (1b) What is the relationship between the hash algorithms SHA256 and Keccak?
* SHA-256 and Keccak are both cryptographic hash functions designed to be secure, fast, and efficient.
* Keccak-256, a cryptographic function, is part of Solidity (SHA-3 Family) while SHA-256 is the implementation of the SHA-2 standard with a 256 bits key.
* Ethereum uses Keccak-256 in a consensus engine called Ethash.
* The Bitcoin blockchain makes extensive use of SHA-256, including when identifying transaction hashes and when miners are performing proof-of-work mining.
* Another key difference between SHA-256 and Keccak is the size of the hash output: SHA-256 produces a 256-bit hash, while Keccak can produce hashes of various lengths, including 224, 256, 384, and 512 bits.

[Reference: geeksforgeeks.org](https://www.geeksforgeeks.org/difference-between-sha-256-and-keccak-256/)
### (1c) What is an Ethereum Black Hole Address?
* An Ethereum black hole address is a special address on the Ethereum blockchain that is associated with a contract that has no code and therefore cannot execute any actions. Transactions sent to a black hole address are effectively lost, as they cannot be retrieved or used in any way.
* The concept of a black hole address was introduced as part of the Ethereum network's upgrade to Ethereum 2.0, which included a feature called "account abstraction." Account abstraction allows for the creation of smart contracts that are associated with addresses, but do not contain any code. This can be useful in certain situations, such as when creating a contract that serves as a placeholder for a future contract that has not yet been developed.
### (1d) What is the clever idea behind the Ethereum Improvement Proposal 55 ([EIP-55: Mixed-case checksum address encoding](https://eips.ethereum.org/EIPS/eip-55) )?
* The main idea behind EIP 55 is to reduce the likelihood of users sending transactions to the wrong Ethereum address due to human error.

* EIP 55 introduces a checksum mechanism for Ethereum addresses.

* The EIP 55 algorithm transforms the original address in a number of ways to produce a checksummed address. This entails changing the address to all uppercase letters and adding a checksum as suffix.

* The address checksum approach ensures that a given address is legitimate and belongs to the intended recipient. It functions by adding an additional level of address validation, making sure that the  address is entered or copied accurately. The checksum is created by appending the first four bytes of the address's Keccak-256 hash to the end of the address in hexadecimal format.

### (1e) With the ETH vanity address generator](https://vanity-eth.tk/) you can generate Ethereum vanity addresses - i.e. easily identifiable addresses. Behind each address there is an Ethereum account consisting of (secret key, public key, Ethereum address).
### * Create an Ethereum vanity address for yourself ETH-matr , the last 5 characters of which are the last 5 digits of your (confidential) matriculation number.
* Comment : My address ETH-pub is: 
0xf7C15b05235f6F814a2F520c0102923B73777777



