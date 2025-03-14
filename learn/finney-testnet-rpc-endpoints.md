---
icon: list
---

# Finney Testnet RPC Endpoints

### Overview

The Finney testnet provides robust infrastructure through two dedicated RPC (Remote Procedure Call) endpoints. These endpoints serve as archive nodes, offering developers and users comprehensive access to the network's historical data and current state.

### Available Endpoints

<table><thead><tr><th width="286">Endpoint</th><th>Type</th><th>Status</th></tr></thead><tbody><tr><td><code>https://finney.uomi.ai</code></td><td>Primary Archive Node</td><td>Active</td></tr><tr><td><code>https://finney2.uomi.ai</code></td><td>Secondary Archive Node</td><td>Active</td></tr></tbody></table>

### Features

#### Archive Node Functionality

* Complete historical state access
* Full block history from genesis
* State queries at any block height
* Transaction receipt retrieval for all historical transactions

#### Supported Methods

Both endpoints support the standard JSON-RPC methods including:

* Ethereum JSON-RPC API (eth\_\*)
* Net API (net\_\*)
* Web3 API (web3\_\*)
* Debug API (debug\_\*)

### Usage Examples

#### Connecting with Web3.js

```javascript
const Web3 = require('web3');
const web3 = new Web3('https://finney.uomi.ai');
```

#### Connecting with Ethers.js

```javascript
const { ethers } = require('ethers');
const provider = new ethers.providers.JsonRpcProvider('https://finney.uomi.ai');
```

### Best Practices

1. **Load Balancing**
   * Alternate between both endpoints for optimal performance
   * Implement retry logic with endpoint switching
2. **Rate Limiting**
   * Respect rate limits to ensure fair usage
   * Implement appropriate caching strategies
3. **Error Handling**
   * Always implement proper error handling
   * Monitor response times and implement timeouts

### Support

For technical issues or questions:

* Join our Discord community
* Open a GitHub issue
* Contact our developer support team

### Network Parameters

* Chain ID: 4386
* Block Time: 3s

### Monitoring

Both endpoints are continuously monitored for:

* Uptime
* Response time
* Sync status
* Block height consistency
