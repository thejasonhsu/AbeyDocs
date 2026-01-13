# Integrate an Exchange with Abey

## Overview

The objective of this document is to provide a brief overview of integrating with the EVM-compatible Abey blockchain. For teams that already support ETH, supporting Abey is as straightforward as spinning up an Abey node (which has the same API as go-ethereum) and populating Abey's chain ID (179) when constructing transactions.

## Integration using EVM Endpoints

### Running an Abey node

you can get it from [source code](https://github.com/abeychain/go-abey) or [release versin](https://github.com/abeychain/go-abey/releases).

from source code:

```
// make sure the golang env

git clone https://github.com/abeychain/go-abey.git
cd go-abey
git checkout release3.0
make gabey

```
Then start a node with the RPC service in the background. Use `gabey -h` get more details.

```
./build/bin/gabey --datadir ./data --gcmode "archive" --syncmode "full" --rpc --rpcaddr "0.0.0.0"  --rpccorsdomain "*" --rpcvhosts "*"  --rpcapi "eth,abey,web3,net,impawn" console 
```

## Interacting with Abey

Interacting with the Abey is identical to interacting with [go-ethereum](https://geth.ethereum.org/). You can find the reference material for ABEY API [here](/RPC/json-rpc.md).

Please note that personal_ namespace is turned off by default. To turn it on, you need to pass the appropriate command-line argument.


### Java SDK and Web3.js

you can use the [Java SDK](https://github.com/web3j/web3j) and [web3.js](https://web3js.readthedocs.io) libs interacting with Abey.

If you plan on extracting data from Abey into your own systems using golang, we recommend using our custom [abeyclient](https://github.com/abeychain/go-abey/tree/main/abeyclient).

### Constructing transactions

Abey transactions are identical to standard EVM transactions with one exceptions:

    They must be signed with Abey’s chain ID (179).


For development purposes, Abey supports all the popular tooling for Ethereum, such as `MetaMask and Remix`, `Truffle`, and `Hardhat`, so developers familiar with Ethereum and Solidity can feel right at home.


Abey consensus provides fast and irreversible finality within 5 seconds. To query the most up-to-date finalized block, query any value (i.e., block, balance, state, etc.) with the latest parameters.
