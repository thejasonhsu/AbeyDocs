_Last updated: January 2026_

# Abey JSON-RPC API

This document includes complete, per-method JSON-RPC references for the Abey client **`gabey`**.
It includes supported transports, usage notes, and detailed documentation for every method listed in this file (parameters, returns, and examples).

## Quick Notes

- Default endpoint: `http://localhost:8545`
- Protocol: JSON-RPC 2.0
- Transports: HTTP / IPC / WebSocket
- Batch requests: Supported
- Pub/Sub: Experimental (see RPC-PUB-SUB link in this doc)

---

**Contents**
- [JSON RPC API](#json-rpc-api)
  - [JSON-RPC Endpoint](#json-rpc-endpoint)
    - [Go](#go)
  - [JSON-RPC support](#json-rpc-support)
  - [The default block parameter](#the-default-block-parameter)
  - [Curl Examples Explained](#curl-examples-explained)
  - [JSON-RPC methods](#json-rpc-methods)
  - [JSON RPC API Reference](#json-rpc-api-reference)
  - [web3](#web3)
      - [web3_clientVersion](#web3_clientversion)
        - [Parameters](#parameters)
        - [Returns](#returns)
        - [Example](#example)
      - [web3_sha3](#web3_sha3)
        - [Parameters](#parameters-1)
        - [Example Parameters](#example-parameters)
        - [Returns](#returns-1)
        - [Example](#example-1)
  - [net](#net)
      - [net_version](#net_version)
        - [Parameters](#parameters-2)
        - [Returns](#returns-2)
        - [Example](#example-2)
      - [net_listening](#net_listening)
        - [Parameters](#parameters-3)
        - [Returns](#returns-3)
        - [Example](#example-3)
      - [net_peerCount](#net_peercount)
        - [Parameters](#parameters-4)
        - [Returns](#returns-4)
        - [Example](#example-4)
  - [ABEY](#abey)
      - [abey_protocolVersion](#abey_protocolversion)
        - [Parameters](#parameters-5)
        - [Returns](#returns-5)
        - [Example](#example-5)
      - [abey_syncing](#abey_syncing)
        - [Parameters](#parameters-6)
        - [Returns](#returns-6)
        - [Example](#example-6)
      - [abey_coinbase](#abey_coinbase)
        - [Parameters](#parameters-7)
        - [Returns](#returns-7)
        - [Example](#example-7)
      - [abey_mining](#abey_mining)
        - [Parameters](#parameters-8)
        - [Returns](#returns-8)
        - [Example](#example-8)
      - [abey_hashrate](#abey_hashrate)
        - [Parameters](#parameters-9)
        - [Returns](#returns-9)
        - [Example](#example-9)
      - [abey_gasPrice](#abey_gasprice)
        - [Parameters](#parameters-10)
        - [Returns](#returns-10)
        - [Example](#example-10)
      - [abey_accounts](#abey_accounts)
        - [Parameters](#parameters-11)
        - [Returns](#returns-11)
        - [Example](#example-11)
      - [abey_blockNumber](#abey_blocknumber)
        - [Parameters](#parameters-12)
        - [Returns](#returns-12)
        - [Example](#example-12)
      - [abey_getBalance](#abey_getbalance)
        - [Parameters](#parameters-13)
        - [Example Parameters](#example-parameters-1)
        - [Returns](#returns-13)
        - [Example](#example-13)
      - [abey_getLockBalance](#abey_getlockbalance)
        - [Parameters](#parameters-14)
        - [Example Parameters](#example-parameters-2)
        - [Returns](#returns-14)
        - [Example](#example-14)
      - [abey_getTotalBalance](#abey_gettotalbalance)
        - [Parameters](#parameters-15)
        - [Example Parameters](#example-parameters-3)
        - [Returns](#returns-15)
        - [Example](#example-15)
      - [abey_getStorageAt](#abey_getstorageat)
        - [Parameters](#parameters-16)
        - [Returns](#returns-16)
        - [Example](#example-16)
      - [abey_getTransactionCount](#abey_gettransactioncount)
        - [Parameters](#parameters-17)
        - [Example Parameters](#example-parameters-4)
        - [Returns](#returns-17)
        - [Example](#example-17)
      - [abey_getBlockTransactionCountByHash](#abey_getblocktransactioncountbyhash)
        - [Parameters](#parameters-18)
        - [Example Parameters](#example-parameters-5)
        - [Returns](#returns-18)
        - [Example](#example-18)
      - [abey_getBlockTransactionCountByNumber](#abey_getblocktransactioncountbynumber)
        - [Parameters](#parameters-19)
        - [Example Parameters](#example-parameters-6)
        - [Returns](#returns-19)
        - [Example](#example-19)
      - [abey_getCode](#abey_getcode)
        - [Parameters](#parameters-20)
        - [Example Parameters](#example-parameters-7)
        - [Returns](#returns-20)
        - [Example](#example-20)
      - [abey_sign](#abey_sign)
        - [Parameters](#parameters-21)
        - [Returns](#returns-21)
        - [Example](#example-21)
      - [abey_sendTransaction](#abey_sendtransaction)
        - [Parameters](#parameters-22)
        - [Example Parameters](#example-parameters-8)
        - [Returns](#returns-22)
        - [Example](#example-22)
      - [abey_sendRawTransaction](#abey_sendrawtransaction)
        - [Parameters](#parameters-23)
        - [Example Parameters](#example-parameters-9)
        - [Returns](#returns-23)
        - [Example](#example-23)
      - [abey_sendAbeyRawTransaction](#abey_sendabeyrawtransaction)
        - [Parameters](#parameters-24)
        - [Example Parameters](#example-parameters-10)
        - [Returns](#returns-24)
        - [Example](#example-24)
      - [abey_call](#abey_call)
        - [Parameters](#parameters-25)
        - [Returns](#returns-25)
        - [Example](#example-25)
      - [abey_estimateGas](#abey_estimategas)
        - [Parameters](#parameters-26)
        - [Returns](#returns-26)
        - [Example](#example-26)
      - [abey_getBlockByHash](#abey_getblockbyhash)
        - [Parameters](#parameters-27)
        - [Example Parameters](#example-parameters-11)
        - [Returns](#returns-27)
        - [Example](#example-27)
      - [abey_getBlockByNumber](#abey_getblockbynumber)
        - [Parameters](#parameters-28)
        - [Example Parameters](#example-parameters-12)
        - [Returns](#returns-28)
        - [Example](#example-28)
      - [abey_getTransactionByHash](#abey_gettransactionbyhash)
        - [Parameters](#parameters-29)
        - [Example Parameters](#example-parameters-13)
        - [Returns](#returns-29)
        - [Example](#example-29)
      - [abey_getTransactionByBlockHashAndIndex](#abey_gettransactionbyblockhashandindex)
        - [Parameters](#parameters-30)
        - [Example Parameters](#example-parameters-14)
        - [Returns](#returns-30)
        - [Example](#example-30)
      - [abey_getTransactionByBlockNumberAndIndex](#abey_gettransactionbyblocknumberandindex)
        - [Parameters](#parameters-31)
        - [Example Parameters](#example-parameters-15)
        - [Returns](#returns-31)
        - [Example](#example-31)
      - [abey_getTransactionReceipt](#abey_gettransactionreceipt)
        - [Parameters](#parameters-32)
        - [Example Parameters](#example-parameters-16)
        - [Returns](#returns-32)
        - [Example](#example-32)
      - [abey_newFilter](#abey_newfilter)
        - [A note on specifying topic filters:](#a-note-on-specifying-topic-filters)
        - [Parameters](#parameters-33)
        - [Example Parameters](#example-parameters-17)
        - [Returns](#returns-33)
        - [Example](#example-33)
      - [abey_newBlockFilter](#abey_newblockfilter)
        - [Parameters](#parameters-34)
        - [Returns](#returns-34)
        - [Example](#example-34)
      - [abey_newPendingTransactionFilter](#abey_newpendingtransactionfilter)
        - [Parameters](#parameters-35)
        - [Returns](#returns-35)
        - [Example](#example-35)
      - [abey_uninstallFilter](#abey_uninstallfilter)
        - [Parameters](#parameters-36)
        - [Example Parameters](#example-parameters-18)
        - [Returns](#returns-36)
        - [Example](#example-36)
      - [abey_getFilterChanges](#abey_getfilterchanges)
        - [Parameters](#parameters-37)
        - [Example Parameters](#example-parameters-19)
        - [Returns](#returns-37)
        - [Example](#example-37)
      - [abey_getFilterLogs](#abey_getfilterlogs)
        - [Parameters](#parameters-38)
        - [Example Parameters](#example-parameters-20)
        - [Returns](#returns-38)
        - [Example](#example-38)
      - [abey_getLogs](#abey_getlogs)
        - [Parameters](#parameters-39)
        - [Example Parameters](#example-parameters-21)
        - [Returns](#returns-39)
        - [Example](#example-39)
      - [abey_getWork](#abey_getwork)
        - [Parameters](#parameters-40)
        - [Returns](#returns-40)
        - [Example](#example-40)
      - [abey_submitWork](#abey_submitwork)
        - [Parameters](#parameters-41)
        - [Example Parameters](#example-parameters-22)
        - [Returns](#returns-41)
        - [Example](#example-41)
      - [abey_submitHashrate](#abey_submithashrate)
        - [Parameters](#parameters-42)
        - [Example Parameters](#example-parameters-23)
        - [Returns](#returns-42)
        - [Example](#example-42)
      - [abey_committeeNumber](#abey_committeenumber)
        - [Parameters](#parameters-43)
        - [Returns](#returns-43)
        - [Example](#example-43)
      - [abey_fruitNumber](#abey_fruitnumber)
        - [Parameters](#parameters-44)
        - [Returns](#returns-44)
        - [Example](#example-44)
      - [abey_rewardSnailBlock](#abey_rewardsnailblock)
        - [Parameters](#parameters-45)
        - [Returns](#returns-45)
        - [Example](#example-45)
      - [abey_snailBlockNumber](#abey_snailblocknumber)
        - [Parameters](#parameters-46)
        - [Returns](#returns-46)
        - [Example](#example-46)
      - [abey_getCommittee](#abey_getcommittee)
        - [Parameters](#parameters-47)
        - [Returns](#returns-47)
        - [Example](#example-47)
      - [abey_getFruitByNumber](#abey_getfruitbynumber)
        - [Parameters](#parameters-48)
        - [Example Parameters](#example-parameters-24)
        - [Returns](#returns-48)
        - [Example](#example-48)
      - [abey_getFruitByHash](#abey_getfruitbyhash)
        - [Parameters](#parameters-49)
        - [Example Parameters](#example-parameters-25)
        - [Returns](#returns-49)
        - [Example](#example-49)
      - [abey_getBlockFruitCountByNumber](#abey_getblockfruitcountbynumber)
        - [Parameters](#parameters-50)
        - [Example Parameters](#example-parameters-26)
        - [Returns](#returns-50)
        - [Example](#example-50)
      - [abey_getBlockFruitCountByHash](#abey_getblockfruitcountbyhash)
        - [Parameters](#parameters-51)
        - [Example Parameters](#example-parameters-27)
        - [Returns](#returns-51)
        - [Example](#example-51)
      - [abey_getRewardBlock](#abey_getrewardblock)
        - [Parameters](#parameters-52)
        - [Example Parameters](#example-parameters-28)
        - [Returns](#returns-52)
        - [Example](#example-52)
      - [abey_getSnailBlockByNumber](#abey_getsnailblockbynumber)
        - [Parameters](#parameters-53)
        - [Example Parameters](#example-parameters-29)
        - [Returns](#returns-53)
        - [Example](#example-53)
      - [abey_getSnailBlockByHash](#abey_getsnailblockbyhash)
        - [Parameters](#parameters-54)
        - [Example Parameters](#example-parameters-30)
        - [Returns](#returns-54)
        - [Example](#example-54)
      - [abey_getDataset](#abey_getdataset)
        - [Parameters](#parameters-55)
        - [Returns](#returns-55)
        - [Example](#example-55)
      - [abey_getSnailRewardContent](#abey_getsnailrewardcontent)
        - [Parameters](#parameters-56)
        - [Returns](#returns-56)
        - [Example](#example-56)
      - [abey_getChainRewardContent](#abey_getchainrewardcontent)
        - [Parameters](#parameters-57)
        - [Returns](#returns-57)
        - [Example](#example-57)
      - [abey_balance](#abey_balance)
        - [Parameters](#parameters-58)
        - [Example Parameters](#example-parameters-31)
        - [Returns](#returns-58)
        - [Example](#example-58)
  - [Impawn](#impawn)
      - [impawn_getAllStakingAccount](#impawn_getallstakingaccount)
        - [Parameters](#parameters-59)
        - [Example Parameters](#example-parameters-32)
        - [Returns](#returns-59)
        - [Example](#example-59)
      - [impawn_getStakingAsset](#impawn_getstakingasset)
        - [Parameters](#parameters-60)
        - [Example Parameters](#example-parameters-33)
        - [Returns](#returns-60)
        - [Example](#example-60)
      - [impawn_getLockedAsset](#impawn_getlockedasset)
        - [Parameters](#parameters-61)
        - [Example Parameters](#example-parameters-34)
        - [Returns](#returns-61)
        - [Example](#example-61)
      - [impawn_getAllCancelableAsset](#impawn_getallcancelableasset)
        - [Parameters](#parameters-62)
        - [Example Parameters](#example-parameters-35)
        - [Returns](#returns-62)
        - [Example](#example-62)
      - [impawn_getStakingAccount](#impawn_getstakingaccount)
        - [Parameters](#parameters-63)
        - [Example Parameters](#example-parameters-36)
        - [Returns](#returns-63)
        - [Example](#example-63)
 

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# JSON-RPC API [JSON](http://json.org/) is a lightweight data-interchange format. It can represent numbers, strings, ordered sequences of values, and collections of name/value pairs.

[JSON-RPC](http://www.jsonrpc.org/specification) is a stateless, light-weight remote procedure call (RPC) protocol. Primarily this specification defines several data structures and the rules around their processing. It is transport agnostic in that the concepts can be used within the same process, over sockets, over HTTP, or in many various message passing environments. It uses JSON ([RFC 4627](http://www.ietf.org/rfc/rfc4627.txt)) as data format.

gabey has experimental pub/sub support. See [this](https://github.com/AbeyFoundation/go-abey/wiki/RPC-PUB-SUB) page for more information.


## JSON-RPC Endpoint

Default JSON-RPC endpoints:
http://localhost:8545 


### Go

You can start the HTTP JSON-RPC with the `--rpc` flag
```bash
gabey --rpc
```

change the default port (8545) and listening address (localhost) with:

```bash
gabey --rpc --rpcaddr <ip> --rpcport <portnumber>
```

If accessing the RPC from a browser, CORS will need to be enabled with the appropriate domain set. Otherwise, JavaScript calls are limited by the same-origin policy and requests will fail:

```bash
gabey --rpc --rpccorsdomain "http://localhost:3000"
```

## JSON-RPC support
JSON-RPC 2.0/ Batch requests/  HTTP/  IPC/  WS

## The default block parameter

The following methods have an extra default block parameter:

- [abey_getBalance](#abey_getbalance)
- [abey_balance](#abey_balance)
- [abey_getCode](#abey_getcode)
- [abey_getTransactionCount](#abey_gettransactioncount)
- [abey_getStorageAt](#abey_getstorageat)
- [abey_call](#abey_call)
- [impawn_getAllStakingAccount](#impawn_getAllStakingAccount)
- [impawn_getStakingAsset](#impawn_getStakingAsset)
- [impawn_getLockedAsset](#impawn_getLockedAsset)
- [impawn_getAllCancelableAsset](#impawn_getAllCancelableAsset)
- [impawn_getStakingAccount](#impawn_getStakingAccount)

When requests are made that act on the state of Abey, the last default block parameter determines the block height.

The following options are possible for the defaultBlock parameter:

- `HEX String` - an integer block number【
- `String "earliest"` for the earliest/genesis block
- `String "latest"` - for the latest mined block
- `String "pending"` - for the pending state/transactions

## Curl Examples Explained

The curl options below might return a response where the node complains about the content type, this is because the --data option sets the content type to application/x-www-form-urlencoded . If your node does complain, manually set the header by placing -H "Content-Type: application/json" at the start of the call.

The examples also do not include the URL/IP & port combination which must be the last argument given to curl e.x. 127.0.0.1:8545

## JSON-RPC methods

|   [abey](#abey)   |  [impawn](#impawn) |[web3](#web3) |[net](#net) |
| :------------------: | :-----------------:|:-----------------: |:-----------------: |
| [protocolVersion](#abey_protocolVersion)|[getAllStakingAccount](#impawn_getAllStakingAccount)|[clientVersion](#web3_clientVersion)|[version](#net_version)|
|[syncing](#abey_syncing)|[getStakingAsset](#impawn_getStakingAsset)|[sha3](#web3_sha3)| [peerCount](#net_peercount)|
|[coinbase](#abey_coinbase)|[getLockedAsset](#impawn_getLockedAsset)||[listening](#net_listening)|
|[mining](#abey_mining)|[getAllCancelableAsset](#impawn_getAllCancelableAsset)|
|[hashrate](#abey_hashrate)|[getStakingAccount](#impawn_getStakingAccount)|
|[gasPrice](#abey_gasPrice)|
|[accounts](#abey_accounts)|
|[blockNumber](#abey_blockNumber) |
|[getBalance](#abey_getBalance)|
|[getLockBalance](#abey_getLockBalance)|
|[getTotalBalance](#abey_getTotalBalance)|
|[balance](#abey_balance)|
|[getStorageAt](#abey_getStorageAt) |
| [getTransactionCount](#abey_getTransactionCount)|
| [getBlockTransactionCountByHash](#abey_getBlockTransactionCountByHash) |
| [getBlockTransactionCountByNumber](#abey_getBlockTransactionCountByNumber) |
|         [getCode](#abey_getCode)            |
|           [sign](#abey_sign)                |
|     [sendTransaction](#abey_sendTransaction)      
|    [sendRawTransaction](#abey_sendRawTransaction)   |
| [sendAbeyRawTransaction](#abey_sendAbeyRawTransaction)   | -|
|           [call](#abey_call)          |
|       [estimateGas](#abey_estimateGas)             |
| [getBlockByHash](#abey_getblockbyhash)         | 
| [getBlockByNumber](#abey_getblockbynumber)         | 
|[getTransactionByHash](#abey_gettransactionbyhash) | 
|[getTransactionByBlockHashAndIndex](#abey_gettransactionbyblockhashandindex) | 
|[getTransactionByBlockNumberAndIndex](#abey_gettransactionbyblocknumberandindex) | 
|  [getTransactionReceipt](#abey_gettransactionreceipt)   |
|[newFilter](#abey_newfilter)|
|[newBlockFilter](#abey_newblockfilter)|
|[newPendingTransactionFilter](#abey_newpendingtransactionfilter)|
|[uninstallFilter](#abey_uninstallfilter)|
|[getFilterChanges](#abey_getfilterchanges)|
|[getFilterLogs](#abey_getfilterlogs)|
|[getLogs](#abey_getlogs)|
|[getWork](#abey_getwork)|
|[submitWork](#abey_submitwork)|
|[submitHashrate](#abey_submithashrate)|
|     [committeeNumber](#abey_committeeNumber)      |
|       [fruitNumber](#abey_fruitNumber)        |
|     [rewardSnailBlock](#abey_rewardSnailBlock)     |
|     [snailBlockNumber](#abey_snailBlockNumber)     |
|       [getCommittee](#abey_getCommittee)       |
|  [getFruitByNumber](#abey_getFruitByNumber)         |
|  [getFruitByHash](#abey_getFruitByHash)         |
|  [abey_getBlockFruitCountByNumber](#abey_getBlockFruitCountByNumber)         |
|  [abey_getBlockFruitCountByHash](#abey_getBlockFruitCountByHash)         |
|      [getRewardBlock](#abey_getRewardBlock)      |
|         [getSnailBlockByNumber](#abey_getSnailBlockByNumber)    | 
|         [getSnailBlockByHash](#abey_getSnailBlockByHash)    | 
|         [getDataset](#abey_getDataset)    | 
|         [getSnailRewardContent](#abey_getSnailRewardContent)    | 
|                            |



## JSON RPC API Reference

***

## web3

#### web3_clientVersion

Returns the current client version.

##### Parameters
none

##### Returns

`String` - The current client version.

##### Example
```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"web3_clientVersion","params":[],"id":67}'

// Result
{
  "id":67,
  "jsonrpc":"2.0",
  "result": "gabey/v1.1.0-unstable-d4c05e98/linux-amd64/go1.10"
}
```

***

#### web3_sha3

Returns Keccak-256 (*not* the standardized SHA3-256) of the given data.

##### Parameters

1. `DATA` - the data to convert into a SHA3 hash.

##### Example Parameters
```js
params: [
  "0x68656c6c6f20776f726c64"
]
```

##### Returns

`DATA` - The SHA3 result of the given string.

##### Example
```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"web3_sha3","params":["0x68656c6c6f20776f726c64"],"id":64}'

// Result
{
  "id":64,
  "jsonrpc": "2.0",
  "result": "0x47173285a8d7341e5e972fc677286384f802f8ef42a5ec5f03bbfa254cb01fad"
}
```

***

## net

#### net_version

Returns the current network id.

##### Parameters
none

##### Returns

`String` - The current network id.
- `"19330"`: Abey Mainnet
- `"18928"`: Testnet
- `"100"`:   Devnet

##### Example
```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"net_version","params":[],"id":67}'

// Result
{
  "id":67,
  "jsonrpc": "2.0",
  "result": "19330"
}
```

***

#### net_listening

Returns `true` if client is actively listening for network connections.

##### Parameters
none

##### Returns

`Boolean` - `true` when listening, otherwise `false`.

##### Example
```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"net_listening","params":[],"id":67}'

// Result
{
  "id":67,
  "jsonrpc":"2.0",
  "result":true
}
```

***

#### net_peerCount

Returns number of peers currently connected to the client.

##### Parameters
none

##### Returns

`QUANTITY` - integer of the number of connected peers.

##### Example
```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"net_peerCount","params":[],"id":74}'

// Result
{
  "id":74,
  "jsonrpc": "2.0",
  "result": "0x2" // 2
}
```

***
## ABEY

#### abey_protocolVersion

Returns the current Abey protocol version.

##### Parameters
none

##### Returns

`String` - The current Abey protocol version.

##### Example
```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"abey_protocolVersion","params":[],"id":67}'

// Result
{
  "id":67,
  "jsonrpc": "2.0",
  "result": "0x40"
}
```

***

#### abey_syncing

Returns an object with data about the sync status or `false`.


##### Parameters
none

##### Returns

`Object|Boolean`, An object with sync status data or `FALSE`, when not syncing:
  - `currentFastBlock`: `QUANTITY` -current block number(fastchain)
  - `currentSnailBlock`: `QUANTITY` -current block number(snailchain)
  - `highestFastBlock`: `QUANTITY` - already highest block number(fastchain)
  - `highestSnailBlock`: `QUANTITY` -already highest block number(snailchain)
  - `knownStates`:  `String` -already know state 
  - `pulledStates`: `String` -already complete state
  - `startingFastBlock`: `QUANTITY` -start sync block number(fastchain)
  - `startingSnailBlock`: `QUANTITY` -start sync block number(snailchain)

##### Example
```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"abey_syncing","params":[],"id":1}'

// Result
{
	"jsonrpc": "2.0",
	"id": 1,
	"result": {
		"currentFastBlock": "0x2e9a",
		"currentSnailBlock": "0xab",
		"highestFastBlock": "0x3a3d2",
		"highestSnailBlock": "0xab7",
		"knownStates": "0x0",
		"pulledStates": "0x0",
		"startingFastBlock": "0x2e98",
		"startingSnailBlock": "0x0"
	}
}
// Or when not syncing
{
  "id":1,
  "jsonrpc": "2.0",
  "result": false
}
```

***

#### abey_coinbase

Returns the client coinbase address.


##### Parameters
none

##### Returns

`DATA`, 20 bytes - the current coinbase address.

##### Example
```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"abey_coinbase","params":[],"id":64}'

// Result
{
  "id":64,
  "jsonrpc": "2.0",
  "result": "0xc94770007dda54cF92009BFF0dE90c06F603a09f"
}
```

***

#### abey_mining

Returns `true` if client is actively mining new blocks.

##### Parameters
none

##### Returns

`Boolean` - returns `true` of the client is mining, otherwise `false`.

##### Example
```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"abey_mining","params":[],"id":71}'

// Result
{
  "id":71,
  "jsonrpc": "2.0",
  "result": true
}

```

***

#### abey_hashrate

Returns the number of hashes per second that the node is mining with.

##### Parameters
none

##### Returns

`QUANTITY` - number of hashes per second.

##### Example
```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"abey_hashrate","params":[],"id":71}'

// Result
{
  "id":71,
  "jsonrpc": "2.0",
  "result": "0x38a"
}

```

***

#### abey_gasPrice

Returns the current price per gas in wei.

##### Parameters
none

##### Returns

`QUANTITY` - integer of the current gas price in wei.

##### Example
```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"abey_gasPrice","params":[],"id":73}'

// Result
{
  "id":73,
  "jsonrpc": "2.0",
  "result": "0xf4240" // 1000000
}
```

***

#### abey_accounts

Returns a list of addresses owned by client.


##### Parameters
none

##### Returns

`Array of DATA`, 20 Bytes - addresses owned by the client.

##### Example
```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"abey_accounts","params":[],"id":1}'

// Result
{
  "id":1,
  "jsonrpc": "2.0",
  "result": ["0xc94770007dda54cF92009BFF0dE90c06F603a09f"]
}
```

***

#### abey_blockNumber

Returns the number of most recent block.

##### Parameters
none

##### Returns

`QUANTITY` - integer of the current block number the client is on.

##### Example
```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"abey_blockNumber","params":[],"id":1}'

// Result
{
  "id":83,
  "jsonrpc": "2.0",
  "result": "0xc94" // 1207
}
```

***

#### abey_getBalance

Returns the balance of the account of given address.

##### Parameters

1. `DATA`, 20 Bytes - address to check for balance.
2. `QUANTITY|TAG` - integer block number, or the string `"latest"`, `"earliest"` or `"pending"`, see the [default block parameter](#the-default-block-parameter)

##### Example Parameters
```js
params: [
   '0xc94770007dda54cF92009BFF0dE90c06F603a09f',
   'latest'
]
```

##### Returns

`QUANTITY` - integer of the current balance in wei.


##### Example
```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"abey_getBalance","params":["0xc94770007dda54cF92009BFF0dE90c06F603a09f", "latest"],"id":1}'

// Result
{
  "id":1,
  "jsonrpc": "2.0",
  "result": "0x0234c8a3397aab58" // 158972490234375000
}
```

***

#### abey_getLockBalance

Returns the locked balance of the account of given address.

##### Parameters

1. `DATA`, 20 Bytes - address to check for balance.
2. `QUANTITY|TAG` - integer block number, or the string `"latest"`, `"earliest"` or `"pending"`, see the [default block parameter](#the-default-block-parameter)

##### Example Parameters
```js
params: [
   '0xc94770007dda54cF92009BFF0dE90c06F603a09f',
   'latest'
]
```

##### Returns

`QUANTITY` - integer of the current balance in wei.


##### Example
```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"abey_getLockBalance","params":["0xc94770007dda54cF92009BFF0dE90c06F603a09f", "latest"],"id":1}'

// Result
{
  "id":1,
  "jsonrpc": "2.0",
  "result": "0x0234c8a3397aab58" // 158972490234375000
}
```

***

#### abey_getTotalBalance

Returns the total balance of the account of given address.

##### Parameters

1. `DATA`, 20 Bytes - address to check for balance.
2. `QUANTITY|TAG` - integer block number, or the string `"latest"`, `"earliest"` or `"pending"`, see the [default block parameter](#the-default-block-parameter)

##### Example Parameters
```js
params: [
   '0xc94770007dda54cF92009BFF0dE90c06F603a09f',
   'latest'
]
```

##### Returns

`QUANTITY` - integer of the current balance in wei.


##### Example
```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"abey_getTotalBalance","params":["0xc94770007dda54cF92009BFF0dE90c06F603a09f", "latest"],"id":1}'

// Result
{
  "id":1,
  "jsonrpc": "2.0",
  "result": "0x0234c8a3397aab58" // 158972490234375000
}
```

***

#### abey_getStorageAt

Returns the value from a storage position at a given address. 

##### Parameters

1. `DATA`, 20 Bytes - address of the storage.
2. `QUANTITY` - integer of the position in the storage.
3. `QUANTITY|TAG` - integer block number, or the string `"latest"`, `"earliest"` or `"pending"`, see the [default block parameter](#the-default-block-parameter)

##### Returns

`DATA` - the value at this storage position.

##### Example
Calculating the correct position depends on the storage to retrieve. Consider the following contract deployed at `0x295a70b2de5e3953354a6a8344e616ed314d7251` by address `0x391694e7e0b0cce554cb130d723a9d27458f9298`.

```
contract Storage {
    uint pos0;
    mapping(address => uint) pos1;
    
    function Storage() {
        pos0 = 1234;
        pos1[msg.sender] = 5678;
    }
}
```

Retrieving the value of pos0 is straight forward:

```js
curl -X POST --data '{"jsonrpc":"2.0", "method": "abey_getStorageAt", "params": ["0x295a70b2de5e3953354a6a8344e616ed314d7251", "0x0", "latest"], "id": 1}' localhost:8545

{"jsonrpc":"2.0","id":1,"result":"0x00000000000000000000000000000000000000000000000000000000000004d2"}
```

Retrieving an element of the map is harder. The position of an element in the map is calculated with:
```js
keccak(LeftPad32(key, 0), LeftPad32(map position, 0))
```

This means to retrieve the storage on pos1["0x391694e7e0b0cce554cb130d723a9d27458f9298"] we need to calculate the position with:
```js
keccak(decodeHex("000000000000000000000000391694e7e0b0cce554cb130d723a9d27458f9298" + "0000000000000000000000000000000000000000000000000000000000000001"))
```
The gabey console which comes with the web3 library can be used to make the calculation:
```js
> var key = "000000000000000000000000391694e7e0b0cce554cb130d723a9d27458f9298" + "0000000000000000000000000000000000000000000000000000000000000001"
undefined
> web3.sha3(key, {"encoding": "hex"})
"0x6661e9d6d8b923d5bbaab1b96e1dd51ff6ea2a93520fdc9eb75d059238b8c5e9"
```
Now to fetch the storage:
```js
curl -X POST --data '{"jsonrpc":"2.0", "method": "abey_getStorageAt", "params": ["0x295a70b2de5e3953354a6a8344e616ed314d7251", "0x6661e9d6d8b923d5bbaab1b96e1dd51ff6ea2a93520fdc9eb75d059238b8c5e9", "latest"], "id": 1}' localhost:8545

{"jsonrpc":"2.0","id":1,"result":"0x000000000000000000000000000000000000000000000000000000000000162e"}

```

***

#### abey_getTransactionCount

Returns the number of transactions *sent* from an address.


##### Parameters

1. `DATA`, 20 Bytes - address.
2. `QUANTITY|TAG` - integer block number, or the string `"latest"`, `"earliest"` or `"pending"`, see the [default block parameter](#the-default-block-parameter)

##### Example Parameters
```js
params: [
   '0xc94770007dda54cF92009BFF0dE90c06F603a09f',
   'latest' // state at the latest block
]
```

##### Returns

`QUANTITY` - integer of the number of transactions send from this address.


##### Example
```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"abey_getTransactionCount","params":["0xc94770007dda54cF92009BFF0dE90c06F603a09f","latest"],"id":1}'

// Result
{
  "id":1,
  "jsonrpc": "2.0",
  "result": "0x1" // 1
}
```

***

#### abey_getBlockTransactionCountByHash

Returns the number of transactions in a block from a block matching the given block hash.


##### Parameters

1. `DATA`, 32 Bytes - hash of a block.

##### Example Parameters
```js
params: [
   '0xb903239f8543d04b5dc1ba6579132b143087c68db1b2168786408fcbce568238'
]
```

##### Returns

`QUANTITY` - integer of the number of transactions in this block.


##### Example
```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"abey_getBlockTransactionCountByHash","params":["0xb903239f8543d04b5dc1ba6579132b143087c68db1b2168786408fcbce568238"],"id":1}'

// Result
{
  "id":1,
  "jsonrpc": "2.0",
  "result": "0xc" // 11
}
```

***

#### abey_getBlockTransactionCountByNumber
> > 
Returns the number of transactions in a block matching the given block number.


##### Parameters

1. `QUANTITY|TAG` - integer of a block number, or the string `"earliest"`, `"latest"` or `"pending"`, as in the [default block parameter](#the-default-block-parameter).

##### Example Parameters
```js
params: [
   '0xe8', // 232
]
```

##### Returns

`QUANTITY` - integer of the number of transactions in this block.

##### Example
```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"abey_getBlockTransactionCountByNumber","params":["0xe8"],"id":1}'

// Result
{
  "id":1,
  "jsonrpc": "2.0",
  "result": "0xa" // 10
}
```

***

#### abey_getCode

Returns code at a given address.


##### Parameters

1. `DATA`, 20 Bytes - address.
2. `QUANTITY|TAG` - integer block number, or the string `"latest"`, `"earliest"` or `"pending"`, see the [default block parameter](#the-default-block-parameter).

##### Example Parameters
```js
params: [
   '0xa94f5374fce5edbc8e2a8697c15331677e6ebf0b',
   '0x2'  // 2
]
```

##### Returns

`DATA` - the code from the given address.


##### Example
```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"abey_getCode","params":["0xa94f5374fce5edbc8e2a8697c15331677e6ebf0b", "0x2"],"id":1}'

// Result
{
  "id":1,
  "jsonrpc": "2.0",
  "result": "0x600160008035811a818181146012578301005b601b6001356025565b8060005260206000f25b600060078202905091905056"
}
```

***

#### abey_sign

The sign method calculates an Abey specific signature with: `sign(keccak256("\x19abey Signed Message:\n" + len(message) + message)))`.

Adding a prefix to the message makes the calculated signature recognisable as an Abey-specific signature. This prevents misuse where a malicious dApp can sign arbitrary data and use the signature to impersonate the victim.

**Note** the address to sign with must be unlocked. 

##### Parameters
account, message

1. `DATA`, 20 Bytes - address.
2. `DATA`, N Bytes - message to sign.

##### Returns

`DATA`: Signature

##### Example

```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"abey_sign","params":["0x9b2055d370f73ec7d8a03e965129118dc8f5bf83", "0xdeadbeaf"],"id":1}'

// Result
{
  "id":1,
  "jsonrpc": "2.0",
  "result": "0xa3f20717a250c2b0b729b7e5becbff67fdaef7e0699da4de7ca5895b02a170a12d887fd3b17bfdce3481f10bea41f45ba9f709d39ce8325427b57afcfc994cee1b"
}
```

An example how to use solidity ecrecover to verify the signature calculated with `abey_sign` can be found [here](https://gist.github.com/bas-vk/d46d83da2b2b4721efb0907aecdb7ebd). The contract is deployed on the testnet Ropsten and Rinkeby.

***

#### abey_sendTransaction

Creates new message call transaction or a contract creation, if the data field contains code.

##### Parameters

1. `Object` - The transaction object
  - `from`: `DATA`, 20 Bytes - The address the transaction is send from.
  - `to`: `DATA`, 20 Bytes - (optional when creating new contract) The address the transaction is directed to.
  - `gas`: `QUANTITY`  - (optional, default: 90000) Integer of the gas provided for the transaction execution. It will return unused gas.
  - `gasPrice`: `QUANTITY`  - (optional, default: To-Be-Determined) Integer of the gasPrice used for each paid gas
  - `value`: `QUANTITY`  - (optional) Integer of the value sent with this transaction
  - `data`: `DATA`  - The compiled code of a contract OR the hash of the invoked method signature and encoded parameters. 
  - `nonce`: `QUANTITY`  - (optional) Integer of a nonce. This allows to overwrite your own pending transactions that use the same nonce.

##### Example Parameters
```js
params: [{
  "from": "0xb60e8dd61c5d32be8058bb8eb970870f07233155",
  "to": "0xd46e8dd67c5d32be8058bb8eb970870f07244567",
  "gas": "0x76c0", // 30400
  "gasPrice": "0x9184e72a000", // 10000000000000
  "value": "0x9184e72a", // 2441406250
  "data": "0xd46e8dd67c5d32be8d46e8dd67c5d32be8058bb8eb970870f072445675058bb8eb970870f072445675"
}]
```

##### Returns

`DATA`, 32 Bytes - the transaction hash, or the zero hash if the transaction is not yet available.

Use [abey_getTransactionReceipt](#abey_gettransactionreceipt) to get the contract address, after the transaction was mined, when you created a contract.

##### Example
```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"abey_sendTransaction","params":[{see above}],"id":1}'

// Result
{
  "id":1,
  "jsonrpc": "2.0",
  "result": "0xe670ec64341771606e55d6b4ca35a1a6b75ee3d5145a99d05921026d1527331"
}
```

***

#### abey_sendRawTransaction

Creates new message call transaction or a contract creation for signed transactions.

##### Parameters

1. `DATA`, The signed transaction data.

##### Example Parameters
```js
params: ["0xd46e8dd67c5d32be8d46e8dd67c5d32be8058bb8eb970870f072445675058bb8eb970870f072445675"]
```

##### Returns

`DATA`, 32 Bytes - the transaction hash, or the zero hash if the transaction is not yet available.

Use [abey_getTransactionReceipt](#abey_gettransactionreceipt) to get the contract address, after the transaction was mined, when you created a contract.

##### Example
```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"abey_sendRawTransaction","params":[{see above}],"id":1}'

// Result
{
  "id":1,
  "jsonrpc": "2.0",
  "result": "0xe670ec64341771606e55d6b4ca35a1a6b75ee3d5145a99d05921026d1527331"
}
```

***

#### abey_sendAbeyRawTransaction

When transaction contain payer or fee,Creates new message call transaction or a contract creation for signed transactions.

##### Parameters

1. `DATA`, The signed transaction data.

##### Example Parameters
```js
params: ["0xf8c60183989680834c4b4094bea78fea68dba84363d0f9b79219ddf5991ccb2a880de0b6b3a76400008094cfb7ec3ac64a3afde043a5b32212d0b9c25b5d808081eba07cc4b8300a8ab6a7d6aee713f6dc61311848bf827794c370873ca334e7cc2cc1a05cd365ffc46cada820911e3c11123e36245ed1cec7943038632715a89a421b0281eca037d6e60016bd70371fd45a2fadd63f8824b34331f2cb5f7fe69f04df7f6d9caea04e05dda8cffa3e453aa474f955eef97fe63e9c9721860aaea379a0ace111fd16"]
```

##### Returns

`DATA`, 32 Bytes - the transaction hash, or the zero hash if the transaction is not yet available.

Use [abey_getTransactionReceipt](#abey_gettransactionreceipt) to get the contract address, after the transaction was mined, when you created a contract.

##### Example
```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"abey_sendAbeyRawTransaction","params":[{see above}],"id":1}'

// Result
{
  "id":1,
  "jsonrpc": "2.0",
  "result": "0xc7509ef7672e1c1d59cec2854d3d074d442984382bd03c665c2e82ebfdacc25e"
}
```

***

#### abey_call

Executes a new message call immediately without creating a transaction on the block chain.


##### Parameters

1. `Object` - The transaction call object
  - `from`: `DATA`, 20 Bytes - (optional) The address the transaction is sent from.
  - `to`: `DATA`, 20 Bytes  - The address the transaction is directed to.
  - `gas`: `QUANTITY`  - (optional) Integer of the gas provided for the transaction execution. abey_call consumes zero gas, but this parameter may be needed by some executions.
  - `gasPrice`: `QUANTITY`  - (optional) Integer of the gasPrice used for each paid gas
  - `value`: `QUANTITY`  - (optional) Integer of the value sent with this transaction
  - `data`: `DATA`  - (optional) Hash of the method signature and encoded parameters
2. `QUANTITY|TAG` - integer block number, or the string `"latest"`, `"earliest"` or `"pending"`, see the [default block parameter](#the-default-block-parameter)

##### Returns

`DATA` - the return value of executed contract.

##### Example
```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"abey_call","params":[{see abey_sendTransaction parameter}],"id":1}'

// Result
{
  "id":1,
  "jsonrpc": "2.0",
  "result": "0x"
}
```

***

#### abey_estimateGas

Generates and returns an estimate of how much gas is necessary to allow the transaction to complete. The transaction will not be added to the blockchain. Note that the estimate may be significantly more than the amount of gas actually used by the transaction, for a variety of reasons including EVM mechanics and node performance.

##### Parameters

See [abey_call](#abey_call) parameters, expect that all properties are optional. If no gas limit is specified gabey uses the block gas limit from the pending block as an upper bound. As a result the returned estimate might not be enough to executed the call/transaction when the amount of gas is higher than the pending block gas limit.

##### Returns

`QUANTITY` - the amount of gas used.

##### Example
```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"abey_estimateGas","params":[{see abey_sendTransaction parameter}],"id":1}'

// Result
{
  "id":1,
  "jsonrpc": "2.0",
  "result": "0x5208" // 21000
}
```

***

#### abey_getBlockByHash

Returns information about a block by hash.


##### Parameters

1. `DATA`, 32 Bytes - Hash of a block.
2. `Boolean` - If `true` it returns the full transaction objects, if `false` only the hashes of the transactions.

##### Example Parameters
```js
params: [
   '0xe670ec64341771606e55d6b4ca35a1a6b75ee3d5145a99d05921026d1527331',
   true
]
```

##### Returns

`Object` - A block object, or `null` when no block was found:

  - `SnailHash`: `DATA`, 32 Bytes - hash of the snail block.
  - `SnailNumber`: `QUANTITY`- the snail block number.
  - `committeeRoot`: `DATA`, 32 Bytes - hash of the committtee.
  - `extraData`: `DATA` - the "extra data" field of this block.
  - `gasLimit`: `QUANTITY` - the maximum gas allowed in this block.
  - `gasUsed`: `QUANTITY` - the total used gas by all transactions in this block.
  - `hash`: `DATA`, 32 Bytes - hash of the block. `null` when its pending block.
  - `logsBloom`: `DATA`, 256 Bytes - the bloom filter for the logs of the block. `null` when its pending block.
  - `maker`: `DATA`, 20 Bytes - the address of the beneficiary to whom the mining rewards were given.
  - `number`: `QUANTITY` - the block number. `null` when its pending block.
  - `parentHash`: `DATA`, 32 Bytes - hash of the parent block.
  - `receiptsRoot`: `DATA`, 32 Bytes - the root of the receipts trie of the block.
  - `signs`: `Array`, committee signs.
      - `fastHash`: `DATA`, 32 Bytes - hash of the fast block.
      - `fastHeight`: `QUANTITY`- the fast block number.
      - `result`: `QUANTITY`- the vote.
      - `sign`: `DATA`, 32 Bytes - committee sign hash.
  - `size`: `QUANTITY` - integer the size of this block in bytes.
  - `stateRoot`: `DATA`, 32 Bytes - the root of the final state trie of the block.
  - `switchInfos`: `Array`, committee member switch.
  - `timestamp`: `QUANTITY` - the unix timestamp for when the block was collated.
  - `transactions`: `Array` - Array of transaction objects, or 32 Bytes transaction hashes depending on the last given parameter.
  - `transactionsRoot`: `DATA`, 32 Bytes - the root of the transaction trie of the block.


##### Example
```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"abey_getBlockByHash","params":["0xe670ec64341771606e55d6b4ca35a1a6b75ee3d5145a99d05921026d15273312", true],"id":1}'

// Result
{
"id":1,
"jsonrpc":"2.0",
"result": {
    {"SnailHash":"0x0000000000000000000000000000000000000000000000000000000000000000",
"SnailNumber":"0x0",
"committeeRoot":"0x1dcc4de8dec75d7aab85b567b6ccd41ad312451b948a7413f0a142fd40d49347",
"extraData":"0x",
"gasLimit":"0xb71b00","gasUsed":"0x0",
"hash":"0xd58570f394347e6b73c4beeabfb75f8b4a6c6f08c71f159a233309365836e3d2",
"logsBloom":"0x00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000",
"maker":"0x49fc88c2576b4f015cf75dae80e87a815d832888",
"number":"0xab4",
"parentHash":"0x0832d972f5b16ddefc3de154cc0a5a4ea16be2991be19bd740ae3486a83ff59f",
"receiptsRoot":"0x56e81f171bcc55a6ff8345e692c0f86e5b48e01b996cadc001622fb5e363b421",
"signs":[{"fastHash":"0xd58570f394347e6b73c4beeabfb75f8b4a6c6f08c71f159a233309365836e3d2",
"fastHeight":"0xab4","result":1,
"sign":"0x7a07be32cce585b6d74e6134022c973b6433dcb87447ec456712f5d3b40b8907403ea24c81309aa090e7469fc761372de2e2d32beddf0031eed1aa557185cbc101"},
{"fastHash":"0xd58570f394347e6b73c4beeabfb75f8b4a6c6f08c71f159a233309365836e3d2","fastHeight":"0xab4","result":1,"sign":"0x4f1033692e2f354409002ff0ce9eb20d4edb676f6c02dc58223c9d2d15eebcaf4071c2efc630a44d78865da714ae694cd690d4b0d02b393d64ca377c63594a6e01"},{"fastHash":"0xd58570f394347e6b73c4beeabfb75f8b4a6c6f08c71f159a233309365836e3d2","fastHeight":"0xab4","result":1,"sign":"0xc8303a5c76fb70e834b63e70180af9720ac09eda59327a0e6be3ab85fcfcbb9b40434ae7dcd23df13fe6a55c1967d29de7db5c5e545674b6849a1a4eabb59b4b00"},
{"fastHash":"0xd58570f394347e6b73c4beeabfb75f8b4a6c6f08c71f159a233309365836e3d2","fastHeight":"0xab4","result":1,"sign":"0x041df0a05407cb302695babfeff03d669d300e76cc2d33305512dc0859aeb4dc47b68065b5d62bd88a6b60e3993ed07ae0a64223681084c5c958cdd2041f42a100"},
{"fastHash":"0xd58570f394347e6b73c4beeabfb75f8b4a6c6f08c71f159a233309365836e3d2","fastHeight":"0xab4","result":1,"sign":"0x8b8c97a4155c2b687b0eb90e1a716ede85c1b32ec7b164c0fa721f1b18ada4c41bc46373419d625ef8f0368b123bf86a60b07c168ebb5b97de4b9095847fad5001"}],"size":"0x40a","stateRoot":"0x5c7127948504801c7db0ef17df87950b471a94d6f5332d39ceff41298f3f6b74",
"switchInfos":[],"timestamp":"0x5ce25206","transactions":[],
"transactionsRoot":"0x56e81f171bcc55a6ff8345e692c0f86e5b48e01b996cadc001622fb5e363b421"}
  }
}
```

***

#### abey_getBlockByNumber

Returns information about a block by block number.

##### Parameters

1. `QUANTITY|TAG` - integer of a block number, or the string `"earliest"`, `"latest"` or `"pending"`, as in the [default block parameter](#the-default-block-parameter).
2. `Boolean` - If `true` it returns the full transaction objects, if `false` only the hashes of the transactions.

##### Example Parameters
```js
params: [
   '0x1b4', // 436
   true
]
```

##### Returns

See [abey_getBlockByHash](#abey_getblockbyhash)

##### Example
```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"abey_getBlockByNumber","params":["0x1b4", true],"id":1}'
```

Result see [abey_getBlockByHash](#abey_getblockbyhash)

***

#### abey_getTransactionByHash

Returns the information about a transaction requested by transaction hash.


##### Parameters

1. `DATA`, 32 Bytes - hash of a transaction

##### Example Parameters
```js
params: [
   "0x88df016429689c079f3b2f6ad39fa052532c56795b733da78a91ebe6a713944b"
]
```

##### Returns

`Object` - A transaction object, or `null` when no transaction was found:

  - `blockHash`: `DATA`, 32 Bytes - hash of the block where this transaction was in. `null` when its pending.
  - `blockNumber`: `QUANTITY` - block number where this transaction was in. `null` when its pending.
  - `from`: `DATA`, 20 Bytes - address of the sender.
  - `gas`: `QUANTITY` - gas provided by the sender.
  - `gasPrice`: `QUANTITY` - gas price provided by the sender in Wei.
  - `hash`: `DATA`, 32 Bytes - hash of the transaction.
  - `input`: `DATA` - the data send along with the transaction.
  - `nonce`: `QUANTITY` - the number of transactions made by the sender prior to this one.
  - `to`: `DATA`, 20 Bytes - address of the receiver. `null` when its a contract creation transaction.
  - `transactionIndex`: `QUANTITY` - integer of the transaction's index position in the block. `null` when its pending.
  - `value`: `QUANTITY` - value transferred in Wei.
  - `v`: `QUANTITY` - ECDSA recovery id
  - `r`: `DATA`, 32 Bytes - ECDSA signature r
  - `s`: `DATA`, 32 Bytes - ECDSA signature s

  - `payer`: `DATA`, 20 Bytes - address of the payer.
  - `fee`: `QUANTITY` - transaction fee in Wei.
  - `pv`: `QUANTITY` - ECDSA recovery id
  - `pr`: `DATA`, 32 Bytes - ECDSA signature pr
  - `ps`: `DATA`, 32 Bytes - ECDSA signature ps


##### Example
```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"abey_getTransactionByHash","params":["0x88df016429689c079f3b2f6ad39fa052532c56795b733da78a91ebe6a713944b"],"id":1}'

// Result
{
  "jsonrpc":"2.0",
  "id":1,
  "result":{
    "blockHash":"0x1d59ff54b1eb26b013ce3cb5fc9dab3705b415a67127a003c3e61eb445bb8df2",
    "blockNumber":"0x5daf3b", // 6139707
    "from":"0xa7d9ddbe1f17865597fbd27ec712455208b6b76d",
    "gas":"0xc350", // 50000
    "gasPrice":"0x4a817c800", // 20000000000
    "hash":"0x88df016429689c079f3b2f6ad39fa052532c56795b733da78a91ebe6a713944b",
    "input":"0x68656c6c6f21",
    "nonce":"0x15", // 21
    "to":"0xf02c1c8e6114b1dbe8937a39260b5b0a374432bb",
    "transactionIndex":"0x41", // 65
    "value":"0xf3dbb76162000", // 4290000000000000
    "v":"0x25", // 37
    "r":"0x1b5e176d927f8e9ab405058b2d2457392da3e20f328b16ddabcebc33eaac5fea",
    "s":"0x4ba69724e8f69de52f0125ad8b3c5c2cef33019bac3249e2c0a2192766d1721c"
    "payer":null,
    "fee":null,
    "pv":null,
    "pr":null,
    "ps":null
  }
}
```

***

#### abey_getTransactionByBlockHashAndIndex

Returns information about a transaction by block hash and transaction index position.


##### Parameters

1. `DATA`, 32 Bytes - hash of a block.
2. `QUANTITY` - integer of the transaction index position.

##### Example Parameters
```js
params: [
   '0xe670ec64341771606e55d6b4ca35a1a6b75ee3d5145a99d05921026d1527331',
   '0x0' // 0
]
```

##### Returns

See [abey_getTransactionByHash](#abey_gettransactionbyhash)

##### Example
```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"abey_getTransactionByBlockHashAndIndex","params":["0xc6ef2fc5426d6ad6fd9e2a26abeab0aa2411b7ab17f30a99d3cb96aed1d1055b", "0x0"],"id":1}'
```

Result see [abey_getTransactionByHash](#abey_gettransactionbyhash)

***

#### abey_getTransactionByBlockNumberAndIndex

Returns information about a transaction by block number and transaction index position.


##### Parameters

1. `QUANTITY|TAG` - a block number, or the string `"earliest"`, `"latest"` or `"pending"`, as in the [default block parameter](#the-default-block-parameter).
2. `QUANTITY` - the transaction index position.

##### Example Parameters
```js
params: [
   '0x29c', // 668
   '0x0' // 0
]
```

##### Returns

See [abey_getTransactionByHash](#abey_gettransactionbyhash)

##### Example
```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"abey_getTransactionByBlockNumberAndIndex","params":["0x29c", "0x0"],"id":1}'
```

Result see [abey_getTransactionByHash](#abey_gettransactionbyhash)

***

#### abey_getTransactionReceipt

Returns the receipt of a transaction by transaction hash.

**Note** That the receipt is not available for pending transactions.


##### Parameters

1. `DATA`, 32 Bytes - hash of a transaction

##### Example Parameters
```js
params: [
   '0xb903239f8543d04b5dc1ba6579132b143087c68db1b2168786408fcbce568238'
]
```

##### Returns

`Object` - A transaction receipt object, or `null` when no receipt was found:

  - `transactionHash `: `DATA`, 32 Bytes - hash of the transaction.
  - `transactionIndex`: `QUANTITY` - integer of the transaction's index position in the block.
  - `blockHash`: `DATA`, 32 Bytes - hash of the block where this transaction was in.
  - `blockNumber`: `QUANTITY` - block number where this transaction was in.
  - `from`: `DATA`, 20 Bytes - address of the sender.
  - `to`: `DATA`, 20 Bytes - address of the receiver. null when it's a contract creation transaction.
  - `cumulativeGasUsed `: `QUANTITY ` - The total amount of gas used when this transaction was executed in the block.
  - `gasUsed `: `QUANTITY ` - The amount of gas used by this specific transaction alone.
  - `contractAddress `: `DATA`, 20 Bytes - The contract address created, if the transaction was a contract creation, otherwise `null`.
  - `to`: `DATA`, 20 Bytes  - The address the transaction is directed to.
  - `logs`: `Array` - Array of log objects, which this transaction generated.
  - `logsBloom`: `DATA`, 256 Bytes - Bloom filter for light clients to quickly retrieve related logs.
  
It also returns _either_ :

  - `root` : `DATA` 32 bytes of post-transaction stateroot (pre Byzantium)
  - `status`: `QUANTITY` either `1` (success) or `0` (failure) 


##### Example
```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"abey_getTransactionReceipt","params":["0xb903239f8543d04b5dc1ba6579132b143087c68db1b2168786408fcbce568238"],"id":1}'

// Result
{
"id":1,
"jsonrpc":"2.0",
"result": {
     transactionHash: '0xb903239f8543d04b5dc1ba6579132b143087c68db1b2168786408fcbce568238',
     transactionIndex:  '0x1', // 1
     blockNumber: '0xb', // 11
     blockHash: '0xc6ef2fc5426d6ad6fd9e2a26abeab0aa2411b7ab17f30a99d3cb96aed1d1055b',
     cumulativeGasUsed: '0x33bc', // 13244
     gasUsed: '0x4dc', // 1244
     contractAddress: '0xb60e8dd61c5d32be8058bb8eb970870f07233155', // or null, if none was created
     logs: [{
         // logs as returned by getFilterLogs, etc.
     }, ...],
     logsBloom: "0x00...0", // 256 byte bloom filter
     status: '0x1'
  }
}
```

***

#### abey_newFilter

Creates a filter object, based on filter options, to notify when the state changes (logs).
To check if the state has changed, call [abey_getFilterChanges](#abey_getfilterchanges).

##### A note on specifying topic filters:
Topics are order-dependent. A transaction with a log with topics [A, B] will be matched by the following topic filters:
* `[]` "anything"
* `[A]` "A in first position (and anything after)"
* `[null, B]` "anything in first position AND B in second position (and anything after)"
* `[A, B]` "A in first position AND B in second position (and anything after)"
* `[[A, B], [A, B]]` "(A OR B) in first position AND (A OR B) in second position (and anything after)"

##### Parameters

1. `Object` - The filter options:
  - `fromBlock`: `QUANTITY|TAG` - (optional, default: `"latest"`) Integer block number, or `"latest"` for the last mined block or `"pending"`, `"earliest"` for not yet mined transactions.
  - `toBlock`: `QUANTITY|TAG` - (optional, default: `"latest"`) Integer block number, or `"latest"` for the last mined block or `"pending"`, `"earliest"` for not yet mined transactions.
  - `address`: `DATA|Array`, 20 Bytes - (optional) Contract address or a list of addresses from which logs should originate.
  - `topics`: `Array of DATA`,  - (optional) Array of 32 Bytes `DATA` topics. Topics are order-dependent. Each topic can also be an array of DATA with "or" options.

##### Example Parameters
```js
params: [{
  "fromBlock": "0x1",
  "toBlock": "0x2",
  "address": "0x8888f1f195afa192cfee860698584c030f4c9db1",
  "topics": ["0x000000000000000000000000a94f5374fce5edbc8e2a8697c15331677e6ebf0b", null, ["0x000000000000000000000000a94f5374fce5edbc8e2a8697c15331677e6ebf0b", "0x0000000000000000000000000aff3454fce5edbc8cca8697c15331677e6ebccc"]]
}]
```

##### Returns

`QUANTITY` - A filter id.

##### Example
```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"abey_newFilter","params":[{"topics":["0x0000000000000000000000000000000000000000000000000000000012341234"]}],"id":73}'

// Result
{
  "id":1,
  "jsonrpc": "2.0",
  "result": "0x1" // 1
}
```

***

#### abey_newBlockFilter

Creates a filter in the node, to notify when a new block arrives.
To check if the state has changed, call [abey_getFilterChanges](#abey_getfilterchanges).

##### Parameters
None

##### Returns

`QUANTITY` - A filter id.

##### Example
```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"abey_newBlockFilter","params":[],"id":73}'

// Result
{
  "id":1,
  "jsonrpc":  "2.0",
  "result": "0x1" // 1
}
```

***

#### abey_newPendingTransactionFilter

Creates a filter in the node, to notify when new pending transactions arrive.
To check if the state has changed, call [abey_getFilterChanges](#abey_getfilterchanges).

##### Parameters
None

##### Returns

`QUANTITY` - A filter id.

##### Example
```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"abey_newPendingTransactionFilter","params":[],"id":73}'

// Result
{
  "id":1,
  "jsonrpc":  "2.0",
  "result": "0x1" // 1
}
```

***

#### abey_uninstallFilter

Uninstalls a filter with given id. Should always be called when watch is no longer needed.
Additonally Filters timeout when they aren't requested with [abey_getFilterChanges](#abey_getfilterchanges) for a period of time.


##### Parameters

1. `QUANTITY` - The filter id.

##### Example Parameters
```js
params: [
  "0xb" // 11
]
```

##### Returns

`Boolean` - `true` if the filter was successfully uninstalled, otherwise `false`.

##### Example
```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"abey_uninstallFilter","params":["0xb"],"id":73}'

// Result
{
  "id":1,
  "jsonrpc": "2.0",
  "result": true
}
```

***

#### abey_getFilterChanges

Polling method for a filter, which returns an array of logs which occurred since last poll.


##### Parameters

1. `QUANTITY` - the filter id.

##### Example Parameters
```js
params: [
  "0x16" // 22
]
```

##### Returns

`Array` - Array of log objects, or an empty array if nothing has changed since last poll.

- For filters created with `abey_newBlockFilter` the return are block hashes (`DATA`, 32 Bytes), e.g. `["0x3454645634534..."]`.
- For filters created with `abey_newPendingTransactionFilter ` the return are transaction hashes (`DATA`, 32 Bytes), e.g. `["0x6345343454645..."]`.
- For filters created with `abey_newFilter` logs are objects with following params:

  - `removed`: `TAG` - `true` when the log was removed, due to a chain reorganization. `false` if its a valid log.
  - `logIndex`: `QUANTITY` - integer of the log index position in the block. `null` when its pending log.
  - `transactionIndex`: `QUANTITY` - integer of the transactions index position log was created from. `null` when its pending log.
  - `transactionHash`: `DATA`, 32 Bytes - hash of the transactions this log was created from. `null` when its pending log.
  - `blockHash`: `DATA`, 32 Bytes - hash of the block where this log was in. `null` when its pending. `null` when its pending log.
  - `blockNumber`: `QUANTITY` - the block number where this log was in. `null` when its pending. `null` when its pending log.
  - `address`: `DATA`, 20 Bytes - address from which this log originated.
  - `data`: `DATA` - contains the non-indexed arguments of the log.
  - `topics`: `Array of DATA` - Array of 0 to 4 32 Bytes `DATA` of indexed log arguments. (In *solidity*: The first topic is the *hash* of the signature of the event (e.g. `Deposit(address,bytes32,uint256)`), except you declared the event with the `anonymous` specifier.)

##### Example
```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"abey_getFilterChanges","params":["0x16"],"id":73}'

// Result
{
  "id":1,
  "jsonrpc":"2.0",
  "result": [{
    "logIndex": "0x1", // 1
    "blockNumber":"0x1b4", // 436
    "blockHash": "0x8216c5785ac562ff41e2dcfdf5785ac562ff41e2dcfdf829c5a142f1fccd7d",
    "transactionHash":  "0xdf829c5a142f1fccd7d8216c5785ac562ff41e2dcfdf5785ac562ff41e2dcf",
    "transactionIndex": "0x0", // 0
    "address": "0x16c5785ac562ff41e2dcfdf829c5a142f1fccd7d",
    "data":"0x0000000000000000000000000000000000000000000000000000000000000000",
    "topics": ["0x59ebeb90bc63057b6515673c3ecf9438e5058bca0f92585014eced636878c9a5"]
    },{
      ...
    }]
}
```

***

#### abey_getFilterLogs

Returns an array of all logs matching filter with given id.


##### Parameters

1. `QUANTITY` - The filter id.

##### Example Parameters
```js
params: [
  "0x16" // 22
]
```

##### Returns

See [abey_getFilterChanges](#abey_getfilterchanges)

##### Example
```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"abey_getFilterLogs","params":["0x16"],"id":74}'
```

Result see [abey_getFilterChanges](#abey_getfilterchanges)

***

#### abey_getLogs

Returns an array of all logs matching a given filter object.

##### Parameters

1. `Object` - The filter options:
  - `fromBlock`: `QUANTITY|TAG` - (optional, default: `"latest"`) Integer block number, or `"latest"` for the last mined block or `"pending"`, `"earliest"` for not yet mined transactions.
  - `toBlock`: `QUANTITY|TAG` - (optional, default: `"latest"`) Integer block number, or `"latest"` for the last mined block or `"pending"`, `"earliest"` for not yet mined transactions.
  - `address`: `DATA|Array`, 20 Bytes - (optional) Contract address or a list of addresses from which logs should originate.
  - `topics`: `Array of DATA`,  - (optional) Array of 32 Bytes `DATA` topics. Topics are order-dependent. Each topic can also be an array of DATA with "or" options.
  - `blockhash`:  `DATA`, 32 Bytes - (optional) With the addition of EIP-234 (gabey >= v1.8.13 or Parity >= v2.1.0), `blockHash` is a new filter option which restricts the logs returned to the single block with the 32-byte hash `blockHash`.  Using `blockHash` is equivalent to `fromBlock` = `toBlock` = the block number with hash `blockHash`.  If `blockHash` is present in the filter criteria, then neither `fromBlock` nor `toBlock` are allowed.

##### Example Parameters
```js
params: [{
  "topics": ["0x000000000000000000000000a94f5374fce5edbc8e2a8697c15331677e6ebf0b"]
}]
```

##### Returns

See [abey_getFilterChanges](#abey_getfilterchanges)

##### Example
```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"abey_getLogs","params":[{"topics":["0x000000000000000000000000a94f5374fce5edbc8e2a8697c15331677e6ebf0b"]}],"id":74}'
```

Result see [abey_getFilterChanges](#abey_getfilterchanges)

***

#### abey_getWork

Returns the hash of the current block, the seedHash, and the boundary condition to be met ("target").

##### Parameters
none

##### Returns

`Array` - Array with the following properties:
  1. `DATA`, 32 Bytes - current snailBlock header without nonce
  2. `DATA`, 32 Bytes - the seed hash used for the DAG.
  3. `DATA`, 32 Bytes - the boundary condition ("target"), 2^256 / fruit difficulty.
  4. `DATA`, 32 Bytes - the boundary condition ("target"), 2^256 / snailBlock difficulty.

##### Example
```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"abey_getWork","params":[],"id":73}'

// Result
{
	"jsonrpc": "2.0",
	"id": 73,
	"result": ["0xbcdcb8533dcf88b736f5b44f777250922f0f0472d4dd34a5a1b445a0c47aceed", "58bc067579760d307143ec1cd416eb3814110d29bf21aba0cd18586e2f038791", "0x0000000000000000000000000000000000000000000000000000000000000064", "0x0000000000000000000000000000000000000000000000000000000000002710"]
}
```

***

#### abey_submitWork

Used for submitting a proof-of-work solution.


##### Parameters

1. `DATA`, 8 Bytes - The nonce found (64 bits)
2. `DATA`, 32 Bytes - The header's pow-hash (256 bits)
3. `DATA`, 32 Bytes - The mix digest (256 bits)

##### Example Parameters
```js
params: [
  "0x0000000000000001",
  "0x1234567890abcdef1234567890abcdef1234567890abcdef1234567890abcdef",
  "0xD1FE5700000000000000000000000000D1FE5700000000000000000000000000"
]
```

##### Returns

`Boolean` - returns `true` if the provided solution is valid, otherwise `false`.


##### Example
```js
// Request
curl -X POST --data '{"jsonrpc":"2.0", "method":"abey_submitWork", "params":["0x0000000000000001", "0x1234567890abcdef1234567890abcdef1234567890abcdef1234567890abcdef", "0x1234567890abcdef1234567890abcdef1234567890abcdef1234567890abcdef"],"id":73}'

// Result
{
  "id":73,
  "jsonrpc":"2.0",
  "result": true
}
```

***

#### abey_submitHashrate

Used for submitting mining hashrate.


##### Parameters

1. `Hashrate`, a hexadecimal string representation (32 bytes) of the hash rate 
2. `ID`, String - A random hexadecimal(32 bytes) ID identifying the client

##### Example Parameters
```js
params: [
  "0x500000",
  "0x59daa26581d0acd1fce254fb7e85952f4c09d0915afd33d3886cd914bc7d283c"
]
```

##### Returns

`Boolean` - returns `true` if submitting went through succesfully and `false` otherwise.


##### Example
```js
// Request
curl -X POST --data '{"jsonrpc":"2.0", "method":"abey_submitHashrate", "params":["0x500000", "0x59daa26581d0acd1fce254fb7e85952f4c09d0915afd33d3886cd914bc7d283c"],"id":73}'

// Result
{
  "id":73,
  "jsonrpc":"2.0",
  "result": true
}
```

***

#### abey_committeeNumber

get current committee number


##### Parameters
none


##### Returns

`QUANTITY` - integer of the current committee number.

##### Example
```js
// Request
curl -X POST --data '{"jsonrpc":"2.0", "method":"abey_committeeNumber", "params":[],"id":100}'

// Result
{
   "jsonrpc": "2.0",
   "id": 100,
   "result": 8
}
```

***

#### abey_fruitNumber

get current fruit number


##### Parameters
none


##### Returns

`QUANTITY` - integer of the current fruit number.

##### Example
```js
// Request
curl -X POST --data '{"jsonrpc":"2.0", "method":"abey_fruitNumber", "params":[],"id":100}'

// Result
{
	"jsonrpc": "2.0",
	"id": 100,
	"result": "0x1975f"
}
```

***

#### abey_rewardSnailBlock

get current reward snalBlock  infomation


##### Parameters
none


##### Returns

`Object` - A snail block object, or `null` when no snail reward block was found:

  - `beginFruitNumber`: `QUANTITY` - the beign fruit number in snail reward block.
  - `difficulty`: `QUANTITY` - integer of the difficulty for this snail reward block.
  - `endFruitNumber`: `QUANTITY` - the end fruit number in snail reward block.
  - `extraData`: `DATA` - the "extra data" field of this reward block.
  - `fastNumber`: `QUANTITY` - default 0.
  - `fruitDifficulty`: `QUANTITY` - integer of all fruit difficulty for this snail reward block.
  - `fruitsHash`: `DATA`, 32 Bytes - hash of fruits in snail reward block.
  - `fruits`: `Array` -  Array of all hash of fruits in snail reward block.
  - `hash`: `DATA`, 32 Bytes - hash of the snail reward block. 
  - `miner`: `DATA`, 20 Bytes - the address of the beneficiary to whom the mining rewards were given.
  - `mixHash`: `DATA`, 32 Bytes - the mix digest.
  - `nonce`: `QUANTITY` - the mining nonce.
  - `number`: `QUANTITY` - the snail reward number.
  - `parentHash`: `DATA`, 32 Bytes - hash of the parent block.
  - `pointerNumber`: `QUANTITY` - current snail reward block minus 7.
  - `size`: `QUANTITY` - integer the size of this reward block in bytes.
  - `timestamp`: `QUANTITY` - the unix timestamp for when the block was collated.

##### Example
```js
// Request
curl -X POST --data '{"jsonrpc":"2.0", "method":"abey_rewardSnailBlock", "params":[],"id":100}'

// Result
{
	"jsonrpc": "2.0",
	"id": 100,
	"result": {
		"beginFruitNumber": "0x193a0",
		"difficulty": "0x2710",
		"endFruitNumber": "0x193db",
		"extraData": "0xd9820a018667657472756588676f312e31302e38856c696e7578",
		"fastNumber": 0,
		"fruitDifficulty": "0x64",
		"fruitsHash": "0x7dad8df99080412919a4e0018c980e6bf40e64bff5e09f0ec45efd63530ed95e",
		"fruits": ["0x421cf9aa22145c141d3235a5b19c73d10613cc782221f279c3a136f136ac811e", "0xb6fdad4a0287102adba3b5bbe9789598694c028491de7c459f47248ee1844132", "0xa784a38af019ea321c725bf6b6ed61398bb86d4b20289c0a6ef54a20eced554c", "0x8fe6337b88ee4f19514670d49f5a64e736edfd7ae8c34fecc6d54ce923325991", "0x9dcbc81a1e764f6c0a6591e72000776dad6752be2d9c65b77a2157bc88a65333", "0x5cbac30e3a8d3246bbb84b570aecfa69b1d364738e4223fa17fe047b307ed7f2", "0x75115e8cdcfba043c57c2a10c6e781ab582f85b2e715cbacc42e3d2f96bc7cac", "0x08fe101b8ef2304965b5008267ee321ddbc48b1fd210b4dff81441e9c57f2e0f", "0xe31577b98c384074b37ad1d9cad88d31483b152ed96106e17f9864ded8052f62", "0xce3f340eb5c0582e82f5f9c550373ba069af140b50eabaf7a044717eada32536", "0x879f73afc3cb734dd9b5b7fe7687b6f8d6a5b8e7d20c4f730dbd0f84590ac0de", "0xd0c3c2c31f6b19c618c709968624ca02b23341b01178f9de23b43a406755c93e", "0xaa1285332874bd67c7b37b7b81f66e0b3fe6b38b176d22a0ee0349ef3120f9c4", "0xaed6ff91f2f1de5fcd7880797b841c61458bd59fe3f4bd4e80823d99a555049f", "0x46e8303c548f82e59741ee94ced70a0df1500abef9d667f131a39e71b9aaff0c", "0xedc6d3a94624bd914435119493c5d73545c854385758e86cb1b2c6445de7a6d4", "0x46a42a82461c5b92ef7ecf86c6d84dd8a7abeddd72a444467c109da53de26f10", "0xf23a03a1698426280449512bf19f731c56051cd19d04976009d7d2e03d6294a4", "0x4f2b30f026d64d56ec8d0fc69ef0959bbceeff648eafe8ff3c711af4d0f796eb", "0xbcfd45ccd7456fdb5dc3d19a2bd65f4b37530e4772fb6a6364376df3dee8b883", "0x3c1ce1153fc784870812be53f89c549063c5056d653db16621232582b6fe51f7", "0x8dfbb789f49ff07237c5650fa166d7adf9a2ff5b0ee967e510686a0df24b5e6e", "0x06aef81bc2b5d914fc553dad79a83dc0660c33c75448e62129b1da8443a3e704", "0xf291e642625bf18ddc03eb2c6287dea900457bcf8102834c26fb08438eb5d434", "0x73bb8bc367a789ce6e69ef4e21ffec76e103d36a85a01a460bd268a1ceec992a", "0x364c55068a8341db5e3ecc6fa6cc328e1b038d9a567b85fb3e470d8ee12a2fd8", "0x460e5ddc879c9da6090296d04fc522fcfc4341566856c0e4b81a6ee459922c06", "0x0f17e693874809172d889bd813ed4e825fdc4436d4c97bc48725b924ab612ce5", "0xca07ab5afbdc766247276e3641df8206cc89ca00660f3d72ceac86b1e192652a", "0x96027060b71fb9b758131b70c481c2e5f916bb821fa7295a834080c89698dbe1", "0xb9ed69d03aa2a80e358950b82fbd083fbef5069b6b8f2488cd74d70260b5de9b", "0x71c0249266f922677eab708b5de86fe1f057b6b39ad6dcd05230de89a0021eba", "0xbcb6f37b63558db10bedab0f722e34341a6329f57248e00606fe184613468c5c", "0x08eb945b49599aa7903bac856f5f5bb9d260bc1c2c553b2ab9d69a8611c4c935", "0xcac5bb4daec7f138cb820e5fd4b958184c9e724d4879662f4422df0230aadb14", "0xd29c761ec3504ae091998c2acbc3e739635d99e0764a7f521021a8ae7c068c36", "0xe5de172346d9d1f423a09726ca6e992725986c54bf2818ba537d341ba63ebf19", "0x337f7834016004bfa59869cfc357035dbc8128b340ed062e7a30a93bee2ac3fb", "0xf0f6790614751991e9d04548c7a81b0ef28aff686a704e4cfbfe1de01e6fce9e", "0x502e04348f225907f8528d9196c7113bd3c15d3080c1c1718c85c8cc073ea7bb", "0x663bf65f7e766082cfb84ab2d9303f9c078e2819eb2197d6f6318dafc55cfa98", "0x282b563050f940059986b4745345f14ce6d7e96b7160ead7a27985ce99aeae9b", "0x1459d9bb5939fe563d6d9502e28184d5c4619c10d0e0f6699b746daf4c1b0400", "0xc7d3fff73e954042d5bd91107252ff3986c9c756869c0484954d82831dfbfb28", "0x0cb9e8929bf625b0916879359fc617ce0b4e7f474df5cf53414d2075e8668cca", "0x9b8715bf3090fdf115ae675bf4692e5dc3e8453b126fd729ddfabbfaed11aa66", "0x69668b51d76ce1ab186158f05ef8d92394803b9f0c3e508d18d39fe45f2e25d4", "0x972f4e2f865c5e00f377418a2a0469e3abd3c1b64b2cdd803904dd33a9bfc7a0", "0x3ccb10a6a69fcfe8da544bddfcadf6be6a295dd4b38ab5ba0f9e8aa36f4188fd", "0x4098d83b4e8f506c7b85560a8e84017f845b1083b864e3975208ece07f98074f", "0xec6edff7cd40f0bc41b1ed6a129e88bec38cd6474f130363cd6dccc31d49a717", "0xa89aa9e24a3f0926bbbfc4c8a80ed872f58963f83d7d99b13f169463783f4e52", "0x07bb236e22e0d322d962bb28f329aed7ce8e9c460215d8700826a5faa777ccac", "0xf87aa14c01ee28584632c6db9b1667d27c8d0c0eb947d286318641e549dd3c1c", "0x2aeb9798bade7ece9a6fc803e9fb7359c14cb6c27dd986a27e8b20639dda5377", "0x07858381ad1ab29ac5a0f34fd78420cffa494c0dbb9bca64c1bff28075ba490b", "0x121d18d6770575a34480a8ddc268e87438e424d721d1b76c84cb60297d0a7156", "0xc54357b750662ef429845acfeacf482cffa4b253373e05199da3e01301823572", "0xe1bcd60bafc03acbcc8c8c6b454af4ecc1171cbc7d9ab4ed1e321bab82308af8", "0x7917615f4519f7160de4ac556c8de805ce8bc464e5784642cc937e000512ad0f"],
		"hash": "0xb913521609feef2b7cc351efe0d0b359796c709176e8824622bc6dee312a227f",
		"miner": "0x7c357530174275dd30e46319b89f71186256e4f7",
		"mixHash": "0x0000b9b5b60cbaa58a008425f70c3e610794e9eb9ffa7e8c9ab257c451afcc22",
		"nonce": "0x279486ec07bf61d6",
		"number": "0x6b4",
		"parentHash": "0xa24c81127edb0a630b13205b498d8ef0f7d47a4678e96827f8f19ffd2c470a24",
		"pointerNumber": 1708,
		"size": "0x101bb",
		"timestamp": "0x5c9194ec"
	}
}
```

***

#### abey_snailBlockNumber

get current snail block number


##### Parameters
none


##### Returns

`QUANTITY` - integer of the current snail block number.

##### Example
```js
// Request
curl -X POST --data '{"jsonrpc":"2.0", "method":"abey_snailBlockNumber", "params":[],"id":100}'

// Result
{
  "id":100,
  "jsonrpc":"2.0",
  "result": "0x38"
}
```

***

#### abey_getCommittee

get committee member infomation


##### Parameters
  ```js
params: ["0x1"]
```


##### Returns

- `backups`: `Array` - Array of backup committee members, each committee member info contains pubkey、 coinbase、flag、type.
- `beginNumber`: `QUANTITY` - the begin fast block number.
- `beginSnailNumber`: `QUANTITY` - the begin snail block number.
- `endNumber`: `QUANTITY` - the end fast block number.
- `endSnailNumber`: `QUANTITY` - the end snail block number.
- `id`: `QUANTITY` - committeeId.
- `memberCount`: `QUANTITY` - the number of committee members .
- `members`: `Array` - Array of  committee members.


##### Example
```js
// Request
curl -X POST --data '{"jsonrpc":"2.0", "method":"abey_getCommittee", "params":["0x1"],"id":100}'

// Result
{
	"jsonrpc": "2.0",
	"id": 100,
	"result": {
		"backups": [{
			"PKey": "0488a25849abee5921fdb581ba34cd66adc8e02b108391c4153ca8da27722e16badf4fcd5ba7f557ae76d444ccf3638e4590a181805623de1cab67f31364c79736",
			"coinbase": "0x76ea2f3a002431fede1141b660dbb75c26ba6d97",
			"flag": 160,
			"type": 161
		}, {
			"PKey": "04a9a1cedb8900d893b607c4dbc834abada3fe98f247b8bcb5ef44d3d3a246c4cf41d9d792527473c30ded81fa4b81afe7030a09e093dd92746b98c79e6a204c63",
			"coinbase": "0x831151b7eb8e650dc442cd623fbc6ae20279df85",
			"flag": 160,
			"type": 161
		}, {
			"PKey": "040d153624462927444a8212717e4ad41ec5f5739bc36598d093d114729e1dc782d55d322699705829cf9d69f201009db797ebe8ba952f10a26fe36c64356b111b",
			"coinbase": "0x1074f7deccf8c66efcd0106e034d3356b7db3f2c",
			"flag": 160,
			"type": 161
		}, {
			"PKey": "04a3474c26578fce00d241119758271f6a208cc987c6f37d1518dcea2a51257bafeebd93202ae499cb5a8986720d4b63a04043aadb4d03430194a81860c9ca0763",
			"coinbase": "0xd985e9871d1be109af5a7f6407b1d6b686901fff",
			"flag": 160,
			"type": 161
		}, {
			"PKey": "04a3e174523b1054e14f123580bce258745e65591c2a4ee44764e55eb87a3782c9920d306e6121d4f10f8726800497ad9ca5a0bfdfe0832779dbaf7b95b3bf0111",
			"coinbase": "0x7c357530174275dd30e46319b89f71186256e4f7",
			"flag": 160,
			"type": 161
		}, {
			"PKey": "04d370defb1b7b8c086f98c4a7d7b90348b088cd2effdcc27b86feebdff499a192b4a5a5b16a400625271d69b3fa7d8c42c8b2e15c910cd1f314f28eb5beb73342",
			"coinbase": "0x4cf807958b9f6d9fd9331397d7a89a079ef43288",
			"flag": 160,
			"type": 161
		}, {
			"PKey": "04f67ab0cd48f626da89c718bcd909a04dea393d632d3191891539ef2f5ff6bb1e5d340ebe94cb6d9126b26e1ec64bb4783e9e8ddf31346b53d651d15eb226142e",
			"coinbase": "0x04d2252a3e0ca7c2aa81247ca33060855a34a808",
			"flag": 160,
			"type": 161
		}],
		"beginNumber": 19781,
		"beginSnailNumber": 1,
		"endNumber": 30725,
		"endSnailNumber": 168,
		"id": 1,
		"memberCount": 21,
		"members": [{
			"PKey": "04ad2da6433f25f5063c98fb414496bd794f3054544408a63da4c6488a35a7c7ba38e8dbae49356182dcd81f45a715feb1f7f696c3a1bd77e33ab8ab41d8177ca3",
			"coinbase": "0x7c357530174275dd30e46319b89f71186256e4f7",
			"flag": 161,
			"type": 162
		}, {
			"PKey": "04eb9577ae8e19d99f708b8d46de17220afaf2d4a89676d98c8e8558e4e10f0283733a6a4a47aee779081cedd9266d5b4edcfc4fa22a4d69e3da98f6a03ad4e372",
			"coinbase": "0x7c357530174275dd30e46319b89f71186256e4f7",
			"flag": 161,
			"type": 162
		}, {
			"PKey": "0497413104d718ef592fa4526e796be092a143272f9c7b415c23c73201a9cf3da2ec928e11958246ee2f8e838ec67506e798c4e8fcec89f5e8c9696235f600e765",
			"coinbase": "0x7c357530174275dd30e46319b89f71186256e4f7",
			"flag": 161,
			"type": 162
		}, {
			"PKey": "04dc2a2cba3793da755d28f02336193a8e04c77cb3fdef5f04a4197c71f59f978767729ff652267e4c0fc9204c7e921931c207e32ba442ac27a20acaad49324696",
			"coinbase": "0x7c357530174275dd30e46319b89f71186256e4f7",
			"flag": 161,
			"type": 162
		}, {
			"PKey": "047a523df31438d3efc02c4126b891d5e1258371336decfe43a15efcb519607aa2b932f895808f4414f75b525d7a490e10be314a6b60971c7149156b9d659fb591",
			"coinbase": "0x7c357530174275dd30e46319b89f71186256e4f7",
			"flag": 161,
			"type": 162
		}, {
			"PKey": "04ea62525863113273df44d8b5bbbcf0c1df5d03b94dd60308b747c1894af70dd5eaf5facfb1577bb4416ba517375ce662769f69b0c5fa961ac0cd9a4a342e4f67",
			"coinbase": "0x7c357530174275dd30e46319b89f71186256e4f7",
			"flag": 161,
			"type": 162
		}, {
			"PKey": "04601fc9ac609d9d47d01f76bcdf496a1b3d2aaf9dc4c97319faff49e2284ad843aa5505343376db1357b9cc0d176fe828d7a07cc8cd0993aee3b76d77eda2be4b",
			"coinbase": "0x4cf807958b9f6d9fd9331397d7a89a079ef43288",
			"flag": 161,
			"type": 162
		}, {
			"PKey": "04b64ba599ff0e0c2418357f87ed6058f04801ce0e0f653e336aa2f0b9c9620b1ed9222648100d7cadfbbe2cadb66b94e66eeed80b0a96da7808bc55fd8dd282a2",
			"coinbase": "0x7c357530174275dd30e46319b89f71186256e4f7",
			"flag": 161,
			"type": 162
		}, {
			"PKey": "044aa7cef6d282ec22e0ef6d55d36b17d607afee920668320430717552cd7d4905e07d92a0e939f96ef6d617174a136267ed6a4efcc14879abe6aa097965fb4740",
			"coinbase": "0x7c357530174275dd30e46319b89f71186256e4f7",
			"flag": 161,
			"type": 162
		}, {
			"PKey": "04506b3c55ac57f78b41b2e47afc8d0f8d9891803822bbc4fe35bf3be72ad91056b0aae8a5fa76309f093987202a284c67d5918ca23a31e2e6535255c58240bf59",
			"coinbase": "0x7c357530174275dd30e46319b89f71186256e4f7",
			"flag": 161,
			"type": 162
		}, {
			"PKey": "04a69c470e1b4ea2a05d8a70de7c15687131bbb66105049a5bbdb04645381a29899ec72af7929c645ec0d29a91b4359de2629618b763a333d52bb4f436cccf9a9d",
			"coinbase": "0x7c357530174275dd30e46319b89f71186256e4f7",
			"flag": 161,
			"type": 162
		}, {
			"PKey": "046e61555ed9bd6bb08b3f5034373bfc4472a8ca8868d21e1ceb4fe3f24c42c3a5dc7cb492ca62a31bfa6c82f86fed02dfa23196c25a76673ddb7a1f3f6eaafc84",
			"coinbase": "0x7c357530174275dd30e46319b89f71186256e4f7",
			"flag": 161,
			"type": 162
		}, {
			"PKey": "04eb4c070000b1c4525e42d2cf61a0f1449ef82ce534a19ff03cf8c2a480d16e9738f5162972e15262992eb21cf561b7bae2e49e9dc8eb2b8f52dd9100f2bdce4e",
			"coinbase": "0x7c357530174275dd30e46319b89f71186256e4f7",
			"flag": 161,
			"type": 162
		}, {
			"PKey": "047fc0ae115cadf26b472ed4bed8785ca535c9fe53eb896bd21d7d7d7e1ee242881c50fa5eeaba4465e1992d5b917ae1f891c1e85e0bb5e0201e885886e846102b",
			"coinbase": "0x7c357530174275dd30e46319b89f71186256e4f7",
			"flag": 161,
			"type": 162
		}]
	}
}
```

***

#### abey_getFruitByNumber

get fruit block by fruit number


##### Parameters

1. `QUANTITY` - integer of the fruit number.
2. `Boolean` -  if contais fruit signs of committee member.


##### Example Parameters
```js
params: [
  "0x1",
  false
]
```  

##### Returns

`Object` - A fruit object, or `null` when no fruit  was found:
  - `difficulty`: `QUANTITY` - integer of the difficulty for this snail reward block.
  - `extraData`: `DATA` - the "extra data" field of this reward block.
  - `fastHash`: `DATA`, 32 Bytes - hash of the fruit.
  - `fastNumber`: `QUANTITY` - fruit.
  - `fruitDifficulty`: `QUANTITY` - integer of all fruit difficulty for the fruit.
  - `hash`: `DATA`, 32 Bytes - hash of the fruit. 
  - `miner`: `DATA`, 20 Bytes - the address of the beneficiary to whom the mining rewards were given.
  - `mixHash`: `DATA`, 32 Bytes - the mix digest.
  - `nonce`: `QUANTITY` - the mining nonce.
  - `number`: `QUANTITY` - the fruit number.
  - `size`: `QUANTITY` - integer the size of the fruit in bytes.
  - `timestamp`: `QUANTITY` - the unix timestamp for when the fruit was collated.

##### Example
```js
// Request
curl -X POST --data '{"jsonrpc":"2.0", "method":"abey_getFruitByNumber", "params":["0x1",false],"id":100}'

// Result
{
	"jsonrpc": "2.0",
	"id": 100,
	"result": {
		"difficulty": "0x4e20",
		"extraData": "0xd9820a008667657472756588676f312e31302e38856c696e7578",
		"fastHash": "0x190c33741f18c86ac9b3b3e1102e35c95a354cb6c9214ccc1f0fa28f950840b5",
		"fastNumber": 1,
		"fruitDifficulty": "0x64",
		"hash": "0xd5d8719f456a59249570bb7adf9e37c7d2e30621fbc0aab8ee2817bf591e3acc",
		"miner": "0x7c357530174275dd30e46319b89f71186256e4f7",
		"mixHash": "0xb01e70ab492dcede93395389b9193201011e950f181180f78ad98ae58184601d",
		"nonce": "0x033aebb6a27a911a",
		"number": 1,
		"signs": 5,
		"size": 865,
		"timestamp": "0x5c87b12a"
	}
}
```

***

#### abey_getFruitByHash

get fruit block by fruit hash


##### Parameters
1. `DATA`, 32 Bytes - hash of fast block.
2. `Boolean` -  if contais fruit signs of committee member.

##### Example Parameters
```js
params: [
  "0x190c33741f18c86ac9b3b3e1102e35c95a354cb6c9214ccc1f0fa28f950840b5",
  false
]
``` 

##### Returns

  See [abey_getFruitByNumber](#abey_getFruitByNumber)

##### Example
```js
// Request
curl -X POST --data '{"jsonrpc":"2.0", "method":"abey_getFruitByHash", "params":["0x190c33741f18c86ac9b3b3e1102e35c95a354cb6c9214ccc1f0fa28f950840b5",false],"id":100}'
```

// Result
  See [abey_getFruitByNumber](#abey_getFruitByNumber)


***


#### abey_getBlockFruitCountByNumber
> > 
Returns the number of fruits in a block matching the given block number.


##### Parameters

1. `QUANTITY|TAG` - integer of a block number, or the string `"earliest"`, `"latest"` or `"pending"`, as in the [default block parameter](#the-default-block-parameter).

##### Example Parameters
```js
params: [
   '0xe8', // 232
]
```

##### Returns

`QUANTITY` - integer of the number of transactions in this block.

##### Example
```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"abey_getBlockFruitCountByNumber","params":["0xe8"],"id":1}'

// Result
{
  "id":1,
  "jsonrpc": "2.0",
  "result": "0xa" // 10
}
```

***

#### abey_getBlockFruitCountByHash

Returns the number of fruits in a block from a block matching the given block hash.


##### Parameters

1. `DATA`, 32 Bytes - hash of a block.

##### Example Parameters
```js
params: [
   '0xb903239f8543d04b5dc1ba6579132b143087c68db1b2168786408fcbce568238'
]
```

##### Returns

`QUANTITY` - integer of the number of transactions in this block.


##### Example
```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"abey_getBlockFruitCountByHash","params":["0xb903239f8543d04b5dc1ba6579132b143087c68db1b2168786408fcbce568238"],"id":1}'

// Result
{
  "id":1,
  "jsonrpc": "2.0",
  "result": "0xc" // 11
}
```

***

#### abey_getRewardBlock

return the fast block position where the given snail block is rewarded


##### Parameters
1. `QUANTITY` - integer of the snail reward block number

##### Example Parameters
```js
params: [
  "0x15"
]
``` 

##### Returns

`Object` - A fast block object, or `null` when no snail reward block was found:

  - `committeeRoot`: `DATA`, 32 Bytes - hash of committee members.
  - `snailHash`: `DATA`, 32 Bytes - hash of the snail reward block. 
  - `snailNumber`: `QUANTITY` - the reward snail number.
  - `extraData`: `DATA` - the "extra data" field of fast block.
  - `gasLimit`: `QUANTITY` - the maximum gas allowed in fast block.
  - `gasUsed`: `QUANTITY` - the maximum gas allowed in fast block.
  - `hash`: `DATA`, 32 Bytes - hash of the fast block. 
  - `logsBloom`: `DATA`, 256 Bytes - the bloom filter for the logs of the block. `null` when its pending block.
  - `number`: `QUANTITY` - the fast block number.
  - `parentHash`: `DATA`, 32 Bytes - hash of the parent block. 
  - `receiptsRoot`: `DATA`, 32 Bytes - the root of the receipts trie of the fast block.
  - `signs`: `Array` - Array of each committee member sign. 
  - `size`: `QUANTITY` - integer the size of the fast block in bytes. 
  - `stateRoot`: `DATA`, 32 Bytes - the root of the final state trie of the fast block.
  - `switchInfos`: `Array`,  - Array of switch committee member. 
  - `timestamp`: `QUANTITY` - the unix timestamp for when the fast block was collated. 
  - `transactions`: `Array` - Array of transaction objects, or 32 Bytes transaction hashes depending on the last given parameter.
  - `transactionsRoot`: `DATA`, 32 Bytes - the root of the transaction trie of the fast block.


##### Example
```js
// Request
curl -X POST --data '{"jsonrpc":"2.0", "method":"abey_getRewardBlock", "params":["0x15"],"id":100}'

// Result
{
	"jsonrpc": "2.0",
	"id": 100,
	"result": {
		"committeeRoot": "0x1dcc4de8dec75d7aab85b567b6ccd41ad312451b948a7413f0a142fd40d49347",
		"SnailHash": "0xeae9f91963eb64f0ca6e5bcf8551d8efd0d1c7f8d5ff207856c303faef86e419",
		"SnailNumber": 21,
		"extraData": "0x",
		"gasLimit": "0x4b99999",
		"gasUsed": "0x668a0",
		"hash": "0x5cc873dd449c7c15676211d8adf334da8e46b65cfa336e638678fbe4bc05ae5b",
		"logsBloom": "0x00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000",
		"number": "0x7e5",
		"parentHash": "0x3f1105c6fa853944a84fe9ef7eefcc2bcbb08860c302aed652c773d1bbc0b2f0",
		"receiptsRoot": "0x824c56f642d67e881555ec142168cc2364a1343129e34044dea382088cb5fa42",
		"signs": [{
			"fastHash": "0x5cc873dd449c7c15676211d8adf334da8e46b65cfa336e638678fbe4bc05ae5b",
			"fastHeight": "0x7e5",
			"result": 1,
			"sign": "0x763026fef783d8bfdf4b06309a1aafbebce229e846383131dc430c6c29ff42bd033d3719f4c70e96bd5a80e46a9a67d2f647fbc7c2e0a7b32c0cf8824feca29300"
		}, {
			"fastHash": "0x5cc873dd449c7c15676211d8adf334da8e46b65cfa336e638678fbe4bc05ae5b",
			"fastHeight": "0x7e5",
			"result": 1,
			"sign": "0x4c11748616eda54ca216fc8fd28957459c629caea7967e73f23ac02419e9dfba4650b7cbfd3fd9a0f4737724234941c98fa9a6ad00ffd3f860e9dd5be5ac366c00"
		}, {
			"fastHash": "0x5cc873dd449c7c15676211d8adf334da8e46b65cfa336e638678fbe4bc05ae5b",
			"fastHeight": "0x7e5",
			"result": 1,
			"sign": "0x86d689580eac3ce23453dec705ed8cd5c0ed992d0a4296ebea4bc5373f90bb1a56fbc121d6146f4c7ac6416e74b5a876012e25497dd70402b3461c13c9475a2800"
		}, {
			"fastHash": "0x5cc873dd449c7c15676211d8adf334da8e46b65cfa336e638678fbe4bc05ae5b",
			"fastHeight": "0x7e5",
			"result": 1,
			"sign": "0xaa1d76682f8ce81fa449819a6921808010114d4433945da40d323d5d8f57ebac78fc7096fbf0307dcdac5e8c9e6b2be80aa3959a6e6304d85d7a534e0e2714cc00"
		}, {
			"fastHash": "0x5cc873dd449c7c15676211d8adf334da8e46b65cfa336e638678fbe4bc05ae5b",
			"fastHeight": "0x7e5",
			"result": 1,
			"sign": "0x90b4ce8a7a13d070d9b360173e1c96dd7d95c33e7f7689187117dafae46393a8764ce36db25881fcbf82ba692b18eb2450aa2f69667391e6c55025d9f6704d1701"
		}],
		"size": "0xca7",
		"stateRoot": "0xc422d6752c0aa8e0a773ddd5b12950f3c30361ce7320c6fa59eb77c9f36ca07e",
		"switchInfos": [],
		"timestamp": "0x5c87bf4c",
		"transactions": ["0xee311d9a6e95b0740488a8c797fc36b0ddbba97005c8623d0c8258509ad0e34b", "0x9be94692e83f76fbf8a9ff7a4b9caa83141bf5c7bd91b768669802b18357146c", "0x167c29e6b6278a999c0debe02b51407cbe9f23e701f53fa2c80e141ab42f8cd4", "0x481f16a6866ce56774c5a14bfbf9a19391868761d09403acaea4d9a74fe33bd7", "0x133457504d0c23f84b58b9f82aae15012b48c5bcb5fcff904d795fe78de1ce88", "0xff98231ecd6b0358d15eec72ebb8920ed5312c30305fb21845485316b1cf8ddd", "0x3282ec6633cb743a2e177f0a999b1f366bcf900069c33c40a4a1c3b7b4fc5ed6", "0x897e1b6226666a5c031bf528527e68efb63a7d3c4b6431d758858c554a3cf38a", "0x034bf45b679a2283deec9c3c514b0dfe6dc1ab4980864ceeafd9989bd56410f0", "0x129bc008bcae647a55d900068ae657e3f9073a37d03933d149ba529abd15c63c", "0xc5831516e417510b6f4069093c8845de24e62a1adee409c35164b8b6cf26465d", "0xb0ef3a7209af20990b2e346dc7f963d11e0da3a42ae04640502b81e30ac96202", "0x8c79698e30cbd7c50781eaa6077667428cf51027086762d9349ee9987d9ebe36", "0xf48839d7212c9d01684237131f68f81a30fe6635eb39b0030bab3e68c75bbefa", "0x94aff39d085551389a198378137e5dd694ce689e5ad326910f18f167ff81f16c", "0x882c79c2678a0fc0099bca068ca05bb239405c8a2f363a3b755b4049c67f41c7", "0xaa5855c69051b82716bdc9b7f9190ef06c06411d0d43d8786fbf14a3898446a6", "0x508b4357b911624c11bb5a90d880c86ed92f5f430454f64b6e63ea884cec0589", "0x652a73ec17dab453db88a9e292bf684a814101d7013a489fbbae98ed9c221f03", "0xa95cbff42440d0a000ad55969f95fc5b5ff947efd95dbb0f9607acf849dbfbd9"],
		"transactionsRoot": "0xcfcce7b64115b9086a78c0449ceb70aac9b08c0d221943470ffacbbdc71fba9a"
	}
}
```


***

#### abey_getSnailBlockByNumber

get snail block by number


##### Parameters
1. `QUANTITY` - integer of the snail block number,or the string `"latest"`, `"earliest"` or `"pending"`, see the [default block parameter](#the-default-block-parameter)
2. `Boolean` -  whether show fruits in snail block

##### Example Parameters
```js
params: [
  "0x19",
  false
]
``` 


##### Returns

`Object` - A snail block object, or `null` when no snail block was found:

  - `beginFruitNumber`: `QUANTITY` - the beign fruit number in snail reward block.
  - `difficulty`: `QUANTITY` - integer of the difficulty for this snail reward block.
  - `endFruitNumber`: `QUANTITY` - the end fruit number in snail reward block.
  - `extraData`: `DATA` - the "extra data" field of this reward block.
  - `fruitDifficulty`: `QUANTITY` - integer of all fruit difficulty for this snail reward block.
  - `fruitsHash`: `DATA`, 32 Bytes - hash of fruits in snail reward block.
  - `fruits`: `Array` -  Array of all hash of fruits in snail reward block.
  - `hash`: `DATA`, 32 Bytes - hash of the snail reward block. 
  - `miner`: `DATA`, 20 Bytes - the address of the beneficiary to whom the mining rewards were given.
  - `mixHash`: `DATA`, 32 Bytes - the mix digest.
  - `nonce`: `QUANTITY` - the mining nonce.
  - `number`: `QUANTITY` - the snail block number.
  - `parentHash`: `DATA`, 32 Bytes - hash of the parent block.
  - `pointerNumber`: `QUANTITY` - current snail reward block minus 7.
  - `size`: `QUANTITY` - integer the size of this reward block in bytes.
  - `timestamp`: `QUANTITY` - the unix timestamp for when the block was collated.

##### Example
```js
// Request
curl -X POST --data '{"jsonrpc":"2.0", "method":"abey_getSnailBlockByNumber", "params":["0x19",false],"id":100}'

// Result
{
	"jsonrpc": "2.0",
	"id": 100,
	"result": {
		"beginFruitNumber": "0x5a9",
		"difficulty": "0x9e86",
		"endFruitNumber": "0x5e4",
		"extraData": "0xd9820a008667657472756588676f312e31302e38856c696e7578",
		"fruitDifficulty": "0x64",
		"fruitsHash": "0x02c44355f9aa43305f7507efd77ab13a74b0a7bde1d923ac62135e8e3bfe4afd",
		"fruits": 60,
		"hash": "0x6fd8a5f62ec65a6f244be17a32605c705736ed960130bad87b79730312d77741",
		"miner": "0x7c357530174275dd30e46319b89f71186256e4f7",
		"mixHash": "0x000122785a555ca6b63c54e1ff398cf5ae9f6176391de526a7d3a9277b13cfaf",
		"nonce": "0x719c13f20436b852",
		"number": "0x19",
		"parentHash": "0x1d2361639c4b26f9481caf680e6db21af1c8d9159f804b01a37611a1e057487a",
		"pointerNumber": 17,
		"size": "0xcee1",
		"timestamp": "0x5c87bbcf"
	}
}
```


***

#### abey_getSnailBlockByHash

get snail block by hash


##### Parameters

1. `DATA`, 32 Bytes - hash of snail block .
2. `Boolean` -  if contais fruit signs of committee member.

##### Example Parameters
```js
params: [
  "0x6fd8a5f62ec65a6f244be17a32605c705736ed960130bad87b79730312d77741",
  false
]
``` 


##### Returns

 See [abey_getSnailBlockByNumber](#abey_getSnailBlockByNumber)

##### Example
```js
// Request
curl -X POST --data '{"jsonrpc":"2.0", "method":"abey_getSnailBlockByHash", "params":["0x6fd8a5f62ec65a6f244be17a32605c705736ed960130bad87b79730312d77741",false],"id":100}'
```

// Result
 See [abey_getSnailBlockByNumber](#abey_getSnailBlockByNumber)

***

#### abey_getDataset

the Minverva algorithm calculates the required source of dataset, and updates the dataset every 12,000


##### Parameters

none


##### Returns

[null,null,null,null,null,null....,null].
-when snailBlock number exceed 12000, return not nil 

##### Example
```js
// Request
curl -X POST --data '{"jsonrpc":"2.0", "method":"abey_getDataset", "params":[],"id":100}'

// Result
{
	"jsonrpc": "2.0",
	"id": 100,
	"result": {
		  [null, null, null, null, ......, null, null]
	}
}
```

***

#### abey_getSnailRewardContent

get snailBlock reward imfomation, including snailBlock miner,fruit miner and committee reward for each fruit 


##### Parameters

1. `QUANTITY` - integer of a block number.


##### Returns

- `blockminer`:  -snailblock miner address and  reward 
- `committeReward`:  -committee member address and reward for all fruit 
- `fruitminer`:  -fruit miner address and reward for each fruit

##### Example
```js
// Request
curl -X POST --data '{"jsonrpc":"2.0", "method":"abey_getSnailRewardContent", "params":["0x1"],"id":100}'

// Result
{
	"jsonrpc": "2.0",
	"id": 100,
	"result": {
		"blockminer": {
			"0x7c357530174275dd30e46319b89f71186256e4f7": 61629629629629000000
		},
		"committeReward": {
			"0x7c357530174275dd30e46319b89f71186256e4f7": 23111111111110999785
		},
		"fruitminer": [{
			"0x7c357530174275dd30e46319b89f71186256e4f7": 446591519055275362
		}, {
			"0x7c357530174275dd30e46319b89f71186256e4f7": 446591519055275362
		}, {
			"0x7c357530174275dd30e46319b89f71186256e4f7": 446591519055275362
		}, {
			"0x7c357530174275dd30e46319b89f71186256e4f7": 446591519055275362
		}, {
			"0x7c357530174275dd30e46319b89f71186256e4f7": 446591519055275362
		}, {
			"0x7c357530174275dd30e46319b89f71186256e4f7": 446591519055275362
		}, {
			"0x7c357530174275dd30e46319b89f71186256e4f7": 446591519055275362
		}, {
			"0x7c357530174275dd30e46319b89f71186256e4f7": 446591519055275362
		}, {
			"0x7c357530174275dd30e46319b89f71186256e4f7": 446591519055275362
		}, {
			"0x7c357530174275dd30e46319b89f71186256e4f7": 446591519055275362
		}, {
			"0x7c357530174275dd30e46319b89f71186256e4f7": 446591519055275362
		}, {
			"0x7c357530174275dd30e46319b89f71186256e4f7": 446591519055275362
		}, {
			"0x7c357530174275dd30e46319b89f71186256e4f7": 446591519055275362
		}, {
			"0x7c357530174275dd30e46319b89f71186256e4f7": 446591519055275362
		}, {
			"0x7c357530174275dd30e46319b89f71186256e4f7": 446591519055275362
		}, {
			"0x7c357530174275dd30e46319b89f71186256e4f7": 446591519055275362
		}, {
			"0x7c357530174275dd30e46319b89f71186256e4f7": 446591519055275362
		}, {
			"0x7c357530174275dd30e46319b89f71186256e4f7": 446591519055275362
		}, {
			"0x7c357530174275dd30e46319b89f71186256e4f7": 446591519055275362
		}, {
			"0x7c357530174275dd30e46319b89f71186256e4f7": 446591519055275362
		}, {
			"0x7c357530174275dd30e46319b89f71186256e4f7": 446591519055275362
		}, {
			"0x7c357530174275dd30e46319b89f71186256e4f7": 446591519055275362
		}, {
			"0x7c357530174275dd30e46319b89f71186256e4f7": 446591519055275362
		}, {
			"0x7c357530174275dd30e46319b89f71186256e4f7": 446591519055275362
		}, {
			"0x7c357530174275dd30e46319b89f71186256e4f7": 446591519055275362
		}, {
			"0x7c357530174275dd30e46319b89f71186256e4f7": 446591519055275362
		}, {
			"0x7c357530174275dd30e46319b89f71186256e4f7": 446591519055275362
		}, {
			"0x7c357530174275dd30e46319b89f71186256e4f7": 446591519055275362
		}, {
			"0x7c357530174275dd30e46319b89f71186256e4f7": 446591519055275362
		}, {
			"0x7c357530174275dd30e46319b89f71186256e4f7": 446591519055275362
		}, {
			"0x7c357530174275dd30e46319b89f71186256e4f7": 446591519055275362
		}, {
			"0x7c357530174275dd30e46319b89f71186256e4f7": 446591519055275362
		}, {
			"0x7c357530174275dd30e46319b89f71186256e4f7": 446591519055275362
		}, {
			"0x7c357530174275dd30e46319b89f71186256e4f7": 446591519055275362
		}, {
			"0x7c357530174275dd30e46319b89f71186256e4f7": 446591519055275362
		}, {
			"0x7c357530174275dd30e46319b89f71186256e4f7": 446591519055275362
		}, {
			"0x7c357530174275dd30e46319b89f71186256e4f7": 446591519055275362
		}, {
			"0x7c357530174275dd30e46319b89f71186256e4f7": 446591519055275362
		}, {
			"0x7c357530174275dd30e46319b89f71186256e4f7": 446591519055275362
		}, {
			"0x7c357530174275dd30e46319b89f71186256e4f7": 446591519055275362
		}, {
			"0x7c357530174275dd30e46319b89f71186256e4f7": 446591519055275362
		}, {
			"0x7c357530174275dd30e46319b89f71186256e4f7": 446591519055275362
		}, {
			"0x7c357530174275dd30e46319b89f71186256e4f7": 446591519055275362
		}, {
			"0x7c357530174275dd30e46319b89f71186256e4f7": 446591519055275362
		}, {
			"0x7c357530174275dd30e46319b89f71186256e4f7": 446591519055275362
		}, {
			"0x7c357530174275dd30e46319b89f71186256e4f7": 446591519055275362
		}, {
			"0x7c357530174275dd30e46319b89f71186256e4f7": 446591519055275362
		}, {
			"0x7c357530174275dd30e46319b89f71186256e4f7": 446591519055275362
		}, {
			"0x7c357530174275dd30e46319b89f71186256e4f7": 446591519055275362
		}, {
			"0x7c357530174275dd30e46319b89f71186256e4f7": 446591519055275362
		}, {
			"0x7c357530174275dd30e46319b89f71186256e4f7": 446591519055275362
		}, {
			"0x7c357530174275dd30e46319b89f71186256e4f7": 446591519055275362
		}, {
			"0x7c357530174275dd30e46319b89f71186256e4f7": 446591519055275362
		}, {
			"0x7c357530174275dd30e46319b89f71186256e4f7": 446591519055275362
		}, {
			"0x7c357530174275dd30e46319b89f71186256e4f7": 446591519055275362
		}, {
			"0x7c357530174275dd30e46319b89f71186256e4f7": 446591519055275362
		}, {
			"0x7c357530174275dd30e46319b89f71186256e4f7": 446591519055275362
		}, {
			"0x7c357530174275dd30e46319b89f71186256e4f7": 446591519055275362
		}, {
			"0x7c357530174275dd30e46319b89f71186256e4f7": 446591519055275362
		}, {
			"0x7c357530174275dd30e46319b89f71186256e4f7": 446591519055275362
		}, {
			"0x7c357530174275dd30e46319b89f71186256e4f7": 446591519055275362
		}, {
			"0x7c357530174275dd30e46319b89f71186256e4f7": 446591519055275362
		}, {
			"0x7c357530174275dd30e46319b89f71186256e4f7": 446591519055275362
		}, {
			"0x7c357530174275dd30e46319b89f71186256e4f7": 446591519055275362
		}, {
			"0x7c357530174275dd30e46319b89f71186256e4f7": 446591519055275362
		}, {
			"0x7c357530174275dd30e46319b89f71186256e4f7": 446591519055275362
		}, {
			"0x7c357530174275dd30e46319b89f71186256e4f7": 446591519055275362
		}, {
			"0x7c357530174275dd30e46319b89f71186256e4f7": 446591519055275362
		}, {
			"0x7c357530174275dd30e46319b89f71186256e4f7": 446591519055275362
		}]
	}
}
```

***

#### abey_getChainRewardContent

get snailBlock pos reward imfomation, including snailBlock miner,fruit miner and committee reward for each fruit 

##### Parameters

1. `QUANTITY` - integer of a snail block hex number.
2. `DATA`, 20 Bytes - address of the validator, when `"0x0000000000000000000000000000000000000000"` query all



##### Returns

- `Number`: `QUANTITY`,      -snailblock number 
- `blockminer`: `QUANTITY`,  -snailblock miner address and  reward 
- `committeReward`: `Array`,  -committee member address and reward for all fruit 
    - `Address`:  `DATA`, 20 Bytes - address of the delegate.
    - `Amount`: `QUANTITY`, - reward amount
    - `Staking`:`QUANTITY`, - staking amount when cal reward
- `foundationReward`:  -foundation reward 
- `fruitminer`:  -fruit miner address and reward for each fruit
- `time`:  -miner snail block time

##### Example
```js
// Request
curl -X POST --data '{"jsonrpc":"2.0", "method":"abey_getChainRewardContent", "params":["0x9b44","0x0000000000000000000000000000000000000000"],"id":100}'

// Result
{
	"jsonrpc": "2.0",
	"id": 100,
	"result": {
                Number: "0x9b40",
                blockminer: {
                  Address: "0x3dd442d92e887700f61b0d29aa73094ecedde7a1",
                  Amount: 52432209984299000000,
                  Staking: null
                },
                committeReward: [{
                    Items: [{...}, {...}, {...}]
                }, {
              	  Items: [{...}, {...}]
                }, {
                    Items: [{...}, {...}]
                }, {
                    Items: [{...}]
                }, {
                    Items: [{...}]
                }, {
                    Items: [{...}]
                }, {
                    Items: [{...}]
                }],
                foundationReward: {
                  Address: "0xda79b1c2645750c655d848e04c27e2cd9d263c48",
                  Amount: 0,
                  Staking: null
                },
                fruitminer: [{
                    Address: "0x3dd442d92e887700f61b0d29aa73094ecedde7a1",
                    Amount: 26216104992149000000,
                    Staking: null
                }],
                time: "0x5e6036ac"
              }
}
```

***

#### abey_balance

Returns the balance of the account of given address.

##### Parameters

1. `DATA`, address formatted by abey - address to check for balance.
2. `QUANTITY|TAG` - integer block number, or the string `"latest"`, `"earliest"` or `"pending"`, see the [default block parameter](#the-default-block-parameter)

##### Example Parameters
```js
params: [
   'ABEYWkjDk3QyxZqshGPk6KtasYDc97N6yZTk2',
   'latest'
]
```

##### Returns

`QUANTITY` - integer of the current balance in wei.


##### Example
```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"abey_balance","params":["ABEYWkjDk3QyxZqshGPk6KtasYDc97N6yZTk2", "latest"],"id":1}'

// Result
{
  "id":1,
  "jsonrpc": "2.0",
  "result": "0x0234c8a3397aab58" // 158972490234375000
}
```

***

## Impawn

#### impawn_getAllStakingAccount

Returns  information about all validator staking info, contain staking count.

##### Parameters
1. `QUANTITY|TAG` - integer block number, or the string `"latest"`, `"earliest"` or `"pending"`, see the [default block parameter](#the-default-block-parameter)

##### Example Parameters
```js
params: [
   'latest'
]
```

##### Returns

  - `delegateCount`: `QUANTITY`, the all delegate count.
  - `stakerCount`: `QUANTITY`- the all staking member count.
  - `stakers`: `Array`, detail validator member info.
      - `committee`:  `TAG` - `true` it select as committee. `false` it no not select
      - `delegation`: `Array`, detail delegate member info of this staker.
          - `delegation`: `QUANTITY`- delagate count.
          - `saAddress`:  `DATA`, 20 Bytes - address of the validator.
          - `unit`: `Object`- contain detail delegate info.
          - `validDelegate`: `QUANTITY`- delagate count not contain redeem info.
      - `fee`: `QUANTITY`- validator fee for share reward with delegate.
      - `id`: `QUANTITY`, validator index.
      - `modify`: `Object`, update fee and pub key.
      - `staking`: `DATA`, staking count.
      - `unit`: `Object`, contain detail staking info.
      - `validStaking`: `QUANTITY`, staker count not contain redeem info.
      - `votePubKey`: `DATA`, 32 Bytes - bft communication key.

##### Example
```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"impawn_getAllStakingAccount","params":["latest"],"id":100}'

// Result
{
  "id":100,
  "jsonrpc":"2.0",
  "result":{
             delegateCount: 0,
             stakerCount: 4,
             stakers: [{
                 committee: false,
                 delegation: {
                    0: {
                      delegate: 10000000000000000000,
                      saAddress: "0xc02f50f4f41f46b6a2f08036ae65039b2f9acd69",
                      unit: {
                        address: "0x7c357530174275dd30e46319b89f71186256e4f7",
                        redeemInfo: {},
                        value: {...}
                      },
                      validDelegate: 10000000000000000000
                    }
                 },
                 fee: 0,
                 id: 0,
                 modify: {
                   fee: 0,
                   votePubKey: "0x"
                 },
                 staking: 3.0172e+22,
                 unit: {
                   address: "0xc02f50f4f41f46b6a2f08036ae65039b2f9acd69",
                   redeemInfo: {...},
                   value: {...}
                 },
                 validStaking: 3.0172e+22,
                 votePubKey: "0x040743b25066dac37d3552e63c9ba3325884ba28d0906ce5fd9a18a55312ef478c2b9c33f6ef519c1fd0e42884a25bf2be1ea83f3354a3e54243d521c37865c062"
             }, {
                 committee: false,
                 delegation: {},
                 fee: 0,
                 id: 1,
                 modify: {
                   fee: 0,
                   votePubKey: "0x"
                 },
                 staking: 2e+22,
                 unit: {
                   address: "0x6d348e0188cc2596aaa4046a1d50bb3ba50e8524",
                   redeemInfo: {},
                   value: {...}
                 },
                 validStaking: 2e+22,
                 votePubKey: "0x04a8bd50e35f99f77edda08e333339ab49db89f0c81f49a37e7d4ddb84a06738ff54becf677f3ea6d9abdd321e50d77680871f408dcb7a17dfa7f0d8bb5229d831"
             }, {
                 committee: false,
                 delegation: {},
                 fee: 0,
                 id: 2,
                 modify: {
                   fee: 0,
                   votePubKey: "0x"
                 },
                 staking: 2e+22,
                 unit: {
                   address: "0xe803895897c3ccd35315b2e41c95f817543811a5",
                   redeemInfo: {},
                   value: {...}
                 },
                 validStaking: 2e+22,
                 votePubKey: "0x0439d8ca80e0f4bc3a1aa203fa280909f426db90436dabcc9a1a3ef8efaa9d0ea8a9647eaa821b8346a92eed28cd707cb062ec54b31b819d0bf03af79fd63ea40e"
             }, {
                 committee: false,
                 delegation: {},
                 fee: 0,
                 id: 3,
                 modify: {
                   fee: 0,
                   votePubKey: "0x"
                 },
                 staking: 2e+22,
                 unit: {
                   address: "0x3f739ffd8a59965e07e1b8d7cca938125bce8cfb",
                   redeemInfo: {},
                   value: {...}
                 },
                 validStaking: 2e+22,
                 votePubKey: "0x046bf0f9bb43ac46efe9bd58a465a4b2f9cf8962b728907471f2b4b51a899c3eb166b4a3ee69eac012abc39cd91451dd20089b1371c2acf6e18066d0e8c2cab717"
             }]
           }
}
```

***

#### impawn_getStakingAsset

Returns  information about deposit info by address.

##### Parameters

1. `DATA`, 20 Bytes - address to check for balance.
2. `QUANTITY|TAG` - integer block number, or the string `"latest"`, `"earliest"` or `"pending"`, see the [default block parameter](#the-default-block-parameter)

##### Example Parameters
```js
params: [
   '0xc94770007dda54cF92009BFF0dE90c06F603a09f',
   'latest'
]
```

##### Returns

  - `address`: `DATA`, 20 Bytes - address of the validator.
  - `lockValue`: `Array`, detail validator member info.
      - `amount`: `QUANTITY`- deposit amount value.
      - `height`: `QUANTITY`, deposit tx height.

##### Example
```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"impawn_getStakingAsset","params":["0xc94770007dda54cF92009BFF0dE90c06F603a09f", "latest"],"id":100}'

// Result
{
  "id":100,
  "jsonrpc":"2.0",
  "result":[{
               address: "0xc02f50f4f41f46b6a2f08036ae65039b2f9acd69",
               stakingValue: [{
                   amount: 3.0172e+22,
                   height: 122380
               }]
           }]
}
```

***

#### impawn_getLockedAsset

Returns  information about cancel info by address.

##### Parameters

1. `DATA`, 20 Bytes - address to check for balance.
2. `QUANTITY|TAG` - integer block number, or the string `"latest"`, `"earliest"` or `"pending"`, see the [default block parameter](#the-default-block-parameter)

##### Example Parameters
```js
params: [
   '0xc94770007dda54cF92009BFF0dE90c06F603a09f',
   'latest'
]
```

##### Returns

  - `address`: `DATA`, 20 Bytes - address of the validator.
  - `lockValue`: `Array`, detail validator member info.
      - `amount`: `QUANTITY`- lock amount value.
      - `epochID`: `QUANTITY`, you deposit belong to epoch.
      - `height`: `QUANTITY`, which height you can withdraw.
      - `locked`: `TAG` - `true` instant withdraw. `false` after height can withdraw.

##### Example
```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"impawn_getLockedAsset","params":["0xc94770007dda54cF92009BFF0dE90c06F603a09f", "latest"],"id":100}'

// Result
{
  "id":100,
  "jsonrpc":"2.0",
  "result":[{
               address: "0xc02f50f4f41f46b6a2f08036ae65039b2f9acd69",
               lockValue: [{
                   amount: 1000000000000000000,
                   epochID: 30,
                   height: 115381,
                   locked: false
               }]
           }]
}
```

***

#### impawn_getAllCancelableAsset

Returns  information about can cancel count info by address.

##### Parameters

1. `DATA`, 20 Bytes - address to check for balance.
2. `QUANTITY|TAG` - integer block number, or the string `"latest"`, `"earliest"` or `"pending"`, see the [default block parameter](#the-default-block-parameter)

##### Example Parameters
```js
params: [
   '0xc94770007dda54cF92009BFF0dE90c06F603a09f',
   'latest'
]
```

##### Returns

   - `address`:  `DATA`, 20 Bytes - address of the validator.
   - `value`:  `QUANTITY`, deposit count that you can cancel.
   
##### Example
```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"impawn_getAllCancelableAsset","params":["0xc94770007dda54cF92009BFF0dE90c06F603a09f", "latest"],"id":100}'

// Result
{
  "id":100,
  "jsonrpc":"2.0",
  "result":[{
               address: "0xc02f50f4f41f46b6a2f08036ae65039b2f9acd69",
               value: 3.0172e+22
           }] 
}
```

***

#### impawn_getStakingAccount

Returns  information about certain staking info by address.

##### Parameters

1. `DATA`, 20 Bytes - address to check for balance.
2. `QUANTITY|TAG` - integer block number, or the string `"latest"`, `"earliest"` or `"pending"`, see the [default block parameter](#the-default-block-parameter)

##### Example Parameters
```js
params: [
   '0xc94770007dda54cF92009BFF0dE90c06F603a09f',
   'latest'
]
```

##### Returns

   - `committee`:  `TAG` - `true` it select as committee. `false` it no not select
   - `delegation`: `Array`, detail delegate member info of this staker.
        - `delegation`: `QUANTITY`- delagate count.
        - `saAddress`:  `DATA`, 20 Bytes - address of the validator.
        - `unit`: `Object`- contain detail delegate info.
        - `validDelegate`: `QUANTITY`- delagate count not contain redeem info.
   - `fee`: `QUANTITY`- validator fee for share reward with delegate.
   - `id`: `QUANTITY`, validator index.
   - `modify`: `Object`, update fee and pub key.
   - `staking`: `DATA`, staking count.
   - `unit`: `Object`, contain detail staking info.
   - `validStaking`: `QUANTITY`, staker count not contain redeem info.
   - `votePubKey`: `DATA`, 32 Bytes - bft communication key.
   
##### Example
```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"impawn_getStakingAccount","params":["0xc94770007dda54cF92009BFF0dE90c06F603a09f", "latest"],"id":100}'

// Result
{
  "id":100,
  "jsonrpc":"2.0",
  "result":{
             committee: false,
             delegation: {},
             fee: 0,
             id: {},
             modify: {
               fee: 0,
               votePubKey: "0x"
             },
             staking: 3.0172e+22,
             unit: {
               address: "0xc02f50f4f41f46b6a2f08036ae65039b2f9acd69",
               redeemInfo: {
                 0: {
                   amount: "0xde0b6b3a7640000",
                   epochID: 30,
                   state: 8
                 }
               },
               value: {
                 0: {
                   amount: "0x663a09d1de48df00000",
                   height: "0x1de0c",
                   state: 2
                 }
               }
             },
             validStaking: 3.0172e+22,
             votePubKey: "0x040743b25066dac37d3552e63c9ba3325884ba28d0906ce5fd9a18a55312ef478c2b9c33f6ef519c1fd0e42884a25bf2be1ea83f3354a3e54243d521c37865c062"
           }
}
```

