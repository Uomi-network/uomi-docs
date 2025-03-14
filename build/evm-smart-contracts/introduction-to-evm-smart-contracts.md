---
icon: play
---

# Introduction to EVM Smart Contracts

### What are EVM Smart Contracts?

Smart contracts on UOMI are programs that run on the Ethereum Virtual Machine (EVM). These self-executing contracts contain code and data that live at a specific address on the blockchain. Being EVM-compatible, UOMI supports the same smart contract functionality as Ethereum, allowing developers to write and deploy contracts using familiar tools and languages.

### Programming Language

Smart contracts on UOMI are primarily written in Solidity, the most widely used programming language for EVM development. Solidity is:

* Object-oriented and high-level
* Specifically designed for smart contracts
* Similar to JavaScript/C++ in syntax
* Statically typed

> 💡 **Note**\
> While Solidity is the most common choice, you can also use other EVM-compatible languages like Vyper.

### Development Requirements

To start developing smart contracts on UOMI, you'll need:

#### 1. MetaMask Wallet

* Browser extension for interacting with EVM chains
* Manages your accounts and transactions
* Connects dApps to the blockchain

#### 2. UOMI Network Configuration

```
Network Name: UOMI Finney Testnet
RPC URL: https://finney.uomi.ai
Chain ID: 4386
Currency Symbol: UOMI
```

#### 3. Development Tools

* **Solidity Compiler**: Converts Solidity code to EVM bytecode
* **Development Framework**: Hardhat, Truffle, or Foundry
* **Code Editor**: VS Code with Solidity extensions recommended

### Smart Contract Basics

#### Contract Structure

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.19;

contract MyContract {
    // State variables
    uint256 public value;

    // Events
    event ValueChanged(uint256 newValue);

    // Constructor
    constructor() {
        value = 0;
    }

    // Functions
    function setValue(uint256 newValue) public {
        value = newValue;
        emit ValueChanged(newValue);
    }
}
```

#### Key Concepts

1. **State Variables**
   * Permanently stored in contract storage
   * Represent the contract's state
2. **Functions**
   * Execute contract logic
   * Can be public, private, internal, or external
   * Can modify state or be view/pure
3. **Events**
   * Log important changes
   * Can be monitored by applications

### Interacting with Smart Contracts

You can interact with smart contracts through:

1. **MetaMask**
   * Send transactions
   * Manage accounts
   * Connect to dApps
2. **Web3 Libraries**
   * ethers.js
   * web3.js
3. **Block Explorers**
   * View transactions
   * Verify contracts
   * Monitor events

### Getting Started

1. **Set Up MetaMask**
   * Install the extension
   * Create or import an account
   * Add UOMI network
2. **Get Test Tokens**
   * Use the [testnet faucet](https://app.uomi.ai/faucet)
   * Required for deployment and testing
3. **Choose Development Tools**
   * Install development framework
   * Set up your IDE
   * Configure network settings

### Next Steps

Ready to start developing? Check out:

* [Your First Smart Contract](your-first-evm-smart-contract.md)
* [HardHat](hardhat.md)
