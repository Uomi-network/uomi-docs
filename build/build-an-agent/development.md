# Development

#### Host Environment (`/host`)

The host directory contains the blockchain node simulation environment. This is a critical component that should never be modified as it represents the actual blockchain behavior.

```
⚠️ WARNING: The host directory simulates blockchain node behavior.
Modifying its contents may lead to inconsistent behavior between
development and production environments.
```

#### Agent Development (`/agent-template`)

**Core Files**

* **lib.rs**: Core agent logic
* **utils.rs**: Utility functions and blockchain interactions

**Protected Functions**

The utils.rs file contains essential offchain API functions that must not be modified. These are marked with a specific comment block:

```rust
// ===========================================================
// =============== Offchain API, DO NOT MODIFY ===============
// ===========================================================
```

These functions provide core functionality for:

* Blockchain communication
* Logging and debugging
* Memory management
* Security operations



