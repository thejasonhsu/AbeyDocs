_Last updated: January 2026_

# Abey Staking Guide

This guide explains how staking works on Abey and provides step-by-step instructions for running a validator and using the staking CLI.

---

## Overview

Abey adopts **Delegated Proof of Stake (DPoS)**.  
DPoS committee members use the **PBFT** protocol to produce fast blocks, which include transactions and confirm previous snail blocks.

Meanwhile, **fPoW miners** mine:
- **Fruits** (which reference fast blocks)
- **Snail blocks** (which include fruits)

This hybrid design helps resist long-range attacks and prevents chain tampering.

---

## Validator

Validators are DPoS committee members responsible for processing transactions and producing blocks.

- **Minimum stake:** 20,000 ABEY

### Recommended Validator Node Specifications

- 4 CPU cores  
- 16 GB RAM  
- 200 GB disk space  
- 4 Mbps network bandwidth  
- Public IP address (required for committee participation)

---

## Lock Period

Staked assets are subject to a lock period after unstaking.

- Lock duration: **25,000 fast blocks**  
- Approximate time: **15 days**

---

## Staking Guide

Staking and withdrawal operations are handled by a **system staking contract**. Anyone may interact with the contract directly, and the node also provides RPC APIs to query staking and reward information.

A dedicated **staking CLI** is provided for convenience.

---

## Staking Contract

The Abey staking contract is deployed at:

```
0x000000000000000000747275657374616b696E67
```

- [Full contract function definitions](https://github.com/abeyfoundation/go-abey/wiki/Staking-Contract)

---

## RPC and SDK

- [RPC API definitions](https://github.com/abeyfoundation/go-abey/wiki/RPC-API)

---

## Building the CLI

The **Abey Staking CLI** allows users to interact with the staking contract and participate in PoS consensus.

More details are available here:
- [Staking CLI (Impawn)](https://github.com/abeyfoundation/go-abey/wiki/Impawn-CLI)

---

## Building Abey

Follow the installation instructions for your platform:

- [macOS](https://github.com/abeyfoundation/go-abey/wiki/Installation-Instructions-for-Mac-OS-X)
- [Windows](https://github.com/abeyfoundation/go-abey/wiki/Installation-Instructions-for-Windows)
- Linux / Unix
  - [Ubuntu](https://github.com/abeyfoundation/go-abey/wiki/Installation-Instructions-for-Ubuntu)
  - [CentOS](https://github.com/abeyfoundation/go-abey/wiki/Installation-Instructions-for-Centos)
- Docker
  - [Running in Docker](https://github.com/abeyfoundation/go-abey/wiki/Running-in-Docker)

---

## Preparing the Node

### RPC Support

```bash
./gabey --rpc --rpcaddr 127.0.0.1 --rpcport 8545 --rpcapi "abey,net,web3,impawn" console
```

- Use `--rpcaddr 0.0.0.0` to listen on all interfaces.
- Use `127.0.0.1` to restrict RPC access to the local host.

### BFT Support

```bash
./gabey --bftip <PUBLIC_IP> console
```

Requirements:
- `bftip` must be a public IP
- Open firewall ports:
  - `8545` (RPC)
  - `30310` (BFT)
  - `30311` (BFT secondary)

### Example Startup Command

```bash
./gabey --datadir data --bftip "<PUBLIC_IP>" --rpc --rpcaddr "127.0.0.1" --rpcapi "eth,abey,net,web3,impawn" console
```

---

## Building from Source

Building `impawn` requires both Go and a C compiler.

```bash
cd go-abey/cmd/impawn
go build -o impawn main.go query_stake.go impawn.go
```

---

## Staking CLI Workflow

The staking workflow consists of the following steps:

1. **Impawn (Deposit)**  
   Deposit ≥ 20,000 ABEY to become a validator candidate.

2. **Cancel (Unstake)**  
   Signal intent to leave the committee in the current epoch.

3. **Withdraw**  
   After ~15 days, actively withdraw unlocked tokens.

Additional actions:
- Use **Append** if your stake is below the threshold
- Use **UpdateFee** to change validator fee rate
- Use **UpdatePK** to update validator public key
- Use **QueryReward** to check rewards and balances

---

## Impawn (Deposit)

```bash
./impawn --keystore <KEYSTORE_FILE> --rpcaddr <NODE_IP> --rpcport 8545 --value 20000 --fee 5000
```

**Parameters**
- `--keystore`: Keystore file containing the validator private key
- `--rpcaddr`, `--rpcport`: Validator node RPC endpoint
- `--value`: Deposit amount (must be ≥ 20,000 ABEY)
- `--fee`: Fee rate (0–10000), where `fee / 10000` is the percentage

---

## Cancel (Unstake)

```bash
./impawn --keystore <KEYSTORE_FILE> --rpcaddr <NODE_IP> --rpcport 8545 --value 10 cancel
```

Cancels part of the stake and moves it into the locked state.

---

## Query Staking

```bash
./impawn --keystore <KEYSTORE_FILE> --rpcaddr <NODE_IP> --rpcport 8545 querystaking
```

Displays:
- Staked balance
- Locked balance
- Unlocked balance
- Withdrawable block height

---

## Withdraw

```bash
./impawn --keystore <KEYSTORE_FILE> --rpcaddr <NODE_IP> --rpcport 8545 --value 10 withdraw
```

Withdraws unlocked stake after the lock period.

---

## Append Stake

```bash
./impawn --keystore <KEYSTORE_FILE> --rpcaddr <NODE_IP> --rpcport 8545 --value 10 append
```

Adds additional stake to an existing validator deposit.

---

## Update Fee

```bash
./impawn --keystore <KEYSTORE_FILE> --rpcaddr <NODE_IP> --rpcport 8545 --fee 6000 updatefee
```

Updates the validator’s fee rate.

---

## Update Public Key

```bash
./impawn --keystore <KEYSTORE_FILE> --rpcaddr <NODE_IP> --rpcport 8545 --bftkey <BFT_PRIVATE_KEY> updatepk
```

Updates the validator’s BFT public key used for committee communication.

---

## Query Rewards

```bash
./impawn --keystore <KEYSTORE_FILE> --rpcaddr <NODE_IP> --rpcport 8545 queryreward
```

Displays:
- Available balance
- Locked balance
- Rewards per snail block

---

## Send Transaction

```bash
./impawn --keystore <KEYSTORE_FILE> --rpcaddr <NODE_IP> --rpcport 8545 --value 10 --address <TO_ADDRESS> send
```

Sends a normal transfer transaction (non-contract call).

---

## Query Transaction

```bash
./impawn --rpcaddr <NODE_IP> --rpcport 8545 --txhash <TX_HASH> querytx
```

Queries a transaction by hash.
