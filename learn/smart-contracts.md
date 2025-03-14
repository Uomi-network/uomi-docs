---
icon: file-lines
---

# Smart Contracts

### Overview[​](https://docs.astar.network/docs/learn/smart-contracts#overview) <a href="#overview" id="overview"></a>

Developers can create and deploy smart contracts on Uomi Network using various programming languages, including Solidity, compatible with Ethereum smart contracts, and ink!, a Rust-based smart contract language for the Polkadot ecosystem. This compatibility ensures a seamless transition for developers from other blockchain ecosystems, fostering interoperability and encouraging the adoption of the Uomi Network.

### WebAssembly smart contracts[​](https://docs.astar.network/docs/learn/smart-contracts#webassembly-smart-contracts) <a href="#webassembly-smart-contracts" id="webassembly-smart-contracts"></a>

Uomi runtimes are based on Substrate and incorporate `pallet-contracts`, a sandboxed environment used to deploy and execute WebAssembly (Wasm) smart contracts. Any language that compiles to Wasm can be deployed and run on this Wasm Virtual Machine, provided the code is compatible with the `pallet-contracts` [API](https://docs.rs/pallet-contracts/latest/pallet_contracts/api_doc/trait.Current.html).

To avoid unnecessary complexity and writing boilerplate code, the most appropriate method of building involves the use of an eDSL specifically targeting `pallet-contracts`, such as ink! (based on Rust) or ask! (based on AssemblyScript), among others as the ecosystem grows.&#x20;

After compilation, a Wasm blob can be deployed and stored on-chain.

### Ethereum Virtual Machine smart contracts[​](https://docs.astar.network/docs/learn/smart-contracts#ethereum-virtual-machine-smart-contracts) <a href="#ethereum-virtual-machine-smart-contracts" id="ethereum-virtual-machine-smart-contracts"></a>

The Uomi EVM implementation is based on the Substrate Pallet-EVM, providing a full Rust-based EVM implementation. Smart contracts on Uomi EVM can be implemented using Solidity, Vyper, or any other language capable of compiling smart contracts to EVM-compatible bytecode. Pallet-EVM aims to provide a low-friction and secure environment for the development, testing, and execution of smart contracts that is compatible with the existing Ethereum developer toolchain.
