---
icon: bugs
---

# Debug EVM Transactions

### Overview

UOMI provides advanced transaction tracing capabilities through Geth's debug APIs and OpenEthereum's trace module. These non-standard RPC methods offer deep insights into transaction processing and execution.

### Available Debug Methods

#### debug\_traceTransaction

Replays a transaction in the exact manner it was executed on the network.

```bash
curl http://127.0.0.1:9944 -H "Content-Type:application/json;charset=utf-8" -d \
  '{
    "jsonrpc":"2.0",
    "id":1,
    "method":"debug_traceTransaction",
    "params": ["YOUR-TRANSACTION-HASH"]
  }'
```

Optional parameters:

* `disableStorage`: (default: false) Disables storage capture
* `disableMemory`: (default: false) Disables memory capture
* `disableStack`: (default: false) Disables stack capture

#### debug\_traceBlock

Returns a full stack trace of all invoked opcodes for all transactions in a block.

Variants:

* `debug_traceBlockByHash`
* `debug_traceBlockByNumber`

#### debug\_traceCall

Executes an eth-call-like operation within the context of a given block.

#### trace\_filter

Filters and retrieves trace data based on specific criteria.

```bash
curl http://127.0.0.1:9944 -H "Content-Type:application/json;charset=utf-8" -d \
  '{
    "jsonrpc":"2.0",
    "id":1,
    "method":"trace_filter",
    "params":[{
      "fromBlock":"4142700",
      "toBlock":"4142800",
      "toAddress":["0xYOUR-ADDRESS"],
      "after":0,
      "count":20
    }]
  }'
```

Parameters:

* `fromBlock`: Starting block number
* `toBlock`: Ending block number
* `fromAddress`: Filter transactions from these addresses
* `toAddress`: Filter transactions to these addresses
* `after`: Trace offset (default: 0)
* `count`: Number of traces to return

> ⚠️ **Important Limits**
>
> * Maximum 500 trace entries per request
> * Trace cache duration: 300 seconds

### Running a Debug Node

To access these debugging features, you need to run a node with specific debug flags enabled.

#### Required Flags

```bash
--ethapi=debug     # Enables debug_traceTransaction
--ethapi=trace     # Enables trace_filter
--ethapi=txpool    # Enables transaction pool APIs
--runtime-cache-size 64
```

#### Optional Configurations

```bash
--ethapi-trace-max-count <number>    # Maximum trace entries
--ethapi-trace-cache-duration <seconds>    # Cache duration
```

### Transaction Pool API

Check the transaction pool status:

```bash
curl http://127.0.0.1:9944 -H "Content-Type:application/json;charset=utf-8" -d \
  '{
    "jsonrpc":"2.0",
    "id":1,
    "method":"txpool_status",
    "params":[]
  }'
```

> 💡 **Note**\
> The `txpool` API requires the `--ethapi=txpool` flag when starting the node.

### Best Practices

1. **Node Configuration**
   * Enable only the debug features you need
   * Consider memory usage when setting cache sizes
   * Monitor node performance with tracing enabled
2. **API Usage**
   * Use specific filters to limit data returned
   * Consider pagination for large trace requests
   * Cache commonly requested trace data

### Common Issues

* **Request Timeout**: Reduce the trace range or add more filters
* **Memory Issues**: Adjust cache size and duration
* **Missing Data**: Verify node sync status and cache duration
