# OPOC

OPOC, or Optimistic Proof of Computation, is a mechanism designed to ensure the integrity and security of computational operations that occur outside the blockchain (offchain). This approach leverages both offchain and onchain elements to provide a tamper-proof system where certain operations are validated by multiple nodes to achieve consensus.

## Key Concepts

* **Offchain Operations**: These are processes that occur outside the blockchain environment. While they offer scalability and speed, they are susceptible to tampering by malicious nodes.
* **Onchain Operations**: These are processes executed directly on the blockchain, ensuring tamper-proof and immutable records.

## How OPOC Works

1. **User Interaction**
   * A user initiates a request by calling a function in a Solidity contract, providing necessary parameters such as `NFT_ID`, `INPUT_DATA`, and `INPUT_FILE_CID`.
2. **Request Initialization**
   * The contract generates a unique `REQUEST_ID` and invokes a specific function from a precompiled contract, passing critical parameters like `REQUEST_ID` and the other user parameters.
3. **Data Verification**
   * The system checks the input data and retrieves associated NFT information.
   * It stores the request details in the `Inputs` storage and logs an event indicating the request has been accepted.
4. **Consensus Levels**
   * Depending on the NFT specifications, the system start the assignment of the execution to a random node.

## Security Mechanisms

### **During Block Validation:**

* **Wasm and IPFS File Verification**:
  * The system ensures the availability and validity of files required for the execution (the wasm of the AI Agent and the input file).
  * It checks the status of these files through the IPFS pallet, verifying their usability and expiration.
* **Node Assignment and Execution**:
  * Nodes are assigned to process requests based on current load and execution requirements.
  * The system monitors the execution and consensus among nodes, escalating to higher levels of OPOC if discrepancies arise.

### **Consensus Verification:**

* **Level 0**: A single node executes the request.
* **Level 1**: Multiple nodes are involved to achieve a higher consensus.
* **Level 2**: Additional nodes are engaged if discrepancies are found, ensuring a majority consensus.

### Request Completion and Rewards

* The final result is stored in the `Outputs` storage.
* Nodes are rewarded based on their participation and accuracy, with penalties for nodes in the `OpocBlacklist` or those with timeouts and errors.

### Offchain Worker Execution

* Nodes continually monitor and execute assigned tasks, ensuring timely processing and consensus.
* Results are stored, and timeouts are managed to maintain system efficiency.

