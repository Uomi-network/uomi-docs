---
icon: sack-dollar
---

# Fees

## Transaction Fees in UOMI

### Overview

UOMI implements a dual fee system to support both native Substrate transactions and Ethereum-compatible operations. This hybrid approach ensures efficient resource allocation and network stability while maintaining compatibility with both ecosystems.

<figure><img src="../.gitbook/assets/opoc (5).svg" alt=""><figcaption></figcaption></figure>

### Native Transaction Fees

#### Fee Components

The native fee calculation follows the standard Substrate model:

```
Fee = Length Fee + Base Fee + (Weight Fee × Adjustment) + Tip
```

Where:

* **Length Fee**: Proportional to transaction byte size
* **Base Fee**: Fixed cost per transaction
* **Weight Fee**: Computational resources cost
* **Adjustment**: Dynamic scaling factor based on network congestion
* **Tip**: Optional priority payment

#### Implementation Details

The fee adjustment mechanism ensures network stability by:

* Scaling fees based on block space utilization
* Implementing surge pricing during high congestion
* Maintaining predictable base costs for standard operations

### Ethereum-Compatible Fees

#### Dynamic Fee Model

Based on the provided pallet code, UOMI implements EIP-1559 style fee calculation:

```
Fee = Gas Used × (Base Fee Per Gas + Priority Fee Per Gas)
```

#### Key Features

1. **Base Fee Adjustment**:

```rust
let weight_used = Permill::from_rational(
    weight.total().ref_time(), 
    max_weight.ref_time()
).clamp(lower, upper);
```

2. **Elasticity Mechanism**:

* Adjusts base fee according to block utilization
* Implements upper and lower bounds
* Maintains target block utilization

#### Technical Implementation

The base fee adjusts according to network conditions:

```rust
if usage > target {
    // Increase base fee when above target utilization
    let coef = Permill::from_parts(
        (usage.deconstruct() - target.deconstruct()) * 2u32
    );
} else if usage < target {
    // Decrease base fee when below target utilization
    let coef = Permill::from_parts(
        (target.deconstruct() - usage.deconstruct()) * 2u32
    );
}
```

### Fee Market Dynamics

#### Network Congestion Response

* Fees automatically adjust based on block fullness
* Target block utilization maintained through elasticity
* Smooth fee transitions prevent sudden spikes

#### Economic Incentives

1. **For Users**:
   * Predictable base fees
   * Optional priority fees for faster inclusion
   * Protection against fee spikes
2. **For Validators**:
   * Stable reward structure
   * Additional incentives during high demand
   * Protection against spam attacks

