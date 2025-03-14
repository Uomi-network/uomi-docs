# Basic ink! Contract

### Project Structure

Each ink! contract requires its own crate with two essential files:

* `Cargo.toml`: Project configuration and dependencies
* `lib.rs`: Contract implementation

> 💡 **Tip**\
> You can use Swanky Suite to quickly bootstrap a new project. Check out the Swanky CLI guide for details.

### Contract Configuration

#### Cargo.toml Setup

Your `Cargo.toml` should include the following sections:

```toml
[package]
name = "my_contract"
version = "0.1.0"
authors = ["Your Name <name@email.com>"]
edition = "2021"

[dependencies]
ink = { version = "4.3", default-features = false}
ink_metadata = { version = "4.3", features = ["derive"], optional = true }
scale = { package = "parity-scale-codec", version = "3", default-features = false, features = ["derive"] }
scale-info = { version = "2.5", default-features = false, features = ["derive"], optional = true }

[dev-dependencies]
ink_e2e = { version = "4.3" }

[lib]
path = "lib.rs"

[features]
default = ["std"]
std = [
    "ink/std",
    "scale/std",
    "scale-info/std"
]
ink-as-dependency = []
e2e-tests = []
```

### Contract Implementation

#### Minimum Requirements

Every ink! contract must include:

> ⚠️ **Required Elements**
>
> 1. `no_std` attribute for non-standard library compilation
> 2. Contract module marked with `#[ink::contract]`
> 3. Storage struct with `#[ink(storage)]`
> 4. At least one constructor with `#[ink(constructor)]`
> 5. At least one message with `#[ink(message)]`

#### Basic Contract Template

```rust
#![cfg_attr(not(feature = "std"), no_std)]

#[ink::contract]
mod my_contract {
    /// Contract storage
    #[ink(storage)]
    pub struct MyContract {}

    impl MyContract {
        /// Contract constructor
        #[ink(constructor)]
        pub fn new() -> Self {
            Self {}
        }

        /// Contract message
        #[ink(message)]
        pub fn do_something(&self) {
            ()
        }
    }
}
```

### Example Contracts

#### Flipper Contract

The [flipper contract](https://github.com/paritytech/ink-examples/blob/main/flipper/lib.rs) is the simplest example provided by the ink! team, perfect for understanding basic contract structure.

### Best Practices

1. **Project Organization**
   * Keep one contract per crate
   * Use meaningful names for contract modules
   * Organize tests in a separate module
2. **Code Structure**
   * Group related functionality together
   * Document your code with comments
   * Follow Rust naming conventions
3. **Testing**
   * Include unit tests
   * Add integration tests where needed
   * Use ink!'s testing utilities
