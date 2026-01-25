_Last updated: January 2026_

# Abey Management API

Abey Management APIs provide administrative and operational control over a running **gabey** node.  
These APIs are exposed via **JSON-RPC** and organized by functional modules such as `admin`, `debug`, `miner`, `personal`, and others.

---

## Enabling Management APIs

`gabey` includes an interactive JavaScript console that supports all Management APIs.  
To expose these APIs over RPC, you must explicitly enable them using command-line flags.

### Example

```bash
gabey --ipcapi admin,abey,miner --rpcapi abey,web3 --rpc
```

This configuration:
1. Enables **admin**, **abey**, and **miner** APIs over IPC
2. Enables **abey** and **web3** APIs over HTTP RPC

⚠️ **Security Warning**

Exposing APIs over HTTP (`--rpc`) or WebSocket (`--ws`) allows any client with network access to call them.  
Enable only the APIs you need, especially on public or production systems.

By default:
- **IPC** exposes all APIs
- **HTTP / WS** expose only `db`, `abey`, `net`, and `web3`

---

## Discovering Enabled Modules

You can query enabled RPC modules using the `rpc_modules` method.

### Example (IPC)

```bash
echo '{"jsonrpc":"2.0","method":"rpc_modules","params":[],"id":1}' | nc -U $DATADIR/gabey.ipc
```

### Example Response

```json
{
  "id": 1,
  "jsonrpc": "2.0",
  "result": {
    "admin": "1.0",
    "debug": "1.0",
    "abey": "1.0",
    "miner": "1.0",
    "net": "1.0",
    "personal": "1.0",
    "rpc": "1.0",
    "txpool": "1.0",
    "web3": "1.0"
  }
}
```

---

## API Modules

The Management API is grouped into the following modules:

- **admin** — Node and peer management
- **debug** — Profiling, tracing, and logging
- **miner** — Mining and block production controls
- **personal** — Account and key management
- **txpool** — Transaction pool inspection
- **fruitpool** — Fruit and snail block pool inspection
- **abey** — Chain and protocol information

---

## Admin API

The `admin` module provides low-level control over networking and RPC services.

### `admin_addPeer`

Adds a static peer to the node.

```json
{"method":"admin_addPeer","params":["enode://..."]}
```

**Returns:** `true` if the peer was accepted.

---

### `admin_datadir`

Returns the absolute data directory used by the node.

```json
{"method":"admin_datadir","params":[]}
```

---

### `admin_nodeInfo`

Returns detailed networking and protocol information about the node.

```json
{"method":"admin_nodeInfo","params":[]}
```

---

## Debug API

The `debug` module provides tracing, profiling, and logging controls.

### `debug_traceTransaction`

Traces EVM execution for a transaction.

```javascript
debug.traceTransaction(txHash, options)
```

---

### `debug_verbosity`

Sets the global logging verbosity level.

```json
{"method":"debug_verbosity","params":[level]}
```

---

### `debug_vmodule`

Sets per-module or per-file logging verbosity.

```json
{"method":"debug_vmodule","params":["abey/*=6"]}
```

---

## Miner API

Controls block production and miner configuration.

### Common Methods

- `miner_start`
- `miner_stop`
- `miner_setGasPrice`
- `miner_setCoinbase`
- `miner_setExtra`

---

## Personal API

Manages local accounts and signing operations.

### `personal_newAccount`

Creates a new account in the keystore.

```json
{"method":"personal_newAccount","params":["passphrase"]}
```

---

### `personal_unlockAccount`

Unlocks an account for signing transactions.

```json
{"method":"personal_unlockAccount","params":[address, passphrase, duration]}
```

---

### `personal_sendTransaction`

Signs and submits a transaction without globally unlocking the account.

```json
{"method":"personal_sendTransaction","params":[tx, passphrase]}
```

---

## TxPool API

Inspects the transaction pool.

### Common Methods

- `txpool_content`
- `txpool_inspect`
- `txpool_status`

---

## Abey API

Provides chain-level and protocol information.

### Common Methods

- `abey_protocolVersion`
- `abey_syncing`
- `abey_coinbase`
- `abey_hashrate`
- `abey_gasPrice`
- `abey_accounts`

---

## Notes

- Management APIs are powerful and potentially dangerous
- Avoid exposing `admin`, `debug`, or `personal` APIs publicly
- Use IPC wherever possible for administrative access
