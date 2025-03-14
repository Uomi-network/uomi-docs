# Domain-Specific Languages (DSLs)

### Understanding eDSLs

Embedded Domain-Specific Languages (eDSLs) are specialized programming tools that enhance blockchain and smart contract development. These tools operate within existing programming languages, providing developers with a more intuitive and efficient way to write code.

> 💡 **Key Benefit**\
> EDSLs allow developers to write smart contracts at a higher level of abstraction, making code more readable, maintainable, and less prone to errors.

#### Why Use eDSLs?

EDSLs offer several advantages for blockchain development:

* More expressive and intuitive code writing
* Built-in error checking mechanisms
* Enhanced debugging capabilities
* Specialized testing frameworks
* Domain-specific optimizations

For example, rather than using pure Rust for Wasm smart contracts, developers can use specialized Rust eDSLs designed specifically for blockchain development, making the code more natural and easier to maintain.

### Available eDSLs

#### ink!

> ℹ️ **What is ink!?**\
> ink! is a Rust-based eDSL developed by Parity Technologies, specifically designed for Substrate's `pallet-contracts`.

**Key Features**

* Rust procedural macros support
* Comprehensive crate ecosystem
* Reduced boilerplate code
* Direct integration with `pallet-contracts` API

**Resources**

* [Official Documentation](https://ink.substrate.io/why-rust-for-smart-contracts)
* [GitHub Repository](https://github.com/paritytech/ink)
* [API Reference](https://docs.rs/pallet-contracts/latest/pallet_contracts/api_doc/trait.Current.html)

#### ask!

> 🚧 **Development Status**\
> ask! is a Polkadot treasury funded project currently under active development.

**Overview**

* Framework for AssemblyScript developers
* TypeScript-like syntax
* Targets `pallet-contracts` for Wasm smart contracts

**Resources**

* [GitHub Repository](https://github.com/ask-lang/ask)
* [Treasury Proposal](https://polkadot.polkassembly.io/post/949)

### Choosing the Right eDSL

When selecting an eDSL for your project, consider:

1. **Programming Language Familiarity**
   * ink! for Rust developers
   * ask! for TypeScript/AssemblyScript developers
2. **Project Requirements**
   * Smart contract complexity
   * Performance needs
   * Team expertise
3. **Development Status**
   * ink! is production-ready
   * ask! is under development

> 📚 **Learn More**\
> For detailed information about using these eDSLs, refer to their respective documentation and GitHub repositories.
