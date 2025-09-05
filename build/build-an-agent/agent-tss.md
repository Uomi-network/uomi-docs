---
description: >-
  This guide is ONLY for developers building AGENTS that submit EVM transactions
  through the UOMI TSS system. It intentionally omits validator / pallet
  internals. Focus: how you obtain the agent wallet
---

# Agent TSS

### 1. What is TSS?

Multiple network participants collectively hold shares of an ECDSA (secp256k1) key. No single machine has the full private key. When your agent asks to send a transaction, the network collaborates to produce a normal Ethereum-compatible signature. You just supply transaction parameters in a JSON action; the system handles signing.

### 2. Your Agent Wallet (Public Key → Address)

After a Distributed Key Generation (DKG) round completes, an aggregated secp256k1 public key is stored on-chain and associated with your agent (via the session / NFT context). You need its Ethereum address:

1. Listen for a `DKGCompleted(session_id, aggregated_pubkey_bytes)` event (or query historical events / storage if you missed it).
2. Public key is uncompressed (65 bytes) or already stripped; if it starts with 0x04 drop that prefix.
3. Compute: `address = keccak256(pubkey_x_y_bytes)[12..]` (last 20 bytes) → 0x-prefixed H160.

Rust utility example:

```rust
use tiny_keccak::{Hasher, Keccak};
fn eth_address_from_uncompressed(pk: &[u8]) -> [u8;20] { // pk: 65 or 64 bytes
    let raw = if pk.len() == 65 && pk[0] == 0x04 { &pk[1..] } else { pk }; // expect 64 bytes now
    let mut keccak = Keccak::v256();
    keccak.update(raw);
    let mut out = [0u8;32];
    keccak.finalize(&mut out);
    let mut addr = [0u8;20];
    addr.copy_from_slice(&out[12..]);
    addr
}
```

TypeScript example:

```ts
import { keccak256 } from 'viem';
function ethAddressFromUncompressed(pub: Uint8Array): `0x${string}` {
  const raw = (pub.length === 65 && pub[0] === 0x04) ? pub.slice(1) : pub; // 64 bytes
  const hash = keccak256(raw as any); // returns 0x + 64 hex chars
  return ('0x' + hash.slice(-40)) as `0x${string}`;
}
```

Store / cache this address. Use it as `from` when you want explicit gas estimation or RPC nonce fallback.

### 3. Minimal Action JSON

You output a single JSON object containing an `actions` array. Each transaction is an element:

```json
{
  "actions": [
    {
      "action_type": "multi_chain_transaction",
      "chain_id": 4386,
      "data": "0x",
      "to": "0x000000000000000000000000000000000000dead",
      "value": "0x0"
    }
  ],
  "response": "any text your agent wants to say"
}
```

Only `actions` is parsed for execution. `response` (and any other extra top‑level fields) is ignored by the TSS logic.

### 4. Action Fields (What You Usually Need)

Required:

* `action_type`: "multi\_chain\_transaction"
* `chain_id`: target chain (see table below)
* `data`: hex call data (use "0x" for empty)

Common optional:

* `to`: destination (include for normal calls / transfers)
* `value`: hex or decimal string; default 0
* `from`: agent wallet address (recommended – improves gas / nonce inference)
* `tx_type`: "eip1559" (default) or "legacy"
* `max_fee_per_gas` / `max_priority_fee_per_gas`: for EIP‑1559 fine tuning
* `gas_limit`: let system estimate unless you have a known requirement
* `gas_price`: only for legacy OR as a fallback baseline
* `nonce`: almost never set manually – see next section

### 5. Nonces (Keep It Simple)

Priority order inside the system:

1. Explicit `nonce` you provide
2. Internally allocated windowed nonce (**preferred default**)
3. RPC account nonce (if `from` is supplied and internal allocation not used)
4. Fallback 0

Recommendation: **DO NOT** set `nonce` unless you are coordinating multi‑agent sequencing. Let the system allocate; it preserves ordering and handles gaps. If a tx stalls (e.g. low gas) the system may queue empty fillers to advance—your only action might be to re‑emit a higher fee transaction if desired.

### 6. Gas Strategy

