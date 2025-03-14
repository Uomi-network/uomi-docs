---
icon: key
---

# Accounts

### Overview <a href="#overview" id="overview"></a>

A Uomi Network account is composed of a private key and a public key. The public key, often referred to as the account address, is publicly accessible. The private key, on the other hand, is essential for accessing and managing the associated account. While anyone can send tokens to your public address, only the holder of the private key can access and control those tokens. Consequently, safeguarding your private key is of utmost importance.

Uomi Network is compatible with two types of virtual machines, Wasm VM and EVM, and thus employs two different account formats.

### Substrate Accounts <a href="#substrate-accounts" id="substrate-accounts"></a>

Uomi is developed using Substrate, a framework for building blockchains, and it utilizes Substrate accounts. In Substrate-based chains, the public key is used to derive one or more public addresses. Instead of directly using the public key, Substrate enables the generation of multiple addresses and address formats for an account. This means that a single public-private key pair can be used to derive various addresses for different Substrate chains.

The private key is a cryptographically secure sequence of randomly generated numbers. For easier human readability, the private key can generate a random sequence of words known as a secret seed phrase or mnemonic.

Substrate-based chains, including Uomi, use the ss58 address format. This format is a variant of Bitcoin's Base-58-check, with some modifications. Notably, ss58 includes an address type prefix to identify the address as belonging to a specific network.

### EVM Accounts <a href="#evm-accounts" id="evm-accounts"></a>

On the Uomi EVM side, Ethereum-style addresses (H160 format) are supported within the Substrate-based chain. These addresses are 42 hex characters long. Each Ethereum-style address corresponds to a private key, which can be used to sign transactions on the Ethereum side of the chain. Additionally, these addresses are mapped to a storage slot within the Substrate Balance pallet, linking them to Substrate-style addresses (H256 format).
