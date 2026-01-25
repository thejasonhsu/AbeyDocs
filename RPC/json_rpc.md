_Last updated: January 2026_

# Abey JSON-RPC API

This document describes the **Abey JSON-RPC API** exposed by the `gabey` client.
The API follows the **JSON-RPC 2.0** specification and is compatible with HTTP, IPC, WebSocket, and batch requests.

---

## Overview

- Protocol: **JSON-RPC 2.0**
- Transports: **HTTP, IPC, WebSocket**
- Batch requests: Supported
- Encoding: UTF-8 JSON

The API allows applications to interact with the Abey blockchain, query chain state, submit transactions, and manage staking via the `impawn` namespace.

---

## JSON-RPC Endpoint

### Default Endpoint

```
http://localhost:8545
```

### Enabling JSON-RPC

Start `gabey` with RPC enabled:

```bash
gabey --rpc
```

Customize the bind address and port:

```bash
gabey --rpc --rpcaddr <IP_ADDRESS> --rpcport <PORT>
```

### CORS (Browser Access)

To allow browser-based clients, enable CORS explicitly:

```bash
gabey --rpc --rpccorsdomain "http://localhost:3000"
```

⚠️ **Security Warning**

Do **not** expose JSON-RPC endpoints publicly unless absolutely necessary.  
Restrict access using firewalls and only enable required modules.

---

## Supported Features

- JSON-RPC 2.0
- Batch requests
- HTTP / IPC / WebSocket transports
- Experimental pub/sub support

---

## Default Block Parameter

Many RPC methods accept an optional **default block parameter** that specifies the blockchain state to query.

Supported values:

- `HEX_NUMBER` — Specific block number
- `"earliest"` — Genesis block
- `"latest"` — Most recent finalized block
- `"pending"` — Pending state (unconfirmed)

Common methods using this parameter include:
- `abey_getBalance`
- `abey_getTransactionCount`
- `abey_call`
- `abey_getCode`
- `impawn_getStakingAsset`

---

## Curl Usage Notes

When using `curl`, always specify the JSON content type:

```bash
-H "Content-Type: application/json"
```

Example:

```bash
curl -X POST   -H "Content-Type: application/json"   --data '{"jsonrpc":"2.0","method":"web3_clientVersion","params":[],"id":1}'   localhost:8545
```

---

## API Namespaces

The JSON-RPC API is organized into the following namespaces:

| Namespace | Description |
|---------|-------------|
| `web3`  | Client metadata and utilities |
| `net`   | Network information |
| `abey`  | Core blockchain and protocol APIs |
| `impawn` | Staking and delegation APIs |

---

## web3 API

### `web3_clientVersion`

Returns the current client version string.

### `web3_sha3`

Returns the Keccak-256 hash of the input data.

---

## net API

### `net_version`

Returns the network ID.

Known values:
- `19330` — Mainnet
- `18928` — Testnet
- `100` — Devnet

### `net_listening`

Returns `true` if the node is listening for peers.

### `net_peerCount`

Returns the number of connected peers.

---

## abey API

The `abey` namespace provides access to blockchain state, transactions, blocks, and mining information.

### Common Methods

- `abey_protocolVersion`
- `abey_syncing`
- `abey_coinbase`
- `abey_blockNumber`
- `abey_gasPrice`
- `abey_accounts`
- `abey_getBalance`
- `abey_getTransactionCount`
- `abey_sendTransaction`
- `abey_sendRawTransaction`
- `abey_call`
- `abey_estimateGas`
- `abey_getBlockByHash`
- `abey_getBlockByNumber`
- `abey_getTransactionByHash`
- `abey_getTransactionReceipt`
- `abey_getLogs`

These methods closely mirror Ethereum-compatible JSON-RPC APIs with Abey-specific extensions.

---

## impawn (Staking) API

The `impawn` namespace exposes staking-related RPC calls.

### Common Methods

- `impawn_getAllStakingAccount`
- `impawn_getStakingAsset`
- `impawn_getLockedAsset`
- `impawn_getAllCancelableAsset`
- `impawn_getStakingAccount`

These methods allow querying validator and delegator staking state.

---

## Notes and Best Practices

- Prefer **IPC** for administrative and staking operations
- Avoid exposing RPC APIs on public networks
- Enable only required namespaces
- Use rate limiting and firewall rules in production
- Validate all user-provided input in client applications

---

## References

- JSON-RPC Specification: https://www.jsonrpc.org/specification
- Abey RPC Pub/Sub: https://github.com/abeyfoundation/go-abey/wiki/RPC-PUB-SUB
