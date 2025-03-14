---
icon: helmet-safety
---

# HardHat

### Hardhat Setup

> 💡 **New to Hardhat?**\
> Check out the [Hardhat Quick Start Guide](https://hardhat.org/getting-started/#quick-start#overview) for basics.

#### Project Setup

**1. Configure Your Account**

To deploy contracts to UOMI networks, you'll need to export your private key from MetaMask:

1. Open MetaMask
2. Select your account
3. Click the three dots menu
4. Go to "Account Details"
5. Select "Export Private Key"
6. Enter your password to confirm

You'll get a 64-character hex string like:

```
60ed0dd24087f00faea4e2b556c74ebfa2f0e705f8169733b01530ce4c619883
```

**2. Store Your Private Key**

Create `private.json` in your project root:

```json
{
  "privateKey": "YOUR_PRIVATE_KEY_HERE"
}
```

> ⚠️ **Security Warning**\
> Never commit your private key to version control. Add `private.json` to your `.gitignore` file.

**3. Configure Networks**

Modify your `hardhat.config.js`:

```javascript
const { privateKey } = require("./private.json");

module.exports = {
  networks: {
    // Finney Testnet
    finney: {
      url: "https://finney.uomi.ai",
      chainId: XXX,
      accounts: [privateKey],
    },
  }
};
```

**4. Deploy Your Contract**

```bash
npx hardhat run --network finney scripts/deploy.js
```

### Truffle Setup

#### Prerequisites

Install the HD Wallet Provider:

```bash
npm install @truffle/hdwallet-provider
```

#### Configuration

Modify your `truffle-config.js`:

```javascript
const HDWalletProvider = require('@truffle/hdwallet-provider');
const { privateKey } = require('./private.json');

module.exports = {
  networks: {
    // Finney Testnet
    finney: {
      provider: () => new HDWalletProvider(
        privateKey,
        'https://finney.uomi.ai'
      ),
      network_id: XXX,
    },
  }
};
```

#### Deployment

Deploy to your chosen network:

```bash
truffle migrate --network finney
```

> 💡 **Note**\
> If no network is specified, Truffle will use the default development network.
