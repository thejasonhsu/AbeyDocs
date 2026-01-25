_Last updated: January 2026_

# Command-Line Options Reference

This document provides a **complete reference** for all command-line options supported by the **Abey (`gabey`)** client.
Options are grouped by functionality for easier navigation and operational clarity.

---

## Usage

```bash
gabey [options] command [command options] [arguments...]
```

**Version:** `1.1.2-stable`  
**Copyright:** © 2018–2026 Abey Foundation

---

## Commands

| Command | Description |
|-------|-------------|
| `account` | Manage accounts |
| `attach` | Attach to a running node with a JavaScript console |
| `console` | Start an interactive JavaScript console |
| `bug` | Open a bug report window |
| `copydb` | Create a local chain from an existing chaindata folder |
| `dump` | Dump a specific block from storage |
| `dumpconfig` | Show resolved configuration values |
| `export` | Export blockchain data |
| `import` | Import blockchain data |
| `init` | Initialize a new genesis block |
| `js` | Execute JavaScript files |
| `license` | Display license information |
| `monitor` | Monitor and visualize node metrics |
| `removedb` | Remove blockchain and state databases |
| `version` | Print version information |
| `wallet` | Manage presale wallets |
| `help` | Show help information |

---

## Core Options

### Configuration and Storage

| Option | Description |
|------|-------------|
| `--config` | Path to TOML configuration file |
| `--datadir` | Data directory for blockchain and keystore |
| `--keystore` | Keystore directory (default: inside datadir) |
| `--nousb` | Disable USB hardware wallet support |
| `--identity` | Custom node name |

---

### Network Selection

| Option | Description |
|------|-------------|
| `--networkid` | Network identifier |
| `--testnet` | Enable test network |
| `--devnet` | Enable development network |

---

### Synchronization

| Option | Description |
|------|-------------|
| `--syncmode` | Sync mode (`fast`, `full`, `light`, `snapshot`) |
| `--gcmode` | Garbage collection mode (`full`, `archive`) |
| `--stategc` | Delete block bodies and receipts |

---

## Consensus and Committee Options

### Election

| Option | Description |
|------|-------------|
| `--election` | Enable validator election |

---

### BFT (Committee)

| Option | Description |
|------|-------------|
| `--bftip` | Committee node public IP |
| `--bftport` | Committee port (default: 30310) |
| `--bftport2` | Committee standby port (default: 30311) |
| `--bftkey` | Generate BFT private key |
| `--bftkeyhex` | Generate BFT private key (hex, testing only) |
| `--oldbft` | Run BFT over HTTP |

---

## Transaction Pool Options

Controls how pending transactions are queued and prioritized.

| Option | Description |
|------|-------------|
| `--txpool.nolocals` | Disable local transaction exemptions |
| `--txpool.journal` | Transaction journal file |
| `--txpool.rejournal` | Journal regeneration interval |
| `--txpool.pricelimit` | Minimum accepted gas price |
| `--txpool.pricebump` | Price bump percentage |
| `--txpool.accountslots` | Executable slots per account |
| `--txpool.globalslots` | Global executable slots |
| `--txpool.accountqueue` | Non-executable slots per account |
| `--txpool.globalqueue` | Global non-executable slots |
| `--txpool.lifetime` | Transaction lifetime |

---

## Fruit Pool Options

| Option | Description |
|------|-------------|
| `--fruitpool.journal` | Fruit journal file |
| `--fruitpool.rejournal` | Fruit journal regeneration interval |
| `--fruitpool.count` | Maximum pending fruits |

---

## Performance Tuning

| Option | Description |
|------|-------------|
| `--cache` | Cache size in MB |
| `--cache.database` | Cache percentage for database I/O |
| `--cache.gc` | Cache percentage for trie pruning |
| `--trie-cache-gens` | Trie generations kept in memory |

---

## Account Options

| Option | Description |
|------|-------------|
| `--unlock` | Accounts to unlock |
| `--password` | Password file |

---

## RPC, IPC, and Console

| Option | Description |
|------|-------------|
| `--rpc` | Enable HTTP-RPC |
| `--rpcaddr` | RPC bind address |
| `--rpcport` | RPC port |
| `--rpcapi` | APIs exposed over RPC |
| `--ws` | Enable WebSocket RPC |
| `--wsaddr` | WS bind address |
| `--wsport` | WS port |
| `--wsapi` | APIs exposed over WS |
| `--ipcdisable` | Disable IPC |
| `--ipcpath` | IPC socket path |
| `--rpccorsdomain` | CORS domains |
| `--rpcvhosts` | Allowed virtual hosts |
| `--jspath` | JavaScript root path |
| `--exec` | Execute JavaScript |
| `--preload` | Preload JavaScript files |

---

## Networking

| Option | Description |
|------|-------------|
| `--bootnodes` | Bootstrap nodes |
| `--port` | P2P port |
| `--maxpeers` | Maximum peers |
| `--nodiscover` | Disable peer discovery |
| `--nat` | NAT configuration |
| `--nodekey` | P2P node key file |

---

## Mining

| Option | Description |
|------|-------------|
| `--mine` | Enable mining |
| `--minefruit` | Mine fruits only |
| `--minerthreads` | Mining threads |
| `--coinbase` | Reward address |
| `--gastarget` | Target gas floor |
| `--gaslimit` | Target gas ceiling |
| `--gasprice` | Minimum gas price |
| `--extradata` | Extra block data |
| `--remote` | Enable remote mining |

---

## Logging and Debugging

| Option | Description |
|------|-------------|
| `--verbosity` | Log verbosity |
| `--vmodule` | Per-module verbosity |
| `--debug` | Include file and line numbers |
| `--metrics` | Enable metrics |
| `--pprof` | Enable pprof server |
| `--trace` | Write execution trace |

---

## Miscellaneous

| Option | Description |
|------|-------------|
| `--help`, `-h` | Show help |

---

## Notes

- Not all options are safe for production use
- Avoid exposing RPC interfaces publicly
- Prefer IPC for administrative operations
- Always test configuration changes on non-production nodes first
