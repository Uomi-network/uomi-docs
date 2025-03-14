# ink! Environment

### Overview

This guide will help you set up your environment for ink! and Wasm smart contract development in UOMI.

> ℹ️ **Note**\
> Before proceeding, make sure your system meets the requirements for Rust development.

### What is ink!?

ink! is a Rust-based eDSL (embedded Domain Specific Language) developed by Parity Technologies. It's specifically designed for creating smart contracts that work with Substrate's `pallet-contracts`.

Rather than creating a new programming language, ink! adapts Rust's capabilities for smart contract development.

> 💡 **Tip**\
> Want to learn more about why ink! is a great choice for smart contract development? Check out the detailed benefits here.

#### Why WebAssembly?

Curious about the choice of WebAssembly for smart contracts? Find comprehensive [`explanations here.`](https://use.ink/why-rust-for-smart-contracts/)

### Setting Up Your Environment

#### 1. Installing Rust and Cargo

[Rust](https://www.rust-lang.org/) and Cargo are essential prerequisites for Wasm smart contract development.

**Linux and macOS**

```bash
# Download and install
curl https://sh.rustup.rs -sSf | sh

# Configure environment
source ~/.cargo/env
```

**Windows**

Visit the [Rust website](https://www.rust-lang.org/) and follow the Windows installation instructions.

#### 2. Configuring Rust

Set up your Rust environment with these commands:

```bash
rustup default stable
rustup update
rustup update nightly
rustup component add rust-src
rustup component add rust-src --toolchain nightly
rustup target add wasm32-unknown-unknown --toolchain nightly
```

> ⚠️ **Warning**
>
> Due to a bug in `cargo-contract`, building contracts with **rust nightly 1.70.0 or higher will fail**. It is advised to use rustc v1.69.0 or older until the issue is resolved from `cargo-contract` side. For better dev experience it is advised to create a [rust-toolchain file](https://rust-lang.github.io/rustup/overrides.html#the-toolchain-file) in the root of your project directory with following values.
>
> ```
> [toolchain]
> channel = "1.69.0"
> components = [ "rustfmt", "rust-src" ]
> targets = [ "wasm32-unknown-unknown" ]
> profile = "minimal"
> ```
>
> See more [here](https://github.com/paritytech/cargo-contract/issues/1058)

#### 3. Installing ink! CLI

The primary tool you'll need is `cargo-contract`, a CLI tool for managing WebAssembly smart contracts.

**Prerequisites**

First, install binaryen for WebAssembly bytecode optimization:

<details>

<summary>Debian/Ubuntu</summary>

```bash
apt-get update
apt-get -y install binaryen
```

</details>

<details>

<summary>ArchLinux</summary>

```bash
pacman -S binaryen
```

</details>

<details>

<summary>macOS</summary>

```bash
brew install binaryen
```

</details>

<details>

<summary>Windows</summary>

```bash
Find binary releases at https://github.com/WebAssembly/binaryen/releases
```

</details>

**Additional Dependencies**

Install required linking tools:

```bash
cargo install cargo-dylint dylint-link
```

**Installing cargo-contract**

```bash
cargo install cargo-contract --force --locked
```

> 💡 **Tip**\
> Use `--force` to ensure you get the latest version. For a specific version, add `--version X.X.X`

Example for specific version:

```bash
cargo install cargo-contract --force --version 1.5.1
```

Explore available commands with:

```bash
cargo contract --help
```

### Development Container

> 🔧 **Alternative Setup**\
> Skip manual installation by using our pre-configured development container.

Find detailed instructions for using our dev container in the swanky-dev-container Github repository.

### Additional Resources

* [ink! GitHub Repository](https://github.com/use-ink/ink)
* [Official Documentation](https://use.ink/)
* [Dev Container Guide](https://github.com/inkdevhub/swanky-dev-container)
