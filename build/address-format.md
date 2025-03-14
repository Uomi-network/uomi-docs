---
icon: at
---

# Address format

## Address Format

### Understanding UOMI Addresses

UOMI, being built on Substrate and supporting dual Virtual Machine environments, utilizes a unique address system. The blockchain implements the SS58 address format, which is derived from Bitcoin's Base-58-check encoding with specific modifications to support network-specific addressing.

A key feature of the SS58 format is its network identifier prefix, which ensures addresses are uniquely associated with the UOMI network.

### Two Address Types

Due to UOMI's support for both EVM and Wasm smart contracts, the network operates with two distinct types of addresses:

#### 1. Native Address (SS58)

* Uses 256 bits
* Based on Substrate's SS58 encoding
* Used for native blockchain operations and Wasm contracts

#### 2. EVM Address (H160)

* Uses 160 bits
* Begins with "0x" prefix
* Compatible with Ethereum-style operations
* Used for EVM contract interactions

This dual-address system allows UOMI to maintain compatibility with both ecosystems while preserving each environment's unique features and capabilities.
