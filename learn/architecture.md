---
icon: sitemap
---

# Architecture

UOMI is a Layer 1 blockchain specialized for:

* Executing all types of smart contracts
* Providing a hybrid EVM + Wasm environment with interoperability
* Seamlessly aggregating features and assets within its ecosystem
* Running AI-driven economic agents

### Understanding the Architecture

UOMI is built with Substrate, inheriting many of its powerful features, including its account system. At its core, UOMI leverages Substrate's technology stack while operating as an independent Layer 1 blockchain.

#### Core Components

At a high level, a UOMI node provides a layered environment with two main elements:

1. An outer node that handles:
   * Network activity and peer discovery
   * Transaction request management
   * Consensus mechanisms
   * RPC call responses
2. A runtime containing all the business logic for executing the state transition function of the blockchain. For more detailed information, see the [Infrastructure](infrastructure/) and [Security](security/) documentation.

### FRAME

FRAME (Framework for Runtime Aggregation of Modularized Entities) encompasses numerous modules and support libraries that simplify runtime development. In Substrate, these modules (called pallets) offer customizable business logic for different use cases and features that you might want to include in your runtime.

The framework provides pallets for common blockchain functionalities such as:

* Staking
* Consensus
* Governance
* Uomi-engine
* IPFS
* Other core activities

### Smart Contract Execution

UOMI provides a robust environment for smart contract execution through two main Virtual Machine (VM) implementations:

#### Ethereum Virtual Machine (EVM)

The Ethereum Virtual Machine (EVM) is a virtual computer with components that enable network participants to store data and agree on the state of that data. In UOMI, the core responsibilities of the EVM are implemented in the EVM pallet, which is responsible for executing Ethereum contract bytecode written in high-level languages like Solidity. UOMI EVM provides a fully Ethereum Virtual Machine compatible platform, which you can learn more about in the [EVM chapter](../build/evm-smart-contracts/).

#### Substrate Virtual Machine for Wasm Contracts

UOMI includes the pallet-contracts module for WebAssembly (Wasm) smart contracts. This implementation supports the execution of Wasm-based smart contracts, providing an alternative to traditional EVM-based contracts. The Wasm environment offers several advantages, including:

* Improved performance
* Enhanced security features
* Greater language flexibility

You can learn more about Wasm contract development in the [Wasm chapter](../build/wasm-smart-contracts/).

#### Hybrid Approach

One of UOMI's key features is its hybrid approach to smart contract execution, allowing developers to choose between EVM and Wasm environments based on their specific needs. This flexibility enables:

* Cross-contract interactions between EVM and Wasm
* Optimization of different use cases
* Broader ecosystem compatibility
