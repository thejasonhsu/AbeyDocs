_Last updated: January 2026_

This guide provides an overview for **exchanges and custodial platforms** integrating with the **Abey** blockchain.
Abey is **EVM-compatible**, making integration straightforward for teams already supporting Ethereum.

---

## Overview

Abey is compatible with the Ethereum Virtual Machine (EVM) and exposes APIs equivalent to **go-ethereum (geth)**.

For exchanges that already support ETH, integration typically involves:

- Running an Abey full node
- Using standard Ethereum tooling and SDKs
- Signing transactions with **Abey’s Chain ID: `179`**

No protocol-level changes are required beyond chain-specific configuration.

---

## Running an Abey Node

### Obtain the Client

You can build Abey from source or download a prebuilt release:

- Source code: https://github.com/AbeyFoundation/go-abey
- Releases: https://github.com/AbeyFoundation/go-abey/releases

---

### Build from Source

Ensure a working Go environment is installed, then run:

```bash
git clone https://github.com/AbeyFoundation/go-abey.git
cd go-abey
git checkout release3.0
make gabey
```

---

### Start a Full Node with RPC Enabled

For exchange integration, it is recommended to run a **full or archive node** with RPC enabled:

```bash
./build/bin/gabey   --datadir ./data   --gcmode "archive"   --syncmode "full"   --rpc   --rpcaddr "0.0.0.0"   --rpccorsdomain "*"   --rpcvhosts "*"   --rpcapi "eth,abey,web3,net,impawn"   console
```

Use `gabey -h` to review all available configuration options.

**Security Warning**  
Binding RPC to `0.0.0.0` exposes the node to external access.  
In production, restrict access using firewalls or private networking.

---

## Interacting with Abey

Interacting with Abey is **identical to interacting with Ethereum** using geth-compatible APIs.

- JSON-RPC reference: `/RPC/json-rpc.md`
- All standard Ethereum calls are supported with Abey-specific extensions

### Enabled Namespaces

Note that the `personal` namespace is **disabled by default**.
To enable it, explicitly include it in the RPC configuration:

```text
--rpcapi "... ,personal"
```

---

## SDKs and Tooling

Abey works seamlessly with popular Ethereum SDKs and tools:

### Client Libraries

- **Java:** https://github.com/web3j/web3j
- **JavaScript:** https://web3js.readthedocs.io

### Go Integration

For Go-based backends and indexers, Abey provides a dedicated client library:

- https://github.com/AbeyFoundation/go-abey/tree/main/abeyclient

---

## Constructing Transactions

Abey transactions follow the **standard EVM transaction format**, with one important requirement:

### Chain ID

- **Abey Chain ID:** `179`

Transactions **must** be signed using this chain ID to be accepted by the network.

All common Ethereum tooling supports custom chain IDs, including:
- MetaMask
- Remix
- Truffle
- Hardhat

---

## Finality and Block Confirmation

Abey’s consensus mechanism provides:

- **Fast finality:** ~5 seconds
- **Irreversible blocks** after finalization

To query the most up-to-date finalized state, use the `"latest"` block parameter when requesting:
- Balances
- Blocks
- Transactions
- Contract state

---

## Best Practices for Exchanges

- Run **at least two nodes** for redundancy
- Use **archive nodes** for historical queries
- Restrict RPC access to trusted systems
- Monitor node sync status continuously
- Validate deposits after finality (not just inclusion)

---

## Summary

Abey integration is intentionally simple for Ethereum-compatible platforms:

- Same JSON-RPC APIs as geth
- Same SDKs and tooling
- One chain-specific change: **Chain ID = 179**

This allows exchanges to support Abey with minimal engineering effort.