* Provide neither `gas_limit` nor fees → system fetches gas price / estimates gas (fallbacks: 1 gwei, 21,000).
* For EIP‑1559 pass `max_fee_per_gas` & `max_priority_fee_per_gas`; omit `gas_price`.
* For simple value transfers default estimates are fine.

### 7. Supported Chains

| Chain               | ID    |
| ------------------- | ----- |
| Ethereum            | 1     |
| Binance Smart Chain | 56    |
| Polygon             | 137   |
| Avalanche           | 43114 |
| Arbitrum            | 42161 |
| Optimism            | 10    |
| Fantom              | 250   |
| Uomi                | 4386  |
| Base                | 8453  |

### 8. Example With EIP‑1559 Fees

```json
{
  "actions": [
    {
      "action_type": "multi_chain_transaction",
      "chain_id": 1,
      "to": "0x000000000000000000000000000000000000dead",
      "data": "0x",
      "max_fee_per_gas": "0x77359400",
      "max_priority_fee_per_gas": "0x3b9aca00",
      "value": "0x0"
    }
  ],
  "response": "burn scheduled"
}
```

### 9. Quick Checklist Before Emitting an Action

* Have you derived & stored the agent address from the aggregated public key?
* Are you using the correct `chain_id`?
* Is `data` a hex string with 0x prefix? (Use "0x" if empty.)
* Avoid manual `nonce` unless absolutely necessary.
* Provide EIP‑1559 fee fields or none (let defaults) – but not both `gas_price` and EIP‑1559 fields.

### 10. Troubleshooting

| Symptom                      | Likely Cause             | Fix                                        |
| ---------------------------- | ------------------------ | ------------------------------------------ |
| Tx never mined               | Underpriced gas          | Resubmit with higher fee fields            |
| Repeated nonce errors        | Manual nonce conflict    | Remove explicit nonce, let system allocate |
| Gas estimation fallback used | Missing `from`           | Include `from` address                     |
| Address mismatch in UI       | Incorrect pubkey hashing | Ensure 0x04 prefix removed before keccak   |

That’s all you need as an agent developer. Internals (storage maps, offchain workers, filler logic) are intentionally abstracted away.

***

### Appendix: (For the Curious)

This section is OPTIONAL reading. It gives extra context about how the underlying threshold ECDSA system works, why threshold selection matters, what happens during a reshare, and how core storage items relate. You do NOT need this to build basic agents, but it can help you reason about edge cases and design choices.

#### A. DKG (Distributed Key Generation) – High Level

Goal: Generate a secp256k1 public key without any single participant ever constructing the corresponding private scalar.

Typical (abstracted) flow:

1. Participant Set: An authority set of size n is fixed for the round.
2. Polynomial Commitments: Each participant samples a secret polynomial f\_i(x) of degree t-1. The secret share intended for participant j is f\_i(j). Commitments (e.g. elliptic curve points g^{f\_i(k)}) are broadcast so others can verify consistency.
3. Share Distribution: Encrypted / authenticated channels deliver shares f\_i(j) to each participant j.
4. Verification: Each participant checks that received shares match the published commitments. Invalid senders can be flagged (offence reporting).
5. Aggregation: Each participant sums all valid shares received (including its own) to obtain its final private share s\_j. The global secret key s = Σ\_i f\_i(0) never exists explicitly; each s\_j is just one additive component of s.
6. Public Key: The aggregated public key is computed as G^{s} (secp256k1 point) by aggregating public commitments; stored on-chain as the agent’s key.

Signing later uses an interactive threshold ECDSA protocol (nonce generation + partial signature shares -> combination). The chain only ever sees a standard ECDSA signature.

#### B. Reshare (Dynamic Participation)

When validators leave/enter or you want to adjust n (while keeping/rotating the key):

1. Trigger: Governance / session transition initiates a new DKG session with updated participant set.
2. Old vs New Key:

* Key Rotation: A brand new aggregated key replaces the previous one (most secure; breaks continuity of address).
* Key Continuation (if supported): Some schemes allow resharing the SAME underlying secret s without changing the public key by running a resharing protocol that maps old shares to new shares. (If your implementation performs a fresh DKG every time, you simply get a new key.)

