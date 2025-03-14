# Run an archive node

### Overview[​](https://docs.astar.network/docs/build/nodes/archive-node/#overview) <a href="#overview" id="overview"></a>

An **archive node** stores the history of past blocks. Most of times, an archive node is used as **RPC endpoint**. RPC plays a vital role on our network: it connects users and dApps to the blockchain through WebSocket and HTTP endpoints.&#x20;

**DApp projects** need to run their own RPC node as archive to the retrieve necessary blockchain data and not to rely on public infrastructure. Public endpoints respond slower because of the large amount of users connected and are rate limited.

{% hint style="warning" %}
**CAUTION**\
Be careful not to confuse with a **full node** that has a pruned database: a full node only stores the current state and most recent blocks (256 blocks by default) and uses much less storage space.
{% endhint %}

We maintain 2 different networks: the testnet Uomi Finney and the mainnet Uomi

| Type    | Name   | Token |
| ------- | ------ | ----- |
| Testnet | Finney | $UOMI |
| Mainnet | Uomi   | $UOMI |

### Requirements[​](https://docs.astar.network/docs/build/nodes/archive-node/#requirements) <a href="#requirements" id="requirements"></a>

#### Machine[​](https://docs.astar.network/docs/build/nodes/archive-node/#machine) <a href="#machine" id="machine"></a>

{% hint style="info" %}
**NOTE**

* Storage space will increase as the network grows.
* Archive nodes may require a larger server, depending on the amount and frequency of data requested by a dApp.
{% endhint %}

| Component | Min. requirement             |
| --------- | ---------------------------- |
| System    | Ubuntu 22.04                 |
| CPU       | 8 cores                      |
| Memory    | 16 GB                        |
| Hard Disk | 500 GB SSD (NVMe preferable) |

#### Ports[​](https://docs.astar.network/docs/build/nodes/archive-node/#ports) <a href="#ports" id="ports"></a>

The Uomi node needs different ports to run:

| Description | Port  | Custom Port Flag    |
| ----------- | ----- | ------------------- |
| P2P         | 30333 | `--port`            |
| RPC         | 9944  | `--rpc-port`        |
| Prometheus  | 9615  | `--prometheus-port` |

For all types of nodes, ports `30333` need to be opened for incoming traffic at the Firewall. **Validator nodes should not expose WS and RPC ports to the public.**

***

### Installation[​](https://docs.astar.network/docs/build/nodes/archive-node/#installation) <a href="#installation" id="installation"></a>

Using [Binary](binary.md) - run the node from binary file and set it up as systemd service

\
