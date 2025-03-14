---
icon: file-lock
---

# Your first EVM Smart Contract

## Your First Smart Contract

### Overview

In this guide, we'll create, deploy, and interact with a simple smart contract on UOMI's EVM network using Hardhat. We'll build a basic "Counter" contract that can increment and retrieve a number.

### Prerequisites

Before starting, ensure you have:

* Node.js installed
* A code editor (VS Code recommended)
* A MetaMask wallet with some test tokens
* Basic knowledge of Solidity

### Project Setup

1. Create a new directory and initialize the project:

```bash
mkdir counter-contract
cd counter-contract
npm init -y
```

2. Install required dependencies:

```bash
npm install --save-dev hardhat @nomicfoundation/hardhat-toolbox
```

3. Create a Hardhat project:

```bash
npx hardhat init
```

Select "Create a JavaScript project" when prompted.

### Writing the Contract

Create a new file `contracts/Counter.sol`:

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.19;

contract Counter {
    uint256 private count;
    
    // Event to emit when the counter changes
    event CountUpdated(uint256 newCount);
    
    constructor() {
        count = 0;
    }
    
    function increment() public {
        count += 1;
        emit CountUpdated(count);
    }
    
    function getCount() public view returns (uint256) {
        return count;
    }
}
```

### Deployment Script

Create/modify `scripts/deploy.js`:

```javascript
async function main() {
  const Counter = await ethers.getContractFactory("Counter");
  const counter = await Counter.deploy();
  await counter.waitForDeployment();

  console.log("Counter deployed to:", await counter.getAddress());
}

main()
  .then(() => process.exit(0))
  .catch((error) => {
    console.error(error);
    process.exit(1);
  });
```

### Test the Contract

Create `test/Counter.js`:

```javascript
const { expect } = require("chai");

describe("Counter", function () {
  let counter;

  beforeEach(async function () {
    const Counter = await ethers.getContractFactory("Counter");
    counter = await Counter.deploy();
  });

  it("Should start with count of 0", async function () {
    expect(await counter.getCount()).to.equal(0);
  });

  it("Should increment count", async function () {
    await counter.increment();
    expect(await counter.getCount()).to.equal(1);
  });
});
```

Run the tests:

```bash
npx hardhat test
```

### Deploy to Testnet

1. Configure your network in `hardhat.config.js`:

```javascript
require("@nomicfoundation/hardhat-toolbox");

// Go to https://finney.uomi.ai and replace this with your own RPC URL
const FINNEY_RPC_URL = "https://finney.uomi.ai";

// Replace this private key with your own
// To export your private key from Metamask, go to Account Details > Export Private Key
const PRIVATE_KEY = "YOUR-METAMASK-PRIVATE-KEY";

module.exports = {
  solidity: "0.8.19",
  networks: {
    finney: {
      url: FINNEY_RPC_URL,
      accounts: [PRIVATE_KEY],
    },
  },
};
```

2. Deploy to Finney testnet:

```bash
npx hardhat run scripts/deploy.js --network finney
```

### Interact with Your Contract

After deployment, you can interact with your contract using:

1. The Hardhat console:

```bash
npx hardhat console --network finney
```

2. Example interactions:

```javascript
// Get the contract
const Counter = await ethers.getContractFactory("Counter");
const counter = await Counter.attach("YOUR-DEPLOYED-CONTRACT-ADDRESS");

// Get the current count
const count = await counter.getCount();
console.log("Current count:", count);

// Increment the counter
await counter.increment();
```

### Next Steps

Now that you've deployed your first contract, you can:

1. Add more functionality to your contract
2. Create a frontend to interact with it
3. Learn about contract security and best practices
4. Explore more complex smart contract patterns

### Common Issues and Solutions

> ⚠️ **Common Problems**
>
> * **Transaction Failed**: Make sure you have enough tokens for gas
> * **Contract Not Found**: Verify the contract address is correct
> * **Network Issues**: Ensure you're connected to the correct network

### Resources

* [Solidity Documentation](https://docs.soliditylang.org/)
* [Hardhat Documentation](https://hardhat.org/getting-started/)
* [OpenZeppelin Contracts](https://docs.openzeppelin.com/contracts/)
