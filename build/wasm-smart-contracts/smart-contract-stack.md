---
icon: bring-forward
---

# Smart Contract Stack

### Runtime Architecture

UOMI's runtime is built on Substrate and incorporates `pallet-contracts`, providing a sandboxed environment for WebAssembly smart contracts. While any language that compiles to Wasm can potentially be used, the code must be compatible with the `pallet-contracts` API.

> 💡 **Tip**\
> For efficient development, it's recommended to use an eDSL (embedded Domain-Specific Language) targeting `pallet-contracts`, such as:
>
> * ink! (Rust-based)
> * ask! (AssemblyScript-based)

### Execution Environment

#### Interpreter

The `pallet-contracts` uses the `wasmi` interpreter to execute Wasm smart contracts.

> ℹ️ **Note**\
> While faster JIT interpreters like `wasmtime` exist, `wasmi` is chosen for its higher degree of interpretation correctness, which is crucial for the untrusted environment of smart contracts.

#### Contract Deployment Process

Contract deployment follows a two-step process:

1. **Code Upload**
   * Upload Wasm contract code to the blockchain
   * Each contract receives a unique `code_hash` identifier
2. **Contract Instantiation**
   * Create contract address and storage
   * Anyone can instantiate a contract using its `code_hash`

**Benefits of Two-Step Deployment**

1. **Storage Efficiency**
   * Multiple instances can share the same code
   * Reduces on-chain storage requirements
   * Particularly efficient for standard tokens (like PSP22 & PSP34)
2. **Flexible Deployment**
   * Create new instances from existing contracts
   * Use `code_hash` for contract instantiation within other contracts
   * Single upload for standard contracts, reducing gas costs
3. **Upgradability**
   * Update contract code while preserving storage and balances
   * Use `set_code_hash` to replace contract code at specific addresses

### Development Tools

#### Client APIs

> 🔧 **Available Tools**
>
> * Polkadot.js API for blockchain interaction via JavaScript
> * contracts-ui web application for contract interaction

### Comparison with Ethereum

| Feature             | Ethereum                    | UOMI                                  |
| ------------------- | --------------------------- | ------------------------------------- |
| Architecture        | Ethereum clients            | Substrate                             |
| Runtime Environment | EVM                         | Wasm pallet-contract + EVM frontier   |
| Gas Model           | Fixed price per instruction | Weight + storage fees + loading fees  |
| Smart Contract DSLs | Solidity and Vyper          | ink! (Rust) and ask! (AssemblyScript) |
| Standards           | EIPs                        | PSPs                                  |

### Additional Resources

* pallet-contracts Rust Documentation
* pallet-contracts GitHub Repository
* Polkadot.js API Documentation

> 📚 **Further Reading**\
> For more detailed information about `pallet-contracts`, visit the Rust docs or GitHub repository.
