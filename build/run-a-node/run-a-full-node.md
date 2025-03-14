# Run a full node

### Overview[​](https://docs.astar.network/docs/build/nodes/full-node#overview) <a href="#overview" id="overview"></a>

Running a full node on Uomi allows you to connect to the network, sync with a bootnode, obtain local access to RPC endpoints, author blocks, and more.

Different from archive node, a full node discards all finalized blocks older than configured number of blocks (256 blocks by default). A full node occupies less storage space than an archive node because of pruning.

A full node may eventually be able to rebuild the entire chain with no additional information, and become an archive node, but at the time of writing, this is not implemented. If you need to query historical blocks past what you pruned, you need to purge your database and resync your node starting in archive mode. Alternatively you can use a backup or snapshot of a trusted source to avoid needing to sync from genesis with the network, and only need the blocks past that snapshot. (reference: [https://wiki.polkadot.network/docs/maintain-sync#types-of-nodes](https://wiki.polkadot.network/docs/maintain-sync#types-of-nodes))

If your node need to provide old historical blocks' data, please consider to use Archive node instead.

### Requirements[​](https://docs.astar.network/docs/build/nodes/full-node#requirements) <a href="#requirements" id="requirements"></a>

Requirements for running any node are similar to what we recommend for archive node. Read more about this [here](run-an-archive-node/). Note that Full node requires less disk space. Hard Disk requirement for Archive node is not applied to Full nodes.

To set a full node, you need to specify the number of blocks to be pruned:

```
--pruning 1000 \
```

{% hint style="info" %}
INFO

Running a node for our testnet 'Finney' requires less resources. It's a perfect place to test your node infrastructure and costs.
{% endhint %}
