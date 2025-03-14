# Model Updates Integrity

The **Model Updates Integrity** system ensures seamless and secure transitions between different AI model versions, maintaining the reliability of AI operations across the UOMI network. This process is critical for guaranteeing that AI agents run the appropriate model versions during updates, thereby safeguarding the consistency and accuracy of computations.

## Key Components

#### On-Chain Storage

Two key on-chain storages are utilized to manage model updates:

1. **AiModels**:
   * Stores the list of valid AI models that agents can utilize.
   * Each model is identified by a unique `UOMI_KEY` and includes:
     * **LOCAL\_NAME**: The actual model name installed on the nodes (e.g., `llama-2.0.0`).
     * **USABLE\_FROM\_BLOCK\_NUMBER**: The block number from which the model becomes usable.
     * **OLD\_LOCAL\_NAME**: The previous version of the model used before the update.
2. **NodesVersions**:
   * Contains the version details of each node.
   * Nodes periodically update this storage with their current version via the `offchain_worker`.

## Update Process

During the update process, the system ensures that nodes and agents operate on consistent model versions:

1. **Version Identification**:
   * At each block validation or after a set number of blocks, nodes identify the version used by the majority by reading from `NodesVersions`.
2. **Model Transition**:
   * The active model for the majority is recorded in `AiModels` with the corresponding `LOCAL_NAME`, `USABLE_FROM_BLOCK_NUMBER`, and `OLD_LOCAL_NAME`.
   * This allows for a smooth transition where agents can continue using the older model until the majority has switched to the new version.

## Practical Example

1. **Block 15**: A request is added to the chain.
2. **Block 16**: The initial phase of computation begins, using model `llama-2.0.0`. The validator updates `AiModels` to switch to `llama-2.1.0` from block 16, keeping `llama-2.0.0` as the old model.
3. **Block 18**: The computation continues using the older model for requests initiated before block 16.
4. **Block 20**: New requests use the updated model `llama-2.1.0`, as the transition has been completed.

### Transition Handling

Nodes must be capable of running both the old and new models during transitions to ensure uninterrupted service and accuracy. This dual compatibility is essential to maintain consistent operations throughout the network update process.

