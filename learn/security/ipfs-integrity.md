# IPFS Integrity

### Overview

The IPFS pallet provides decentralized storage capabilities to the blockchain through integration with the InterPlanetary File System (IPFS). It manages pinning, unpinning, and retrieval of files while ensuring data persistence and availability through validator consensus.

### Key Features

#### Pinning System

* **Persistent Pins**: Files that remain pinned indefinitely
* **Temporary Pins**: Files with an expiration block number
* **Validator-based Consensus**: Content becomes available when pinned by majority of validators

#### Pin Types

**Agent Pins**

* Permanent pins associated with NFT IDs
* Used for storing agent-related data
* Can be updated with new CIDs

**Temporary Pins**

* Time-limited storage
* Minimum duration of 28,800 blocks (approximately 1 day)
* Automatically unpinned after expiration

### Core Mechanisms

#### Consensus Process

1. **Validator Pinning**:
   * Validators run offchain workers to process pin requests
   * Each validator maintains its own IPFS node
   * Files become "usable" when pinned by majority (50% + 1) of validators

#### Pin Lifecycle

1. **Pin Request**:
   * User submits CID for pinning
   * System records pin request with expiration time
2. **Processing**:
   * Offchain workers detect new pin requests
   * Validators attempt to pin the content
   * System tracks pinning status per validator
3. **Activation**:
   * Content becomes "usable" once majority threshold is reached
   * System updates UsableFromBlockNumber to avoid using the file on requests received on the chain before the availability of the file
4. **Expiration**:
   * System tracks expiration through ExpirationBlockNumber
   * Automatic cleanup of expired pins
   * Validators remove expired content

### Storage Management

#### Key Storage Items

```rust
NodesPins: (CID, AccountId) => bool
AgentsPins: NFTId => CID
CidsStatus: CID => (ExpirationBlockNumber, UsableFromBlockNumber)
```

#### Status Tracking

* **ExpirationBlockNumber**: When the pin expires (0 for permanent pins)
* **UsableFromBlockNumber**: When content becomes available (post-majority pinning)

### Operations

#### Pin File

```rust
pin_file(origin, cid: CID, duration: BlockNumber)
```

* Requires minimum duration (28,800 blocks)
* Creates temporary pin with expiration
* Triggers validator pinning process

#### Pin Agent

```rust
pin_agent(origin, cid: CID, nft_id: NFTId)
```

* Creates permanent pin associated with NFT
* Updates existing pins if necessary
* Maintains single CID per NFT ID

#### Get File

```rust
get_file(cid: CID) => Result<Vec<u8>>
```

* Checks pin status and expiration
* Verifies content is "usable"
* Returns file content if available

### Block Processing

#### Inherent Data

* Each block processes pin operations
* Updates usable content status
* Removes expired pins
* Updates pin status based on validator consensus

#### Offchain Worker

* Runs after each block
* Processes pending pin requests
* Updates local IPFS node
* Submits pin status updates

