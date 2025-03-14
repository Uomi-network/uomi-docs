# Validator requirements

**Validator staking requirements**

* Bond: some UOMI tokens (not already defined)
* Meet hardware requirements

If your node stops producing blocks for 1 session, your node will be kicked out of the active set and 1% of the bonded funds will be slashed. Running a node with low performance can lead to skipping blocks which may result in being kicked out of the active set.

***

#### System requirements[​](https://docs.astar.network/docs/build/nodes/collator/requirements#system-requirements) <a href="#system-requirements" id="system-requirements"></a>

A validator can deploy its node on a local or remote server. You can choose your preferred provider for dedicated servers and operating system. Generally speaking, we recommand you to select a provider/server in your region, this will increase decentralization of the network. You can choose your preferred operating system, though we highly recommend Linux.

**Hardware requirements**

Use the charts below to find the basic configuration, which guarantees that all blocks can process in time. If the hardware doesn't meet these requirements, there is a high chance it will malfunction and you risk be automatically **kicked out and slashed** from the active set.



{% hint style="info" %}
**CAUTION**

Make sure your server is a **bare metal only dedicated to the validator node**, any unnecessary other process running on it will significantly decrease the validator performance. **We strongly discourage using a VPS** to run a validator because of their low performances.
{% endhint %}

Validator are the nodes which require the most powerful and fast machine, because they only have a very short time frame to assemble and validate it. To run a validator, it is absolutely necessary to use a **CPU of minimum 4 Ghz per core,** a **NVMe SSD disk** (SATA SSD are not suitable for validator because they are too slow) and 2 x RTX 4090 (or similar)

| Component | Min. requirement                  |
| --------- | --------------------------------- |
| System    | Ubuntu 22.04                      |
| CPU       | 12 cores - minimum 4 Ghz per core |
| Memory    | 32 GB                             |
| Hard Disk | 1 TB SSD NVMe                     |
| GPU       | 2 x RTX 4090                      |
