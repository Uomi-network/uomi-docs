---
icon: squid
---

# ink! Development

### Introduction

ink! is a Rust-based eDSL (embedded Domain-Specific Language) developed by Parity Technologies for writing smart contracts on Substrate's `pallet-contracts`.

> 💡 **Why ink!?**\
> ink! is currently the most widely supported eDSL for Substrate-based smart contracts, with strong backing from both Parity and the builder community.

### Key Features

ink! provides developers with powerful tools and features:

#### Core Capabilities

* Write smart contracts using idiomatic Rust code
* Leverage ink! macros and attributes via `#[ink::contract]`
* Utilize trait definitions and implementations
* Create upgradeable contracts through delegate calls
* Interact with Substrate pallets using Chain Extensions
* Perform off-chain testing with `#[ink(test)]`

#### Development Tools

* Procedural macros for simplified development
* Comprehensive crate ecosystem
* Reduced boilerplate code requirements

> ⚙️ **Getting Started**\
> For installation instructions, visit the ink! Environment section.

### Development Resources

#### Official Documentation

* [GitHub Repository](https://github.com/paritytech/ink)
* [Introduction Guide](https://paritytech.github.io/ink/)
* [Official Documentation](https://use.ink/)
* [Rust Documentation](https://docs.rs/ink/4.0.0-rc/ink/index.html)

#### Key Topics

* [Contract Macros & Attributes](https://use.ink/macros-attributes/contract)
* [Trait Support](https://use.ink/3.x/basics/trait-definitions)
* [Upgradeable Contracts](https://use.ink/3.x/basics/upgradeable-contracts)
* [Chain Extensions](https://use.ink/macros-attributes/chain-extension/)