3. Superseding: Previous completed sessions are marked superseded in storage; new session may copy forward the aggregated key if continuity is intended (seen in code path that re-inserts previous aggregated key into a new session if not rotating).
4. Threshold t-of-n Still Applies: Both signing AND (if supported) resharing correctness require ≥ t honest active participants from the initial key generation round — e.g. if the initial `t=3`, then we need at least `3` participants from the previous key generation in order to reshare the key with new participants. Keep in mind that this is evaluated in sequence, so assuming we have `t` and `n` fixed to 3 and 5 then it means that we will still need `3-of-5` **previous** participants in order to succesfully reshare the key.&#x20;

Threshold Choice Guidance:

* Security increases with higher t (attacker must compromise more shares).
* Liveness decreases with higher t (harder to gather enough partial signatures during network churn/outages).
* Recommended: 60–70% of n (e.g. for n=10 → t=7). Avoid >80% because then your agent's wallet would be subject to possibly a malicious attack in which 20% of the validators might cut your wallet out of the system.

Rule of Thumb:

```
if t < 0.6n -> elevated collusion risk
if 0.6n <= t <= 0.7n -> balanced
if t > 0.8n -> liveness hazard under churn
```

#### C. Core Storage (Conceptual)

Not exhaustive—just the parts relevant to multi-chain signing:

* `DkgSessions` – Tracks state of each DKG session (in progress, complete, superseded).
* `AggregatedPublicKeys` – Maps `SessionId` -> aggregated secp256k1 public key bytes. Use most recent non-superseded session for the wallet address.
* `SigningSessions` – Active threshold signing rounds (each linked to a request / message preimage). Contains who has provided what, partial signature collection state, etc.
* `NonceStates` – Per (Agent NFT, ChainID) nonce allocation window (sequential ordering + acceptance pruning).
* `PendingTssOffences` (and related) – Reported misbehavior (e.g. invalid shares, protocol deviations) queued for resolution / slashing logic elsewhere.

#### D. Threshold vs Operational Risks

Choosing t too low:

* Faster signing under partial outages.
* Increased probability that a small colluding subset can forge signatures.

Choosing t too high:

* Resilience against compromise.
* Greater chance a few offline nodes stall all signing or resharing → backlog of pending nonces → potential gap filler churn.

Mitigation Strategies:

* Track historical uptime before lowering t.
* Introduce adaptive fee bump logic off-chain (agent side) if mempool congestion persists.
* Periodic resharing to evict chronically unreliable participants.

#### E. Failure & Recovery (Example Scenarios)

Scenario: Participant goes offline mid-signing.

* If remaining online signers ≥ t → signature completes normally.
* If < t → signing session stalls; agent should avoid spamming retries—wait for reshare or participant recovery.

Scenario: Stuck Low-Gas Transaction.

* Internal nonce window blocks subsequent higher nonces.
* Submit replacement with higher fee (same nonce) OR rely on filler after detection of prolonged gap.

Scenario: Chain RPC Failures.

* Gas price and nonce fallback to default heuristics (1 gwei, 21k, 0 if allocation unreachable).
* Consider explicitly setting fees during known RPC instability.

#### F. Security At a Glance

* Secret never reconstructed; only signatures leak derived info (standard ECDSA security assumptions).
* Share compromise threshold: An attacker needs ≥ t valid shares simultaneously.
* Replay protection: Nonce tracking + enforcement of contiguous acceptance prevents reordering or skipping.

#### G. Practical Checklist for Threshold Tuning (Add-On)

| Metric                            | Observe  | Action                                                       |
| --------------------------------- | -------- | ------------------------------------------------------------ |
| Avg signer availability           | < 85%    | Lower t slightly or remove bad actors                        |
| Stalled signing sessions per week | > 2      | Investigate network latency / raise fee bump policy          |
| Reported offences                 | Frequent | Trigger resharing, re-evaluate membership                    |
| Filler (gap) tx frequency         | High     | Tune gas limits / pricing strategy; investigate stuck nonces |

#### H. Glossary (Quick)

* DKG – Distributed protocol to create a shared public key.
* Reshare – Refresh participant set / shares (with or without rotating key).
* t-of-n – Threshold number t of total participants n required to sign.
* Share – A participant’s fragment of the secret scalar.
* Aggregated Public Key – secp256k1 point derived from commitments; defines agent’s EVM address.
* Signing Session – One threshold signature production workflow for a message / transaction preimage.
