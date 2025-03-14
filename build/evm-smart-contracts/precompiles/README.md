---
icon: rectangle-terminal
---

# Precompiles

A precompile is a common functionality used in smart contracts that has been compiled in advance, so Ethereum nodes can run it more efficiently. From a contract's perspective, this is a single command like an opcode. The Frontier EVM used on Uomi network provides several useful precompiled contracts. These contracts are implemented in our ecosystem as a native implementation. The precompiled contracts `0x01` through `0x08` are the same as those in Ethereum (see list below). Additionally, Uomi implements precompiled contracts that support new Uomi features.

### Ethereum Native Precompiles <a href="#ethereum-native-precompiles" id="ethereum-native-precompiles"></a>

<table><thead><tr><th width="150">Precompile</th><th>Address</th></tr></thead><tbody><tr><td>ECRecover</td><td>0x0000000000000000000000000000000000000001</td></tr><tr><td>Sha256</td><td>0x0000000000000000000000000000000000000002</td></tr><tr><td>Ripemd160</td><td>0x0000000000000000000000000000000000000003</td></tr><tr><td>Identity</td><td>0x0000000000000000000000000000000000000004</td></tr><tr><td>Modexp</td><td>0x0000000000000000000000000000000000000005</td></tr><tr><td>Bn128Add</td><td>0x0000000000000000000000000000000000000006</td></tr><tr><td>Bn128Mul</td><td>0x0000000000000000000000000000000000000007</td></tr><tr><td>Bn128Pairing</td><td>0x0000000000000000000000000000000000000008</td></tr></tbody></table>

### Uomi Specific Precompiles <a href="#astar-specific-precompiles" id="astar-specific-precompiles"></a>

<table><thead><tr><th width="177">Precompile</th><th>Address</th></tr></thead><tbody><tr><td>Sr25519</td><td>0x0000000000000000000000000000000000005002</td></tr><tr><td>SubstrateECDSA</td><td>0x0000000000000000000000000000000000005003</td></tr><tr><td>Assets-erc20</td><td>ASSET_PRECOMPILE_ADDRESS_PREFIX</td></tr><tr><td>UomiEngine</td><td>0x00000000000000000000000000000000756f6D69</td></tr></tbody></table>

The interface descriptions for these precompiles can be found in the `precompiles` folder:  [Uomi repo](https://github.com/Uomi-network/uomi-node). The Addresses can be checked in the [Uomi repo](https://github.com/Uomi-network/uomi-node) for each runtime in `precompile.rs` files.

***

## Usage example

Here we'll demonstrate a simple smart contract that can interact with Sr25519 precompile

```solidity
// SPDX-License-Identifier: GPL-3.0-or-later
pragma solidity ^0.8.0;

contract Sr25519Caller {
    address constant precompileAddress = 0x0000000000000000000000000000000000005002;

    function verify(bytes32 publicKey, bytes memory signature, bytes memory message) public view returns (bool) {
        (bool success, bytes memory result) = precompileAddress.staticcall(
            abi.encodeWithSignature("verify(bytes32,bytes,bytes)", publicKey, signature, message)
        );
        require(success, "Failed to call precompile");
        return abi.decode(result, (bool));
    }
}
```
