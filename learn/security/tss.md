# TSS

TSS, or Threshold Signature Scheme, is a cryptographic protocol that enables a group of nodes to collaboratively generate a shared signature. This scheme ensures that the resulting signature is valid only if a predefined number of nodes (known as the threshold) participate in the signing process.

## Key Concepts

* **Shared Public Key**: The group of nodes collectively holds a shared public key.
* **Private Keys**: Each node possesses its own private key, which is used in the signature generation process.
* **Threshold**: A predefined number of nodes must participate in generating the signature. If the number of participating nodes meets or exceeds this threshold, the signature is considered valid.

## How TSS Works

1. **Initialization**:
   * A shared public key is established for the group.
   * Each node generates its private key, which remains secret.
2. **Signature Generation**:
   * When a transaction or message requires signing, nodes collaborate by using their private keys.
   * The signature is only produced if the number of contributing nodes meets the threshold.
3. **Validation**:
   * The generated signature can be verified using the shared public key.
   * This ensures that the signature is authentic and originates from the group.

In the UOMI blockchain, TSS is integrated to provide agents with their own wallets. This allows agents to perform onchain transactions during their execution. The use of TSS enhances security and ensures that transactions are only authorized when a sufficient number of agents agree, thus maintaining the integrity of the process.
