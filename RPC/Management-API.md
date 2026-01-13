The APIs supported by Abey are mainly based on function modules: admin, miner, personal, etc. These APIs are provided using JSON-RPC.

## Enabling the management APIs

gabey comes with a JavaScript console that supports all of the APIs described here. To provide these APIs through gabey RPC, specify them with the --${interface}api command line argument. Where ${interface} can be HTTP rpc, WebSocket ws and Unix socket ipc or Windows pipe

For example: gabey --ipcapi admin, abey,miner --rpc api abey,web3 --rpc

1. Enable administrators, official DApp and miner APIs via IPC interface
2. Enable official DApp and web3 API via HTTP interface

The HTTP RPC interface must be explicitly enabled using the `--rpc` flag.

Please note, offering an API over the HTTP (`rpc`) or WebSocket (`ws`) interfaces will give everyone access to the APIs who can access this interface (DApps, browser tabs, etc). Be careful which APIs you enable. By default gabey enables all APIs over the IPC (`ipc`) interface and only the `db`, `abey`, `net` and `web3` APIs over the HTTP and WebSocket interfaces.

To determine which APIs an interface provides, the `modules` JSON-RPC method can be invoked. For example over an `ipc` interface on unix systems:

```
echo '{"jsonrpc":"2.0","method":"rpc_modules","params":[],"id":1}' | nc -U $datadir/gabey.ipc
```

will give all enabled modules including the version number:

```
{  
   "id":1,
   "jsonrpc":"2.0",
   "result":{  
      "admin":"1.0",
      "debug":"1.0",
      "abey":"1.0",
      "miner":"1.0",
      "net":"1.0",
      "personal":"1.0",
      "rpc":"1.0",
      "txpool":"1.0",
      "web3":"1.0"
   }
}
```

## API List



|  [admin](#admin)   |        [debug](#debug)        |    [miner](#miner)    |   [personal](#personal)    | [Txpool](#Txpool)  | [fruitpool](#fruitpool) | [abey](#abey) |
| :------: | :-----------------: | :---------: | :-----------: | :-----: | :-------: | :-------: |
| [addPeer](#admin_addPeer)  |     [backtraceAt](#debug_backtraceAt)     |  [setExtra](#miner_setExtra)   | [listAccounts](#personal_listAccounts)  | [content](#Txpool_content) |  [content](#fruitpool_content)  |  [protocolVersion](#abey_protocolVersion)  |
| [datadir](#admin_datadir)  |    [blockProfile](#debug_blockProfile)     | [setGasPrice](#miner_setGasPrice) |  [lockAccount](#personal_lockAccount)  | [inspect](#Txpool_inspect) |  [inspect](#fruitpool_inspect)  |  [syncing](#abey_syncing)  |
| [nodeInfo](#admin_nodeInfo) |     [cpuProfile](#debug_cpuProfile)      |    [start](#miner_start)    |  [newAccount](#personal_newAccount)   | [status](#Txpool_status)  |  [status](#fruitpool_status)   |  [coinbase](#abey_coinbase)  |
|   [peers](#admin_peers)    |      [dumpBlock](#debug_dumpBlock)      |    [stop](#miner_stop)     | [unlockAccount](#personal_unlockAccount) |         |           |  [mining](#abey_mining)  |
| [startRPC](#admin_startRPC) |       [gcStats](#debug_gcStats)       | [setCoinbase](#miner_setCoinbase) |     [sign](#personal_sign)      |         |           |  [hashrate](#abey_hashrate)  |
| [startWS](#admin_startWS)  |     [getBlockRlp](#debug_getBlockRlp)     | [setElection](#miner_setElection) |   [ecRecover](#personal_ecRecover)   |         |           |  [gasPrice](#abey_gasPrice)  |
| [stopRPC](#admin_stopRPC)  |       [goTrace](#debug_goTrace)       |             |               |         |           |  [accounts](#abey_accounts)  |
|  [stopWS](#admin_stopWS)  |      [memStats](#debug_memStats)       |             |               |         |           |  [blockNumber](#abey_blockNumber)  |
|          |      [seedHash](#debug_seedHash)       |             |               |         |           |  [getBalance](#abey_getBalance)  |
|          |       [setHead](#debug_setHead)       |             |               |         |           |  [getStorageAt](#abey_getStorageAt)  |
|          |       [setBlockProfileRate](#debug_setBlockProfileRate)       |             |               |         |           |  [getTransactionCount](#abey_getTransactionCount)  |
|          | [stacks](#debug_stacks) |             |               |         |           |  [getBlockTransactionCount](#abey_getBlockTransactionCount)  |
|          |       [startCPUProfile](#debug_startCPUProfile)        |             |               |         |           |  [getCode](#abey_getCode)  |
|          |   [startGoTrace](#debug_startGoTrace)   |             |               |         |           |  [sign](#abey_sign)  |
|          |    [stopCPUProfile](#debug_stopCPUProfile)     |             |               |         |           |  [sendTransaction](#abey_sendTransaction)  |
|          |   [stopGoTrace](#debug_stopGoTrace)    |             |               |         |           |  [sendRawTransaction](#abey_sendRawTransaction)  |
|          |     [traceBlock](#debug_traceBlock)     |             |               |         |           |  [call](#abey_call)  |
|          |     [traceBlockByNumber](#debug_traceBlockByNumber)     |             |               |         |           |  [estimateGas](#abey_estimateGas)  |
|          | [traceBlockByHash](#debug_traceBlockByHash)  |             |               |         |           |  [getBlock](#abey_getBlock)  |
|          |  [traceBlockFromFile](#debug_traceBlockFromFile)   |             |               |         |           |  [getTransaction](#abey_getTransaction)  |
|          | [traceTransaction](#debug_traceTransaction)  |             |               |         |           |  [getTransactionReceipt](#abey_getTransactionReceipt)  |
|          |  [verbosity](#debug_verbosity)   |             |               |         |           |  [compile](#abey_compile)  |
|          |      [vmodule](#debug_vmodule)      |             |               |         |           |  [getWork](#abey_getWork)  |
|          |       [writeBlockProfile](#debug_writeBlockProfile)       |             |               |         |           |  [defaultBlock](#abey_defaultBlock)  |
|          |  [writeMemProfile](#debug_writeMemProfile) |             |               |         |           |  [pendingTransactions](#abey_pendingTransactions)  |
|          |      |             |               |         |           |  [fruitNumber](#abey_fruitNumber)  |
|          |      |             |               |         |           |  [rewardSnailBlock](#abey_rewardSnailBlock)  |
|          |      |             |               |         |           |  [snailBlockNumber](#abey_snailBlockNumber)  |
|          |      |             |               |         |           |  [getCommittee](#abey_getCommittee)  |
|          |      |             |               |         |           |  [committeeNumber](#abey_committeeNumber)  |
|          |      |             |               |         |           |  [getCurrentState](#abey_getCurrentState)  |
|          |      |             |               |         |           |  [getFruit](#abey_getFruit)  |
|          |      |             |               |         |           |  [getRewardBlock](#abey_getRewardBlock)  |
|          |      |             |               |         |           |  [getSnall](#abey_getSnail)  |
|          |      |             |               |         |           |  [snailBlockNumber](#abey_snailBlockNumber)  |

## Admin

The `admin` API gives you access to several non-standard RPC methods, which will allow you to have a fine grained control over your gabey instance, including but not limited to network peer and RPC endpoint management.

### admin_addPeer

The `addPeer` administrative method requests adding a new remote node to the list of tracked static nodes. The node will try to maintain connectivity to these nodes at all times, reconnecting every once in a while if the remote connection goes down.

The method accepts a single argument, the enode URL of the remote peer to start tracking and returns a `BOOL` indicating whether the peer was accepted for tracking or some error occurred.

| Client  | Method invocation                        |
| ------- | ---------------------------------------- |
| Go      | `admin.AddPeer(url string) (bool, error)` |
| Console | `admin.addPeer(url)`                     |
| RPC     | `{"method": "admin_addPeer", "params": [url]}` |

#### Example

```
> admin.addPeer("enode://0adc97d2046e0b96956feed699b92fc34d2e5ba1f7ab411c884786ded213f70c0e78f0acce7a548ab32e1c2a817b63c43d0d7209f3f9050749a0720825631b55@192.168.46.59:30313")
true
```

### admin_datadir

The `datadir` administrative property can be queried for the absolute path the running gabey node currently uses to store all its databases.

| Client  | Method invocation                 |
| ------- | --------------------------------- |
| Go      | `admin.Datadir() (string, error`) |
| Console | `admin.datadir`                   |
| RPC     | `{"method": "admin_datadir"}`     |

#### Example

```
> admin.datadir
"/home/abeychain/go-abey/cmd/gabey/data"
```

### admin_nodeInfo

The `nodeInfo` administrative property can be queried for all the information known about the running gabey  node at the networking granularity. These include general information about the node itself as a participant of the P2P overlay protocol, as well as specialized information added by each of the running application protocols (e.g. `abey`, `les`, `shh`).

| Client  | Method invocation                        |
| ------- | ---------------------------------------- |
| Go      | `admin.NodeInfo() (*p2p.NodeInfo, error`) |
| Console | `admin.nodeInfo`                         |
| RPC     | `{"method": "admin_nodeInfo"}`           |

#### Example

```
> admin.nodeInfo
{
  enode: "enode://44826a5d6a55f88a18298bca4773fca5749cdc3a5c9f308aa7d810e9b31123f3e7c5fba0b1d70aac5308426f47df2a128a6747040a3815cc7dd7167d03be320d@[::]:30303",
  id: "44826a5d6a55f88a18298bca4773fca5749cdc3a5c9f308aa7d810e9b31123f3e7c5fba0b1d70aac5308426f47df2a128a6747040a3815cc7dd7167d03be320d",
  ip: "::",
  listenAddr: "[::]:30303",
  name: "gabey/v0.7.2-unstable-46f28475/linux-amd64/go1.10.1",
  ports: {
    discovery: 30303,
    listener: 30303
  },
  protocols: {
    abey: {
      difficulty: null,
      genesis: "0xd4e56740f876aef8c010b86a40d5f56745a118d0906a34e69aec8c0db1cb8fa3",
      head: "0xb83f73fbe6220c111136aefd27b160bf4a34085c65ba89f24246b3162257c36a",
      network: 20
    }
  }
}
```

### admin_peers

You can query the peers property for information about all remote nodes that are connected.

| Client  | Method invocation                        |
| ------- | ---------------------------------------- |
| Go      | `admin.Peers() ([]*p2p.PeerInfo, error`) |
| Console | `admin.peers`                            |
| RPC     | `{"method": "admin_peers"}`              |

#### Example

```
> admin.peers
[{
    caps: ["abey/61", "abey/62", "abey/63"],
    id: "08a6b39263470c78d3e4f58e3c997cd2e7af623afce64656cfc56480babcea7a9138f3d09d7b9879344c2d2e457679e3655d4b56eaff5fd4fd7f147bdb045124",
    name: "gabey/v0.7.2-unstable-46f28475/linux-amd64/go1.10.1",
    network: {
      localAddress: "192.168.0.104:51068",
      remoteAddress: "71.62.31.72:30303"
    },
    protocols: {
      abey: {
        difficulty: null,
        head: "5794b768dae6c6ee5366e6ca7662bdff2882576e09609bf778633e470e0e7852",
        version: 63
      }
    }
}, /* ... */ {
    caps: ["abey/61", "abey/62", "abey/63"],
    id: "fcad9f6d3faf89a0908a11ddae9d4be3a1039108263b06c96171eb3b0f3ba85a7095a03bb65198c35a04829032d198759edfca9b63a8b69dc47a205d94fce7cc",
    name: "gabey/v0.7.2-unstable-46f28475/linux-amd64/go1.10.1",
    network: {
      localAddress: "192.168.0.104:55968",
      remoteAddress: "121.196.232.205:30303"
    },
    protocols: {
      abey: {
        difficulty: null,
        head: "5794b768dae6c6ee5366e6ca7662bdff2882576e09609bf778633e470e0e7852",
        version: 63
      }
    }
}]
```

### admin_startRPC

The `startRPC` administrative method starts an HTTP based JSON RPC API webserver to handle client requests. All the parameters are optional:

- `host`: network interface to open the listener socket on (defaults to `"localhost"`)
- `port`: network port to open the listener socket on (defaults to `8545`)
- `cors`: cross-origin resource sharing header to use (defaults to `""`)
- `apis`: API modules to offer over this interface (defaults to `"abey,net,web3"`)

The method returns a boolean flag specifying whether the HTTP RPC listener was opened or not. Please note, only one HTTP endpoint is allowed to be active at any time.

| Client  | Method invocation                        |
| ------- | ---------------------------------------- |
| Go      | `admin.StartRPC(host *string, port *rpc.HexNumber, cors *string, apis *string) (bool, error)` |
| Console | `admin.startRPC(host, port, cors, apis)` |
| RPC     | `{"method": "admin_startRPC", "params": [host, port, cors, apis]}` |

#### Example

```
> admin.startRPC("127.0.0.1", 8545)
true
```

### admin_startWS

The `startWS` administrative method starts an WebSocket based  JSON RPC  API webserver to handle client requests. All the parameters are optional:

- `host`: network interface to open the listener socket on (defaults to `"localhost"`)
- `port`: network port to open the listener socket on (defaults to `8546`)
- `cors`: cross-origin resource sharing header to use (defaults to `""`)
- `apis`: API modules to offer over this interface (defaults to `"abey,net,web3"`)

The method returns a boolean flag specifying whether the WebSocket RPC listener was opened or not. Please note, only one WebSocket endpoint is allowed to be active at any time.

| Client  | Method invocation                        |
| ------- | ---------------------------------------- |
| Go      | `admin.StartWS(host *string, port *rpc.HexNumber, cors *string, apis *string) (bool, error)` |
| Console | `admin.startWS(host, port, cors, apis)`  |
| RPC     | `{"method": "admin_startWS", "params": [host, port, cors, apis]}` |

#### Example

```
> admin.startWS("127.0.0.1", 8546)
true
```

### admin_stopRPC

The `stopRPC` administrative method closes the currently open HTTP RPC endpoint. As the node can only have a single HTTP endpoint running, this method takes no parameters, returning a boolean whether the endpoint was closed or not.

| Client  | Method invocation               |
| ------- | ------------------------------- |
| Go      | `admin.StopRPC() (bool, error`) |
| Console | `admin.stopRPC()`               |
| RPC     | `{"method": "admin_stopRPC"`    |

#### Example

```
> admin.stopRPC()
true
```

### admin_stopWS

The `stopWS` administrative method closes the currently open WebSocket RPC endpoint. As the node can only have a single WebSocket endpoint running, this method takes no parameters, returning a boolean whether the endpoint was closed or not.

| Client  | Method invocation              |
| ------- | ------------------------------ |
| Go      | `admin.StopWS() (bool, error`) |
| Console | `admin.stopWS()`               |
| RPC     | `{"method": "admin_stopWS"`    |

#### Example

```
> admin.stopWS()
true
```

## Miner

The `miner` API allows you to remote control the node's mining operation and set various mining specific settings.

### miner_setExtra

Sets the extra data string that is included when this miner mines a block.

| Client  | Method invocation                        |
| ------- | ---------------------------------------- |
| Go      | `miner.setExtra(extra string) (bool, error)` |
| Console | `miner.setExtra(string)`                 |
| RPC     | `{"method": "miner_setExtra", "params": [string]}` |

#### Example

```
> miner.setExtra("it's my block")
true
```

### miner_setGasPrice

Sets the minimal accepted gas price when mining transactions. Any transactions that are below this limit are excluded from the mining process.

| Client  | Method invocation                        |
| ------- | ---------------------------------------- |
| Go      | `miner.setGasPrice(number *rpc.HexNumber) bool` |
| Console | `miner.setGasPrice(number)`              |
| RPC     | `{"method": "miner_setGasPrice", "params": [number]}` |

#### Example

```
> miner.setGasPrice(10000000)
true
```

### miner_start

Start the CPU mining process with the given number of threads and generate a new DAG if need be.

| Client  | Method invocation                        |
| ------- | ---------------------------------------- |
| Go      | `miner.Start(threads *rpc.HexNumber) (bool, error)` |
| Console | `miner.start(number)`                    |
| RPC     | `{"method": "miner_start", "params": [number]}` |

#### Example

```
> miner.start(8)
null
```

### miner_stop

Stop the CPU mining operation.

| Client  | Method invocation                        |
| ------- | ---------------------------------------- |
| Go      | `miner.Stop() bool`                      |
| Console | `miner.stop()`                           |
| RPC     | `{"method": "miner_stop", "params": []}` |

#### Example

```
> miner.stop()
true
```

### miner_setCoinbase

Sets the coinbase, where mining rewards will go.

| Client  | Method invocation                        |
| ------- | ---------------------------------------- |
| Go      | `miner.Setcoinbase(common.Address) bool` |
| Console | `miner.setCoinbase(address)`             |
| RPC     | `{"method": "miner_setcoinbase", "params": [address]}` |

#### Example

```
> miner.setCoinbase("0xbe7e39fa0645f6eaf501ad02318a7fda5c4df6c6")
true
```

### miner_setElection

Sets the election,To vote or not to vote

| Client  | Method invocation                        |
| ------- | ---------------------------------------- |
| Go      | `miner.SetElection(toElect bool, pubkey []byte) (bool, error)` |
| Console | `miner.setElection(toElect，pubkey)`      |
| RPC     | `{"method": "miner_setElection", "params": [toElect,pubkey]}` |

#### Example

```
> miner.setElection(true,"0x04044308742b61976de7344edb8662d6d10be1c477dd46e8e4c433c1288442a79183480894107299ff7b0706490f1fb9c9b7c9e62ae62d57bd84a1e469460d8ac1")
true
```

## Personal

### personal_listAccounts

Returns all the abeychain account addresses of all keys in the key store.

| Client  | Method invocation                        |
| ------- | ---------------------------------------- |
| Console | `personal.listAccounts`                  |
| RPC     | `{"method": "personal_listAccounts", "params": []}` |

#### Example

```
> personal.listAccounts
["0xbe7e39fa0645f6eaf501ad02318a7fda5c4df6c6", "0xaf1dd3a5135bcdd38079927a80c75ad05cfd140b", "0x4a98debf425a2fd4f56ef6674c72c3606b615e75"]
```

### personal_lockAccount

Removes the private key with given address from memory. The account can no longer be used to send transactions.

| Client  | Method invocation                        |
| ------- | ---------------------------------------- |
| Console | `personal.lockAccount(address)`          |
| RPC     | `{"method": "personal_lockAccount", "params": [string]}` |

#### Example

```
> personal.lockAccount("0xbe7e39fa0645f6eaf501ad02318a7fda5c4df6c6")
true
```

### personal_newAccount

Generates a new private key and stores it in the key store directory. The key file is encrypted with the given passphrase. Returns the address of the new account.

At the gabey console, `newAccount` will prompt for a passphrase when it is not supplied as the argument.

| Client  | Method invocation                        |
| ------- | ---------------------------------------- |
| Console | `personal.newAccount()`                  |
| RPC     | `{"method": "personal_newAccount", "params": [string]}` |

#### Example

```
> personal.newAccount()
Passphrase: 
Repeat passphrase: 
"0x5e97870f263700f46aa00d967821199b9bc5a120"
```

The passphrase can also be supplied as a string.

```
> personal.newAccount("h4ck3r")
"0x3d80b31a78c30fc628f20b2c89d7ddbf6e53cedc"
```

### personal_unlockAccount

Decrypts the key with the given address from the key store.

Both passphrase and unlock duration are optional when using the JavaScript console. If the passphrase is not supplied as an argument, the console will prompt for the passphrase interactively.

The unencrypted key will be held in memory until the unlock duration expires. If the unlock duration defaults to 300 seconds. An explicit duration of zero seconds unlocks the key until gabey exits.

The account can be used with `abey_sign` and `abey_sendTransaction` while it is unlocked.

| Client  | Method invocation                        |
| ------- | ---------------------------------------- |
| Console | `personal.unlockAccount(address, passphrase, duration)` |
| RPC     | `{"method": "personal_unlockAccount", "params": [string, string, number]}` |

#### Examples

```
> personal.unlockAccount("0x5e97870f263700f46aa00d967821199b9bc5a120")
Unlock account 0x5e97870f263700f46aa00d967821199b9bc5a120
Passphrase: 
true
```

Supplying the passphrase and unlock duration as arguments:

```
> personal.unlockAccount("0x5e97870f263700f46aa00d967821199b9bc5a120", "foo", 30)
true
```

If you want to type in the passphrase and stil override the default unlock duration, pass `null` as the passphrase.

```
> personal.unlockAccount("0x5e97870f263700f46aa00d967821199b9bc5a120", null, 30)
Unlock account 0x5e97870f263700f46aa00d967821199b9bc5a120
Passphrase: 
true
```

### personal_sendTransaction

Validate the given passphrase and submit transaction.

The transaction is the same argument as for `abey_sendTransaction` and contains the `from`address. If the passphrase can be used to decrypt the private key belogging to `tx.from` the transaction is verified, signed and send onto the network. The account is not unlocked globally in the node and cannot be used in other RPC calls.

| Client  | Method invocation                        |
| ------- | ---------------------------------------- |
| Console | `personal.sendTransaction(tx, passphrase)` |
| RPC     | `{"method": "personal_sendTransaction", "params": [tx, string]}` |

#### Examples

```
> var tx = {from: "0x391694e7e0b0cce554cb130d723a9d27458f9298", to: "0xafa3f8684e54059998bc3a7b0d2b0da075154d66", value: web3.toWei(1.23, "abey")}
undefined
> personal.sendTransaction(tx, "passphrase")
0x8474441674cdd47b35b875fd1a530b800b51a5264b9975fb21129eeb8c18582f
```

### personal_sign

The sign method calculates an abeychain specific signature with:`sign(keccack256("\x19abeychain Signed Message:\n" + len(message) + message)))`.

By adding a prefix to the message makes the calculated signature recognisable as an abeychain specific signature. This prevents misuse where a malicious DApp can sign arbitrary data (e.g. transaction) and use the signature to impersonate the victim.

See ecRecover to verify the signature.

| Client  | Method invocation                        |
| ------- | ---------------------------------------- |
| Console | `personal.sign(message, account, [password])` |
| RPC     | `{"method": "personal_sign", "params": [message, account, password]}` |

#### Examples

```
> personal.sign("0xdeadbeaf", "0xbe7e39fa0645f6eaf501ad02318a7fda5c4df6c6", "")
"0x0d7d2150b38dc30ba0aa8db9171abb1bd31b6cf39f4edf2b93de0a2b6ba05a81034aa81b462fb5b8b09c74dba29172c9ad098607d7898a3aa8412b40ef2686841b"
```

### personal_ecRecover

`ecRecover` returns the address associated with the private key that was used to calculate the signature in `personal_sign`.

| Client  | Method invocation                        |
| ------- | ---------------------------------------- |
| Console | `personal.ecRecover(message, signature)` |
| RPC     | `{"method": "personal_ecRecover", "params": [message, signature]}` |

#### Examples

```
> personal.sign("0xdeadbeaf", "0xbe7e39fa0645f6eaf501ad02318a7fda5c4df6c6", "")
"0x0d7d2150b38dc30ba0aa8db9171abb1bd31b6cf39f4edf2b93de0a2b6ba05a81034aa81b462fb5b8b09c74dba29172c9ad098607d7898a3aa8412b40ef2686841b"
> personal.ecRecover("0xdeadbeaf", "0x0d7d2150b38dc30ba0aa8db9171abb1bd31b6cf39f4edf2b93de0a2b6ba05a81034aa81b462fb5b8b09c74dba29172c9ad098607d7898a3aa8412b40ef2686841b")
"0xbe7e39fa0645f6eaf501ad02318a7fda5c4df6c6"
```

## Txpool

The `txpool` API gives you access to several non-standard RPC methods to inspect the contents of the transaction pool containing all the currently pending transactions as well as the ones queued for future processing.

### txpool_content

Content returns the transactions contained within the transaction pool.

| Client  | Method invocation                        |
| ------- | ---------------------------------------- |
| Go      | `txpool.Content() (map[string]map[string]map[string][]*RPCTransaction)` |
| Console | `txpool.content`                         |
| RPC     | `{"method": "txpool_content"}`           |

#### Example

```
> txpool.content
{
  pending: {
    0xbE7e39Fa0645F6eaf501Ad02318A7fDa5c4DF6C6: {
      25421: {
        blockHash: "0x0000000000000000000000000000000000000000000000000000000000000000",
        blockNumber: null,
        from: "0xbe7e39fa0645f6eaf501ad02318a7fda5c4df6c6",
        gas: "0x15f90",
        gasPrice: "0x430e23400",
        hash: "0xf6d3aefa3bad795a58fbf7afcc82f0cccb4419828c8f4dde5df5c9c2f494affb",
        input: "0x",
        nonce: "0x634d",
        r: "0xf70109404e86563118a585eaf7aa7643549d9bb0e903b7761dac36a1aacb27a3",
        s: "0x74d4091f7c18ab01d8c647090d0e77766578f19690390379fdc86e3fd4e0d8ee",
        to: "0x3216e8c10ad02ebb828aaf3b2dbc47fdbe24d336",
        transactionIndex: "0x0",
        v: "0x37",
        value: "0x2100"
      },
      25465: {
        blockHash: "0x0000000000000000000000000000000000000000000000000000000000000000",
        blockNumber: null,
        from: "0xbe7e39fa0645f6eaf501ad02318a7fda5c4df6c6",
        gas: "0x15f90",
        gasPrice: "0x430e23400",
        hash: "0x80bc09cd8414a6a9b139860709fedbb0a1714ff72c99ca02f3a28b2c3f387f02",
        input: "0x",
        nonce: "0x6379",
        r: "0x78f88d00d391c5087ad8d7d96451545c3e5ad4edb675bb1162f2bb0953b3c53f",
        s: "0x20234bc4790b06863f9d277d5afe4967fc84b9959715fb8b669434a23ac4182a",
        to: "0x95e5478957995d52da5f5e01cf907bbd3a739c51",
        transactionIndex: "0x0",
        v: "0x38",
        value: "0x2100"
      }
    }
  },
  queued: {}
}
```

### txpool_inspect

The `inspect` inspection property can be queried to list a textual summary of all the transactions currently pending for inclusion in the next block(s), as well as the ones that are being scheduled for future execution only. This is a method specifically tailored to developers to quickly see the transactions in the pool and find any potential issues.

The result is an object with two fields `pending` and `queued`. Each of these fields are associative arrays, in which each entry maps an origin-address to a batch of scheduled transactions. These batches themselves are maps associating nonces with transactions summary strings.

Please note, there may be multiple transactions associated with the same account and nonce. This can happen if the user broadcast mutliple ones with varying gas allowances (or even complerely different transactions).

| Client  | Method invocation                        |
| ------- | ---------------------------------------- |
| Go      | `txpool.Inspect() (map[string]map[string]map[string][]string)` |
| Console | `txpool.inspect`                         |
| RPC     | `{"method": "txpool_inspect"}`           |

#### Example

```
> txpool.inspect
{
  pending: {},
  queued: {}
}
```

### txpool_status

The `status` inspection property can be queried for the number of transactions currently pending for inclusion in the next block(s), as well as the ones that are being scheduled for future execution only.

The result is an object with two fields `pending` and `queued`, each of which is a counter representing the number of transactions in that particular state.

| Client  | Method invocation                        |
| ------- | ---------------------------------------- |
| Go      | `txpool.Status() (map[string]*rpc.HexNumber)` |
| Console | `txpool.status`                          |
| RPC     | `{"method": "txpool_status"}`            |

#### Example

```
> txpool.status
{
  pending: 100,
  queued: 0
}
```

## Fruitpool

### fruitpool_content

Content returns the pendingFruits contained within the snail pool.

| Client  | Method invocation                   |
| ------- | ----------------------------------- |
| Go      | `fruitpool.Content() ([]*RPCFruit)` |
| Console | `fruitpool.content`                 |
| RPC     | `{"method": "fruitpool_content"}`   |

#### Example

```
> fruitpool.content

[{
    fastHash: "0xbf847397758ea1280a82c667c1cdda7604d12a15f062a3d82e4b8e3b7f372af8",
    fastNumber: 126,
    fruitDifficulty: 2000,
    fruitHash: "0xbc839c8efd9117a565ba16c0cee9644c83304f7c333be1d28bce282a3a61ae9d",
    number: 2,
    pointerHash: "0x5b445e49ca5b67eeca43a46420d6400a9ba90a3a3520927114d7d734683fa0e2",
    pointerNumber: 0,
    signHash: "0xca046c8d3dfea13c4c5b8224ce3bc2fc4230ea12135483a15de7d11157d40805"
}, {
    fastHash: "0x2558e2ef0d0df2ff11353ecc85fb4ae6982da71f464a0ecbf59ec297443aa259",
    fastNumber: 127,
    fruitDifficulty: 2000,
    fruitHash: "0x608b5237ec8a572c8168ab050981ca840271447a6cae8ac2f9693e7e6680abfc",
    number: 3,
    pointerHash: "0x5b445e49ca5b67eeca43a46420d6400a9ba90a3a3520927114d7d734683fa0e2",
    pointerNumber: 0,
    signHash: "0xe6bd08a4475a566d7e0659697563f7e65d3e6ea633e6908a8f6aa94bdba3632f"
}, {
    fastHash: "0xea22558ecc9c92591f6379bc766fec8af55a0641b22508bf5431d5d499b794ce",
    fastNumber: 128,
    fruitDifficulty: 2000,
    fruitHash: "0xf5e45e78c58061c327782f49fee6cdbf8874d4e20d7082f99cbbb32f06a46bb9",
    number: 3,
    pointerHash: "0x5b445e49ca5b67eeca43a46420d6400a9ba90a3a3520927114d7d734683fa0e2",
    pointerNumber: 0,
    signHash: "0x18e13d804ac9966c48e4d7e70e6ff2d16ac53b0d23d644a54fc3201566ae0857"
}]
```

### fruitpool_inspect

Inspect returns the unVerifiedFruits contained within the snail pool.

| Client  | Method invocation                   |
| ------- | ----------------------------------- |
| Go      | `fruitpool.Inspect() ([]*RPCFruit)` |
| Console | `fruitpool.inspect`                 |
| RPC     | `{"method": "fruitpool_inspect"}`   |

#### Example

```
> fruitpool.inspect
[{
    fastHash: "0xa71cfac04c172f29b6c300758df80d2a238f62918616cfe951dbcbb564e075cd",
    fastNumber: 153632,
    fruitDifficulty: 100,
    fruitHash: "0x0f23c22f5446c696d142a8e582dd255ee622d2f27894305f8fd4ef6ea281c5ee",
    number: 2480,
    pointerHash: "0x0582995b001c00afa5446b6a410b177685fbabb89f69430b5e6a4d9953041377",
    pointerNumber: 2472,
    signHash: "0x7622c3e1c58f83c80565779a5eeeb4d710bd1533d28c130718c8ecb470649856"
}, {
    fastHash: "0xde0b0918bb97c9314ae511d141d05b2bf9ba8727c2a9c9b0989acbecb4e2bdce",
    fastNumber: 153633,
    fruitDifficulty: 100,
    fruitHash: "0xed8890352230a6a9524e090628899a315f7be70f06849d031a48c47cd615b0a9",
    number: 2480,
    pointerHash: "0x0582995b001c00afa5446b6a410b177685fbabb89f69430b5e6a4d9953041377",
    pointerNumber: 2472,
    signHash: "0x54885fc9f07162ecc23e9656f75977e474b41384294b0cc1cfe222a2b1eedede"
}, {
    fastHash: "0x9b5d0dd3b3d1b33daf29d007b7f7525f60e4c254979fc8b4ed81e585390eebc7",
    fastNumber: 155138,
    fruitDifficulty: 100,
    fruitHash: "0xbf25a17ec04585626a6da4ebe6d12dfb26120d99d523bd5a7c199ac6b5fd9af5",
    number: 2504,
    pointerHash: "0xf99a75e82d1ecdb5f55841276aa4708b7425ec9ed93681f20c03392b36b3497b",
    pointerNumber: 2496,
    signHash: "0x4030b0467603334a57bf6b7e76b042dcfb1c06376a214c1c299cb053e48a85c6"
}]
```

### fruitpool_status

Status returns the number of pending and unVerified Fruits in the pool.

| Client  | Method invocation                        |
| ------- | ---------------------------------------- |
| Go      | `fruitpool.Status() (map[string]hexutil.Uint)` |
| Console | `fruitpool.status`                       |
| RPC     | `{"method": "fruitpool_status"}`         |

#### Example

```
> fruitpool.status
{
  pending: "0x3f",
  unverified: "0x69a"
}
```

## Debug

The `debug` API gives you access to several non-standard RPC methods, which will allow you to inspect, debug and set certain debugging flags during runtime.

### debug_backtraceAt

Sets the logging backtrace location. When a backtrace location is set and a log message is emitted at that location, the stack of the goroutine executing the log statement will be printed to stderr.

The location is specified as `<filename>:<line>`.

| Client  | Method invocation                        |
| ------- | ---------------------------------------- |
| Console | `debug.backtraceAt(string)`              |
| RPC     | `{"method": "debug_backtraceAt", "params": [string]}` |

Example:

```
> debug.backtraceAt("server.go:443")
```

### debug_blockProfile

Turns on block profiling for the given duration and writes profile data to disk. It uses a profile rate of 1 for most accurate information. If a different rate is desired, set the rate and write the profile manually using `debug_writeBlockProfile`.

| Client  | Method invocation                        |
| ------- | ---------------------------------------- |
| Console | `debug.blockProfile(file, seconds)`      |
| RPC     | `{"method": "debug_blockProfile", "params": [string, number]}` |

### debug_cpuProfile

Turns on CPU profiling for the given duration and writes profile data to disk.

| Client  | Method invocation                        |
| ------- | ---------------------------------------- |
| Console | `debug.cpuProfile(file, seconds)`        |
| RPC     | `{"method": "debug_cpuProfile", "params": [string, number]}` |

### debug_dumpBlock

Retrieves the state that corresponds to the block number and returns a list of accounts (including storage and code).

| Client  | Method invocation                        |
| ------- | ---------------------------------------- |
| Go      | `debug.DumpBlock(number uint64) (state.World, error)` |
| Console | `debug.traceBlockByHash(number, [options])` |
| RPC     | `{"method": "debug_dumpBlock", "params": [number]}` |

#### Example

```
> debug.dumpBlock("0xa")
{
    fff7ac99c8e4feb60c9750054bdc14ce1857f181: {
      balance: "49358640978154672",
      code: "",
      codeHash: "c5d2460186f7233c927e7db2dcc703c0e500b653ca82273b7bfad8045d85a470",
      nonce: 2,
      root: "56e81f171bcc55a6ff8345e692c0f86e5b48e01b996cadc001622fb5e363b421",
      storage: {}
    },
    fffbca3a38c3c5fcb3adbb8e63c04c3e629aafce: {
      balance: "3460945928",
      code: "",
      codeHash: "c5d2460186f7233c927e7db2dcc703c0e500b653ca82273b7bfad8045d85a470",
      nonce: 657,
      root: "56e81f171bcc55a6ff8345e692c0f86e5b48e01b996cadc001622fb5e363b421",
      storage: {}
    }
  },
  root: "19f4ed94e188dd9c7eb04226bd240fa6b449401a6c656d6d2816a87ccaf206f1"
}
```

### debug_gcStats

Returns GC statistics.

See <https://golang.org/pkg/runtime/debug/#GCStats> for information about the fields of the returned object.

| Client  | Method invocation                        |
| ------- | ---------------------------------------- |
| Console | `debug.gcStats()`                        |
| RPC     | `{"method": "debug_gcStats", "params": []}` |

### debug_getBlockRlp

Retrieves and returns the RLP encoded block by number.

| Client  | Method invocation                        |
| ------- | ---------------------------------------- |
| Go      | `debug.GetBlockRlp(number uint64) (string, error)` |
| Console | `debug.getBlockRlp(number, [options])`   |
| RPC     | `{"method": "debug_getBlockRlp", "params": [number]}` |

### debug_goTrace

Turns on Go runtime tracing for the given duration and writes trace data to disk.

| Client  | Method invocation                        |
| ------- | ---------------------------------------- |
| Console | `debug.goTrace(file, seconds)`           |
| RPC     | `{"method": "debug_goTrace", "params": [string, number]}` |

### debug_memStats

Returns detailed runtime memory statistics.

See <https://golang.org/pkg/runtime/#MemStats> for information about the fields of the returned object.

| Client  | Method invocation                        |
| ------- | ---------------------------------------- |
| Console | `debug.memStats()`                       |
| RPC     | `{"method": "debug_memStats", "params": []}` |

### debug_seedHash

Fetches and retrieves the seed hash of the block by number

| Client  | Method invocation                        |
| ------- | ---------------------------------------- |
| Go      | `debug.SeedHash(number uint64) (string, error)` |
| Console | `debug.seedHash(number, [options])`      |
| RPC     | `{"method": "debug_seedHash", "params": [number]}` |

### debug_setHead

Sets the current head of the local snail chain by snailBlock number. **Note**, this is a destructive action and may severely damage your snail chain. Use with *extreme* caution.

| Client  | Method invocation                        |
| ------- | ---------------------------------------- |
| Go      | `debug.SetHead(number uint64)`      |
| Console | `debug.setHead(number)`             |
| RPC     | `{"method": "debug_setHead", "params": [number]}` |

warn: it may cause the console quit and may destory the database,just for debug and test.

### debug_setBlockProfileRate

Sets the rate (in samples/sec) of goroutine block profile data collection. A non-zero rate enables block profiling, setting it to zero stops the profile. Collected profile data can be written using `debug_writeBlockProfile`.

| Client  | Method invocation                        |
| ------- | ---------------------------------------- |
| Console | `debug.setBlockProfileRate(rate)`        |
| RPC     | `{"method": "debug_setBlockProfileRate", "params": [number]}` |

### debug_stacks

Returns a printed representation of the stacks of all goroutines. Note that the web3 wrapper for this method takes care of the printing and does not return the string.

| Client  | Method invocation                        |
| ------- | ---------------------------------------- |
| Console | `debug.stacks()`                         |
| RPC     | `{"method": "debug_stacks", "params": []}` |

### debug_startCPUProfile

Turns on CPU profiling indefinitely, writing to the given file.

| Client  | Method invocation                        |
| ------- | ---------------------------------------- |
| Console | `debug.startCPUProfile(file)`            |
| RPC     | `{"method": "debug_startCPUProfile", "params": [string]}` |

### debug_startGoTrace

Starts writing a Go runtime trace to the given file.

| Client  | Method invocation                        |
| ------- | ---------------------------------------- |
| Console | `debug.startGoTrace(file)`               |
| RPC     | `{"method": "debug_startGoTrace", "params": [string]}` |

### debug_stopCPUProfile

Stops an ongoing CPU profile.

| Client  | Method invocation                        |
| ------- | ---------------------------------------- |
| Console | `debug.stopCPUProfile()`                 |
| RPC     | `{"method": "debug_stopCPUProfile", "params": []}` |

### debug_stopGoTrace

Stops writing the Go runtime trace.

| Client  | Method invocation                        |
| ------- | ---------------------------------------- |
| Console | `debug.stopGoTrace()`               |
| RPC     | `{"method": "debug_stopGoTrace", "params": []}` |

### debug_traceBlock

The `traceBlock` method will return a full stack trace of all invoked opcodes of all transaction that were included included in this block. **Note**, the parent of this block must be present or it will fail.

| Client  | Method invocation                        |
| ------- | ---------------------------------------- |
| Go      | `debug.TraceBlock(blockRlp []byte, config. *vm.Config) BlockTraceResult` |
| Console | `debug.traceBlock(tblockRlp, [options])` |
| RPC     | `{"method": "debug_traceBlock", "params": [blockRlp, {}]}` |

#### Example

```
> debug.traceBlock("0xblock_rlp")
{
  gas: 85301,
  returnValue: "",
  structLogs: [{
      depth: 1,
      error: "",
      gas: 162106,
      gasCost: 3,
      memory: null,
      op: "PUSH1",
      pc: 0,
      stack: [],
      storage: {}
  },
    /* snip */
  {
      depth: 1,
      error: "",
      gas: 100000,
      gasCost: 0,
      memory: ["0000000000000000000000000000000000000000000000000000000000000006", "0000000000000000000000000000000000000000000000000000000000000000", "0000000000000000000000000000000000000000000000000000000000000060"],
      op: "STOP",
      pc: 120,
      stack: ["00000000000000000000000000000000000000000000000000000000d67cbec9"],
      storage: {
        0000000000000000000000000000000000000000000000000000000000000004: "8241fa522772837f0d05511f20caa6da1d5a3209000000000000000400000001",
        0000000000000000000000000000000000000000000000000000000000000006: "0000000000000000000000000000000000000000000000000000000000000001",
        f652222313e28459528d920b65115c16c04f3efc82aaedc97be59f3f377c0d3f: "00000000000000000000000002e816afc1b5c0f39852131959d946eb3b07b5ad"
      }
  }]
```

### debug_traceBlockByNumber

Similar to debug_traceBlock， `traceBlockByNumber` accepts a block number and will replay the block that is already present in the database.

| Client  | Method invocation                        |
| ------- | ---------------------------------------- |
| Go      | `debug.TraceBlockByNumber(number uint64, config. *vm.Config) BlockTraceResult` |
| Console | `debug.traceBlockByNumber(number, [options])` |
| RPC     | `{"method": "debug_traceBlockByNumber", "params": [number, {}]}` |

### debug_traceBlockByHash

Similar to debug_traceBlock, `traceBlockByHash` accepts a block hash and will replay the block that is already present in the database.

| Client  | Method invocation                        |
| ------- | ---------------------------------------- |
| Go      | `debug.TraceBlockByHash(hash common.Hash, config. *vm.Config) BlockTraceResult` |
| Console | `debug.traceBlockByHash(hash, [options])` |
| RPC     | `{"method": "debug_traceBlockByHash", "params": [hash {}]}` |

### debug_traceBlockFromFile

Similar to debug_traceBlock, `traceBlockFromFile` accepts a file containing the RLP of the block.

| Client  | Method invocation                        |
| ------- | ---------------------------------------- |
| Go      | `debug.TraceBlockFromFile(fileName string, config. *vm.Config) BlockTraceResult` |
| Console | `debug.traceBlockFromFile(fileName, [options])` |
| RPC     | `{"method": "debug_traceBlockFromFile", "params": [fileName, {}]}` |

### debug_traceTransaction

The `traceTransaction` debugging method will attempt to run the transaction in the exact same manner as it was executed on the network. It will replay any transaction that may have been executed prior to this one before it will finally attempt to execute the transaction that corresponds to the given hash.

In addition to the hash of the transaction you may give it a secondary *optional* argument, which specifies the options for this specific call. The possible options are:

- `disableStorage`: `BOOL`. Setting this to true will disable storage capture (default = false).
- `disableMemory`: `BOOL`. Setting this to true will disable memory capture (default = false).
- `disableStack`: `BOOL`. Setting this to true will disable stack capture (default = false).
- `tracer`: `STRING`. Setting this will enable JavaScript-based transaction tracing, described below. If set, the previous four arguments will be ignored.
- `timeout`: `STRING`. Overrides the default timeout of 5 seconds for JavaScript-based tracing calls. Valid values are described [here](https://golang.org/pkg/time/#ParseDuration).

| Client  | Method invocation                        |
| ------- | ---------------------------------------- |
| Go      | `debug.TraceTransaction(txHash common.Hash, logger *vm.LogConfig) (*ExecutionResurt, error)` |
| Console | `debug.traceTransaction(txHash, [options])` |
| RPC     | `{"method": "debug_traceTransaction", "params": [txHash, {}]}` |

#### Example

```
> debug.traceTransaction("0x2059dd53ecac9827faad14d364f9e04b1d5fe5b506e3acc886eff7a6f88a696a")
{
  gas: 85301,
  returnValue: "",
  structLogs: [{
      depth: 1,
      error: "",
      gas: 162106,
      gasCost: 3,
      memory: null,
      op: "PUSH1",
      pc: 0,
      stack: [],
      storage: {}
  },
    /* snip */
  {
      depth: 1,
      error: "",
      gas: 100000,
      gasCost: 0,
      memory: ["0000000000000000000000000000000000000000000000000000000000000006", "0000000000000000000000000000000000000000000000000000000000000000", "0000000000000000000000000000000000000000000000000000000000000060"],
      op: "STOP",
      pc: 120,
      stack: ["00000000000000000000000000000000000000000000000000000000d67cbec9"],
      storage: {
        0000000000000000000000000000000000000000000000000000000000000004: "8241fa522772837f0d05511f20caa6da1d5a3209000000000000000400000001",
        0000000000000000000000000000000000000000000000000000000000000006: "0000000000000000000000000000000000000000000000000000000000000001",
        f652222313e28459528d920b65115c16c04f3efc82aaedc97be59f3f377c0d3f: "00000000000000000000000002e816afc1b5c0f39852131959d946eb3b07b5ad"
      }
  }]
```

#### JavaScript-based tracing

Specifying the `tracer` option in the second argument enables JavaScript-based tracing. In this mode, `tracer` is interpreted as a JavaScript expression that is expected to evaluate to an object with (at least) two methods, named `step` and `result`.

`step`is a function that takes two arguments, log and db, and is called for each step of the EVM, or when an error occurs, as the specified transaction is traced.

`log` has the following fields:

- `pc`: Number, the current program counter
- `op`: Object, an OpCode object representing the current opcode
- `gas`: Number, the amount of gas remaining
- `gasPrice`: Number, the cost in wei of each unit of gas
- `memory`: Object, a structure representing the contract's memory space
- `stack`: array[big.Int], the EVM execution stack
- `depth`: The execution depth
- `account`: The address of the account executing the current operation
- `err`: If an error occured, information about the error

If `err` is non-null, all other fields should be ignored.

For efficiency, the same `log` object is reused on each execution step, updated with current values; make sure to copy values you want to preserve beyond the current call. For instance, this step function will not work:

```
function(log) {
  this.logs.append(log);
}
```

But this step function will:

```
function(log) {
  this.logs.append({gas: log.gas, pc: log.pc, ...});
}
```

`log.op` has the following methods:

- `isPush()` - returns true if the opcode is a PUSHn
- `toString()` - returns the string representation of the opcode
- `toNumber()` - returns the opcode's number

`log.memory` has the following methods:

- `slice(start, stop)` - returns the specified segment of memory as a byte slice
- `length()` - returns the length of the memory

`log.stack` has the following methods:

- `peek(idx)` - returns the idx-th element from the top of the stack (0 is the topmost element) as a big.Int
- `length()` - returns the number of elements in the stack

`db` has the following methods:

- `getBalance(address)` - returns a `big.Int` with the specified account's balance
- `getNonce(address)` - returns a Number with the specified account's nonce
- `getCode(address)` - returns a byte slice with the code for the specified account
- `getState(address, hash)` - returns the state value for the specified account and the specified hash
- `exists(address)` - returns true if the specified address exists

The second function, 'result', takes no arguments, and is expected to return a JSON-serializable value to return to the RPC caller.

If the step function throws an exception or executes an illegal operation at any point, it will not be called on any further VM steps, and the error will be returned to the caller.

Note that several values are Golang big.Int objects, not JavaScript numbers or JS bigints. As such, they have the same interface as described in the godocs. Their default serialization to JSON is as a Javascript number; to serialize large numbers accurately call `.String()` on them. For convenience, `big.NewInt(x)` is provided, and will convert a uint to a Go BigInt.

Usage example, returns the top element of the stack at each CALL opcode only:

```
debug.traceTransaction(txhash, {tracer: '{data: [], step: function(log) { if(log.op.toString() == "CALL") this.data.push(log.stack.peek(0)); }, result: function() { return this.data; }}'});
```

### debug_verbosity

Sets the logging verbosity ceiling. Log messages with level up to and including the given level will be printed.

The verbosity of individual packages and source files can be raised using `debug_vmodule`.

| Client  | Method invocation                        |
| ------- | ---------------------------------------- |
| Console | `debug.verbosity(level)`                 |
| RPC     | `{"method": "debug_vmodule", "params": [number]}` |

### debug_vmodule

Sets the logging verbosity pattern.

| Client  | Method invocation                        |
| ------- | ---------------------------------------- |
| Console | `debug.vmodule(string)`                  |
| RPC     | `{"method": "debug_vmodule", "params": [string]}` |

#### Examples

If you want to see messages from a particular Go package (directory) and all subdirectories, use:

```
> debug.vmodule("abey/*=6")
```

If you want to restrict messages to a particular package (e.g. p2p) but exclude subdirectories, use:

```
> debug.vmodule("p2p=6")
```

If you want to see log messages from a particular source file, use

```
> debug.vmodule("server.go=6")
```

You can compose these basic patterns. If you want to see all output from peer.go in a package below abey (abey/peer.go, abey/downloader/peer.go) as well as output from package p2p at level <= 5, use:

```
debug.vmodule("abey/*/peer.go=6,p2p=5")
```

### debug_writeBlockProfile

Writes a goroutine blocking profile to the given file.

| Client  | Method invocation                        |
| ------- | ---------------------------------------- |
| Console | `debug.writeBlockProfile(file)`          |
| RPC     | `{"method": "debug_writeBlockProfile", "params": [string]}` |

### debug_writeMemProfile

Writes an allocation profile to the given file. Note that the profiling rate cannot be set through the API, it must be set on the command line using the `--memprofilerate` flag.

| Client  | Method invocation                        |
| ------- | ---------------------------------------- |
| Console | `debug.writeMemProfile(file string)`     |
| RPC     | `{"method": "debug_writeBlockProfile", "params": [string]}` |

## ABEY

### abey_protocolVersion

Returns the current abeychain protocol version.

| Client  | Method invocation                     |
| ------- | ------------------------------------- |
| Console | `abey.protocolVersion`               |
| RPC     | `{"method": "abey_protocolVersion"}` |

### abey_syncing

Returns an object with data about the sync status or `false`

| Client  | Method invocation                      |
| ------- | -------------------------------------- |
| Go      | `abey.syncing() (interface{}, error`) |
| Console | `abey.syncing`                        |
| RPC     | `{"method": "abey_syncing`"}`         |

#### Example

```
> abey.syncing
{
  currentBlock: 961,
  highestBlock: 7092,
  knownStates: 0,
  pulledStates: 0,
  startingBlock: 0
}
```

### abey_coinbase

Returns the client coinbase address.

| Client  | Method invocation              |
| ------- | ------------------------------ |
| Console | `abey.coinbase`               |
| RPC     | `{"method": "abey_coinbase"}` |

#### Example

```
> abey.coinbase
"0xbe7e39fa0645f6eaf501ad02318a7fda5c4df6c6"
```

### abey_mining

Returns `true` if client is actively mining new blocks.

| Client  | Method invocation            |
| ------- | ---------------------------- |
| Console | `abey.mining`               |
| RPC     | `{"method": "abey_mining"}` |

### abey_hashrate

Returns the number of hashes per second that the node is mining with.

| Client  | Method invocation              |
| ------- | ------------------------------ |
| Console | `abey.hashrate`               |
| RPC     | `{"method": "abey_hashrate"}` |

### abey_gasPrice

Returns the current price per gas in wei.

| Client  | Method invocation              |
| ------- | ------------------------------ |
| Console | `abey.gasPrice`               |
| RPC     | `{"method": "abey_gasPrice"}` |

#### Example

```
> abey.gasPrice
18000000000

```

### abey_accounts

Returns a list of addresses owned by client.

| Client  | Method invocation              |
| ------- | ------------------------------ |
| Console | `abey.accounts`               |
| RPC     | `{"method": "abey_accounts"}` |

#### Example

```
> abey.accounts
["0xbe7e39fa0645f6eaf501ad02318a7fda5c4df6c6", "0xaf1dd3a5135bcdd38079927a80c75ad05cfd140b", "0x4a98debf425a2fd4f56ef6674c72c3606b615e75"]

```

### abey_blockNumber

Returns the number of most recent block.

| Client  | Method invocation                 |
| ------- | --------------------------------- |
| Console | `abey.blockNumber`               |
| RPC     | `{"method": "abey_blockNumber"}` |

#### Example

```
> abey.blockNumber
82917

```

### abey_getBalance

Returns the balance of the account of given address.integer of the current balance in wei.

| Client  | Method invocation                                            |
| ------- | ------------------------------------------------------------ |
| Console | `abey.getBalance(address)`                                  |
| RPC     | `{"method": "abey_startCPUProfile", "params": [string, latest]}` |

#### Example

```
> abey.getBalance("0xbE7e39Fa0645F6eaf501Ad02318A7fDa5c4DF6C6")
10000000000000000000

```

### abey_getStorageAt

GetStorageAt returns the storage from the state at the given address, key and
block number. The rpc.LatestBlockNumber and rpc.PendingBlockNumber meta block
numbers are also allowed.

| Client  | Method invocation                                            |
| ------- | ------------------------------------------------------------ |
| Go      | `abey.getStorageAt(address,key,blockNr) (hexutil.Bytes, error) |
| Console | `abey.getStorageAt(address, key)`                           |
| RPC     | `{"method": "abey_getStorageAt", "params": [address, key, latest]}` |

#### Example

```
> abey.getStorageAt("0x295a70b2de5e3953354a6a8344e616ed314d7251","0x0","latest")
"0x0000000000000000000000000000000000000000000000000000000000000000"

```

### abey_getTransactionCount

GetTransactionCount returns the number of transactions the given address has sent for the given block number

| Client  | Method invocation                                            |
| ------- | ------------------------------------------------------------ |
| Console | `abey.getTransactionCount(address)`                         |
| RPC     | `{"method": "abey_getTransactionCount", "params": [address, latest]}` |

#### Example

```
> abey.getTransactionCount("0x295a70b2de5e3953354a6a8344e616ed314d7251")
12

```

### abey_getBlockTransactionCount

Returns the number of transactions in a block matching the given block number.

| Client  | Method invocation                                            |
| ------- | ------------------------------------------------------------ |
| Console | `abey.getBlockTransactionCount(number)`             |
| RPC     | `{"method": "abey_getBlockTransactionCountByNumber", "params": [string]}` |

#### Example

```
> abey.getBlockTransactionCount(12)
10
> abey.getBlockTransactionCount("0xb903239f8543d04b5dc1ba6579132b143087c68db1b2168786408fcbce568238")
10

```

### abey_getCode

Returns code at a given address.
Divided into ordinary account and contract account address
Ordinary account return "0x"
Where the contract account returns the binary code

| Client  | Method invocation                                          |
| ------- | ---------------------------------------------------------- |
| Console | `abey.getCode(address)`                                   |
| RPC     | `{"method": "abey_getCode", "params": [address, latest]}` |

#### Example

```
> abey.getCode("0xB824adf0Bad5E49bB214922F4499bD2cf08ef519")
"0x"

```

### abey_sign

The sign method calculates an abeychain specific signature with: sign(keccak256("\x19abeychain Signed Message:\n" + len(message) + message))).

By adding a prefix to the message makes the calculated signature recognisable as an abeychain specific signature. This prevents misuse where a malicious DApp can sign arbitrary data (e.g. transaction) and use the signature to impersonate the victim.

Note the address to sign with must be unlocked.

| Client  | Method invocation                                        |
| ------- | -------------------------------------------------------- |
| Console | `abey.sign(address,message)`                            |
| RPC     | `{"method": "abey_sign", "params": [address, message]}` |

#### Example

```
> personal.unlockAccount("0xbe7e39fa0645f6eaf501ad02318a7fda5c4df6c6")
Unlock account 0xbe7e39fa0645f6eaf501ad02318a7fda5c4df6c6
Passphrase:
true
> abey.sign("0xbe7e39fa0645f6eaf501ad02318a7fda5c4df6c6","0xdeadbeaf")
"0x0d7d2150b38dc30ba0aa8db9171abb1bd31b6cf39f4edf2b93de0a2b6ba05a81034aa81b462fb5b8b09c74dba29172c9ad098607d7898a3aa8412b40ef2686841b"

```

### abey_sendTransaction

Creates new message call transaction or a contract creation, if the data field contains code.

params:

Object - The transaction object
from: DATA, 20 Bytes - The address the transaction is send from.
to: DATA, 20 Bytes - (optional when creating new contract) The address the transaction is directed to.
gas: QUANTITY - (optional, default: 90000) Integer of the gas provided for the transaction execution. It will return unused gas.
gasPrice: QUANTITY - (optional, default: To-Be-Determined) Integer of the gasPrice used for each paid gas
value: QUANTITY - (optional) Integer of the value sent with this transaction
data: DATA - The compiled code of a contract OR the hash of the invoked method signature and encoded parameters. For details see abeychain Contract ABI
nonce: QUANTITY - (optional) Integer of a nonce. This allows to overwrite your own pending transactions that use the same nonce.

| Client  | Method invocation                                         |
| ------- | --------------------------------------------------------- |
| Console | `abey.sendTransaction(params)`                           |
| RPC     | `{"method": "abey_sendTransaction", "params": [params]}` |

#### Example

```
> personal.unlockAccount("0xbe7e39fa0645f6eaf501ad02318a7fda5c4df6c6")
Unlock account 0xbe7e39fa0645f6eaf501ad02318a7fda5c4df6c6
Passphrase:
true
> abey.sendTransaction({"from":"0xbe7e39fa0645f6eaf501ad02318a7fda5c4df6c6","to":"0xbe7e39fa0645f6eaf501ad02318a7fda5c4df6c6","value":"1000000000000000"})
"0xcf3d3adb9a6b875e3adb966bc436a963f689f9dbf2ea18a3955fed263d865dd1"

```

### abey_sendRawTransaction

SendRawTransaction will add the signed transaction to the transaction pool.
The sender is responsible for signing the transaction and using the correct nonce.

| Client  | Method invocation                                            |
| ------- | ------------------------------------------------------------ |
| Console | `abey.sendRawTransaction(params)`                           |
| RPC     | `{"method": "abey_sendRawTransaction", "params": [params]}` |

#### Example

```
> abey.sendRawTransaction({see above})
"0xe670ec64341771606e55d6b4ca35a1a6b75ee3d5145a99d05921026d1527331"
```

### abey_call

Call executes the given transaction on the state for the given block number.
It doesn't make and changes in the state/blockchain and is useful to execute and retrieve values.

Parameters
Object - The transaction call object
from: DATA, 20 Bytes - (optional) The address the transaction is sent from.
to: DATA, 20 Bytes - The address the transaction is directed to.
gas: QUANTITY - (optional) Integer of the gas provided for the transaction execution. abey_call consumes zero gas, but this parameter may be needed by some executions.
gasPrice: QUANTITY - (optional) Integer of the gasPrice used for each paid gas
value: QUANTITY - (optional) Integer of the value sent with this transaction
data: DATA - (optional) Hash of the method signature and encoded parameters. For details see abeychain Contract ABI
QUANTITY|TAG - integer block number, or the string "latest", "earliest" or "pending", see the default block parameter

| Client  | Method invocation                              |
| ------- | ---------------------------------------------- |
| Console | `abey.call(params)`                           |
| RPC     | `{"method": "abey_call", "params": [params]}` |

#### Example

```
> abey.call({"to":"0x199e7f5314d01c19fa19f220bb19f5b55d26dc33"})
"0x"
```

### abey_estimateGas

Generates and returns an estimate of how much gas is necessary to allow the transaction to complete. The transaction will not be added to the blockchain. Note that the estimate may be significantly more than the amount of gas actually used by the transaction, for a variety of reasons including EVM mechanics and node performance.

| Client  | Method invocation                                       |
| ------- | ------------------------------------------------------- |
| Console | `abey.estimateGas(options)`                            |
| RPC     | `{"method": "abey_estimateGasl", "params": [options]}` |

#### Example

```
> abey.estimateGas({"from":"0xbe7e39fa0645f6eaf501ad02318a7fda5c4df6c6"})
53000

```

### abey_getBlock

Returns information about a block by hash or number

| Client  | Method invocation                                          |
| ------- | ---------------------------------------------------------- |
| Console | `abey.getBlock(Parameters)`                           |
| RPC     | `{"method": "abey_getBlock", "params": [Parameters]}` |

Parameters can be  integer of a block number,or hash of a block,or the string "earliest", "latest" or "pending".

#### Example

```
> abey.getBlock(2)
{
  SnailHash: "0x0000000000000000000000000000000000000000000000000000000000000000",
  SnailNumber: "0x0",
  difficulty: 0,
  extraData: "0x",
  gasLimit: 21977111,
  gasUsed: 0,
  hash: "0xe3af910ef0b1e6385f44b03a88bfa91ae2e6c4409e275151a4fb46670db5c798",
  logsBloom: "0x00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000",
  number: 2,
  parentHash: "0x3247a1fff229e6a3b5f9d48e1dd4f534d59bedc83b8c62f034df7e4421da3392",
  receiptsRoot: "0x56e81f171bcc55a6ff8345e692c0f86e5b48e01b996cadc001622fb5e363b421",
  signs: [{
      result: 1,
      sign: "0x2575eb8dc39067c35cabc2a803215a93857e5ba63f70b571b188fb5e34c4020863e84ff448ef5cef0bb7a379c50c8a27e30a63ab73714be4ef66ed7402f36f7c01"
  }],
  size: 572,
  stateRoot: "0x47e15682f9a8bad160ae6c983327f7ecf0dc319e35362f7c7a23825f713f9202",
  timestamp: 1544147499,
  transactions: [],
  transactionsRoot: "0x56e81f171bcc55a6ff8345e692c0f86e5b48e01b996cadc001622fb5e363b421"
}

> abey.getBlock("0xe3af910ef0b1e6385f44b03a88bfa91ae2e6c4409e275151a4fb46670db5c798")
{
  SnailHash: "0x0000000000000000000000000000000000000000000000000000000000000000",
  SnailNumber: "0x0",
  difficulty: 0,
  extraData: "0x",
  gasLimit: 21977111,
  gasUsed: 0,
  hash: "0xe3af910ef0b1e6385f44b03a88bfa91ae2e6c4409e275151a4fb46670db5c798",
  logsBloom: "0x00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000",
  number: 2,
  parentHash: "0x3247a1fff229e6a3b5f9d48e1dd4f534d59bedc83b8c62f034df7e4421da3392",
  receiptsRoot: "0x56e81f171bcc55a6ff8345e692c0f86e5b48e01b996cadc001622fb5e363b421",
  signs: [{
      result: 1,
      sign: "0x2575eb8dc39067c35cabc2a803215a93857e5ba63f70b571b188fb5e34c4020863e84ff448ef5cef0bb7a379c50c8a27e30a63ab73714be4ef66ed7402f36f7c01"
  }],
  size: 572,
  stateRoot: "0x47e15682f9a8bad160ae6c983327f7ecf0dc319e35362f7c7a23825f713f9202",
  timestamp: 1544147499,
  transactions: [],
  transactionsRoot: "0x56e81f171bcc55a6ff8345e692c0f86e5b48e01b996cadc001622fb5e363b421"
}

> abey.getBlock("latest")
{
  CommitteeHash: "0x1dcc4de8dec75d7aab85b567b6ccd41ad312451b948a7413f0a142fd40d49347",
  SnailHash: "0x0000000000000000000000000000000000000000000000000000000000000000",
  SnailNumber: 0,
  difficulty: 0,
  extraData: "0x",
  gasLimit: 79272345,
  gasUsed: 0,
  hash: "0xd42db5fa19c087b85df80970ad31aa5e1e93080358e210afac7d59098d0cbb73",
  logsBloom: "0x0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000",  number: 106178,
  parentHash: "0xbbc548feed020abba4f22a28f1f91713f1e6be80726d5e65d2ccdaaeea2d59f3",
  receiptsRoot: "0x56e81f171bcc55a6ff8345e692c0f86e5b48e01b996cadc001622fb5e363b421",
  signs: [{
      fastHash: "0xd42db5fa19c087b85df80970ad31aa5e1e93080358e210afac7d59098d0cbb73",
      fastHeight: "0x19ec2",
      result: 1,
      sign: "0x4cab89f8ca069f249752c7b9fad1e003e7cbc1a75b593b03364870ad148da04f447a969d6a968b2a72c05ce170ef9236c83b5c27b2c99e171caa1a3d6c4f407e00"
  }, {
      fastHash: "0xd42db5fa19c087b85df80970ad31aa5e1e93080358e210afac7d59098d0cbb73",
      fastHeight: "0x19ec2",
      result: 1,
      sign: "0xb45a6a6fc093526a40c6949b2b6a8172b84dde26dcdc1f80abedc466bab37478136bf360b940da6faf1ac656b3c7177639d7eb6a4642813614a83424b1aae13001"
  }, {
      fastHash: "0xd42db5fa19c087b85df80970ad31aa5e1e93080358e210afac7d59098d0cbb73",
      fastHeight: "0x19ec2",
      result: 1,
      sign: "0x50581788f64cc3ca0c8e03d8dad419fd75f53d79ca71970380da86918d45479c636acb60d9c30f1a3ac9d9654c0897d256d73e46ee157542b357856b3532bb0f00"
  }, {
      fastHash: "0xd42db5fa19c087b85df80970ad31aa5e1e93080358e210afac7d59098d0cbb73",
      fastHeight: "0x19ec2",
      result: 1,
      sign: "0x40d9a42c5906118600df4e6b9fb641bc682769b6f16fe7f39529b0de8e7c4c10773132e7f790b70962a32b27fa7e178de532abeb8e91f119bcc538801f4a671401"
  }, {
      fastHash: "0xd42db5fa19c087b85df80970ad31aa5e1e93080358e210afac7d59098d0cbb73",
      fastHeight: "0x19ec2",
      result: 1,
      sign: "0xd7dec860b864e680f13ceec2ab9e977a7a1c905a26fa09679557d4665e2c38f74a5436b4d62a607e6490465295d312c7ec480fce521be3da4bbeb8c2e173501b00"
  }, {
      fastHash: "0xd42db5fa19c087b85df80970ad31aa5e1e93080358e210afac7d59098d0cbb73",
      fastHeight: "0x19ec2",
      result: 1,
      sign: "0xfd151b1ade2601125120a43a7f21284d4ae36d3c68df5d53425baa4985a32f4451906ba5860331cc98f0f093eaf24fb2bfaeef1e253b9af04b488e39f56f6e7100"
  }, {
      fastHash: "0xd42db5fa19c087b85df80970ad31aa5e1e93080358e210afac7d59098d0cbb73",
      fastHeight: "0x19ec2",
      result: 1,
      sign: "0xfd4e6c2356095a749e686145ce537b4b5216aaf279c08cf0c5ae5b9aa06f01c307ec0817e3a43a7d4c6fbee5008d24e69a7de82572e03b6e46864290e030b20f01"
  }],
  size: 1255,
  stateRoot: "0x7b89895f05fb321ad8fd4f9286c0deb848f7b09e8b1727d870521b9468d805c7",
  switchInfos: [],
  timestamp: 1553064924,
  transactions: [],
  transactionsRoot: "0x56e81f171bcc55a6ff8345e692c0f86e5b48e01b996cadc001622fb5e363b421"
}
```

### abey_getTransaction

Returns the information about a transaction requested by transaction hash

| Client  | Method invocation                                      |
| ------- | ------------------------------------------------------ |
| Console | `abey.getTransaction(hash)`                           |
| RPC     | `{"method": "abey_getTransaction", "params": [hash]}` |

### abey_getTransactionReceipt

Returns the receipt of a transaction by transaction hash.
Note That the receipt is not available for pending transactions.

| Client  | Method invocation                                            |
| ------- | ------------------------------------------------------------ |
| Console | `abey.getTransactionReceipt(hash)`                          |
| RPC     | `{"method": "abey_getTransactionReceipt", "params": [hash]}` |

#### Example

```
> abey.getTransactionReceipt("0x389f4ea234928502ab7a3066593a810aa9a03957b806527dfba5c55d67cbf427")
{
  blockHash: "0x5c60cb0ad195b9c35bfb5f4efb2d97bf5d0b91ae05d1072167613e9b1c892454",
  blockNumber: 1022,
  contractAddress: null,
  cumulativeGasUsed: 21000,
  from: "0xb824adf0bad5e49bb214922f4499bd2cf08ef519",
  gasUsed: 21000,
  logs: [],
  logsBloom: "0x00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000",
  status: "0x1",
  to: "0xb42669609d9932f89c16c7c1803cb03130a7b193",
  transactionHash: "0x389f4ea234928502ab7a3066593a810aa9a03957b806527dfba5c55d67cbf427",
  transactionIndex: 0
}
```

### abey_compile

Returns a list of available compilers in the client.

| Client  | Method invocation                           |
| ------- | ------------------------------------------- |
| Console | `abey.compile`                             |
| RPC     | `{"method": "abey_compile", "params": []}` |

#### Example

```
> abey.compile
{
  lll: function(),
  serpent: function(),
  solidity: function()
}

```

### abey_getWork

GetWork returns a work package for external miner. The work package consists of 3 strings
result[0], 32 bytes hex encoded current block header pow-hash
result[1], 32 bytes hex encoded seed hash used for DAG
result[2], 32 bytes hex encoded boundary condition ("target"), 2^256/difficulty

| Client  | Method invocation                           |
| ------- | ------------------------------------------- |
| Console | `abey.getWork`                             |
| RPC     | `{"method": "abey_getWork", "params": []}` |

#### Example

```
> abey.getWork
function()

```

### abey_defaultBlock

Return latest block marking

| Client  | Method invocation                                |
| ------- | ------------------------------------------------ |
| Console | `abey.defaultBlock`                             |
| RPC     | `{"method": "abey_defaultBlock", "params": []}` |

#### Example

```
> abey.defaultBlock
"latest"

```

### abey_PendingTransactions

PendingTransactions returns the transactions that are in the transaction pool
and have a from address that is one of the accounts this node manages.

| Client  | Method invocation                                       |
| ------- | ------------------------------------------------------- |
| Console | `abey.pendingTransactions`                             |
| RPC     | `{"method": "abey_PendingTransactions", "params": []}` |

#### Example

```
> abey.pendingTransactions
[]
```

### abey_committeeNumber

Return the current number of sessions of the committee

| Client  | Method invocation                                   |
| ------- | --------------------------------------------------- |
| Console | `abey.committeeNumber`                             |
| RPC     | `{"method": "abey_committeeNumber", "params": []}` |

#### Example

```
> abey.committeeNumber
41
```

### abey_fruitNumber

Returns the height of the current fruit

| Client  | Method invocation                               |
| ------- | ----------------------------------------------- |
| Console | `abey.fruitNumber`                             |
| RPC     | `{"method": "abey_fruitNumber", "params": []}` |

#### Example

```
> abey.fruitNumber
477899
```

### abey_rewardSnailBlock

RewardSnailBlock return the latest snail block rewarded.

| Client  | Method invocation                                    |
| ------- | ---------------------------------------------------- |
| Console | `abey.rewardSnailBlock`                             |
| RPC     | `{"method": "abey_rewardSnailBlock", "params": []}` |

#### Example

```
> abey.rewardSnailBlock
{
    beginFruitNumber: "0x312e",
    difficulty: "0x2934",
    endFruitNumber: "0x3169",
    extraData: "0xd98209008667657472756588676f312e31302e34856c696e7578",
    fruitDifficulty: "0x64",
    fruits: ["0xa632b2a2cb8dbea61af5c4061b506013f262c4fcdae6cfcd4539d4fb8d135295", "0x53e280a0f4566a2a9bc12faec2991059ffd993284e1688b563b95845db0d9d49", "0x82282660c6b20d778dd1c1667f4fc6bf6c29a38d17a53fc7ba638f4b950e1da8", "0xcc30df9962ea9cd0184a1a3a4076198b6936d7c79b3025521fc0b787808b9a69", "0xba1645a82012600e80f3d396da15826152e45c23b25386ac9c828855ef879fc8", "0x5ad1f91b6b40a386f2a06d6478c60395c70f484947de0491f4bd7c0e717472da", "0xb93179ddf6004f7f4a7a1a97e278a4c29170f3bb07cf7b41163f16b33d634898", "0xfbbf8ee6385b4102edfa8a539b91c83e7be2c0aa9a1487e465c6c20fe081d94b", "0x2bea7825294cdebf9b593bb9271d6d4c051111f470b7719f0b23f8d75ee450a8", "0x6123b3a6685f0e92f32d05ee437ed0df2f6a3e79a6af43964495c67e8d1f0dd3", "0xaeb33d2c1eca870093bb9f1f99e67c13411fdddaa598ca12ad1ede94c50ed2ad", "0x1df10f96d711eb500d9a2c5377ca5dbf2dbbe16df9f3faac12159e83a4b53847", "0xbbbf861be37e70d07eec9376d30db13c1704d2517d5cc312fe66b0027a27b65e", "0xf8ba7c14737c7bee419614ad7e2c9bb1ede9efe3cbe6f158d88cc8599c5d7a0b", "0x5258d8a47c1bea863b31cca6b3862da2e57fb752868358837a28fb32fb5c7864", "0xc965f1ba667f88a4370f7f40ee9090a1c66a66c2a58136b1e3653521f7892525", "0xe936d14ef8688abc507e147de72a65f83347273dce93dd19f3b2d55a1d9abe8f", "0xa8d6040922b821164902a6a33196802dc94feabcc3f82fe765c1d88bed96d662", "0x30f1f6c414afc24e4ffd1abf7757f583cca31fe187b7184630e8f1ca5dc52f9d", "0x4e5225be5985de4b6f7a64173d79277d725d236d0448d4d75076aa3ed6e7fa56", "0x7bd8b912b1e4bbc3fd4c73bde1767f5fe14f86c6b3cb041b317f6ce6f06cd542", "0xbc77f4d7e1e49f8afc8aa7d596f3d7981b5abe96d4d4a54a7d7a1010244f79b1", "0xde7998ff1ad4f9871b730d0e720d00153a2c93914d9a16acdad39765f384891f", "0xf91977e68d726fa08ded0407e0b5d590a85e1768261a9f952e236ed225dbcc75", "0x81fb25e55f5ccadca129b3b171c31e246aa00fe6bbf296436cfd70fb0e08c1a3", "0x6b49f2bc587371714436ce122cc137aaf750f394ca3bcf73659960a7b4f571f1", "0xfd13f6b303f6c6e756bd565d3ca42528e0c8de3aaa770775bc9a099361b3dfb6", "0x7d5225d125a763a5ae11470025ba9f3b1d68746786ebd384392d0784017be3b9", "0xb00a74429a42fdc7e407ff38c3a41f10e93709592cf244b861d8cef850cc8927", "0x90832895a34142519811238c7c115ac2f82f3e2bdf320f9a89ea3cc1fa16a8b3", "0x093e1df98b9a55add7da1381402adc8658e35e8d883595f3bc569f056af5ce03", "0x0c6c129571db51f9eaeec3bda70bcb817ab6d09367c33fb013fb09b8388b2c6a", "0x136f55cc75bb589a2abd9cb093c372c6c581ab30c89404f5384418caf6c5fabc", "0x6a5b44ad24be50f63e082e3cb64c804450cfd5a9813c5bb86b2daf1596b0db3d", "0x74e2f61da5829be385aaf3446e4e29a9d1269742c69885fc9188e84187e07a9e", "0x651921c1da81d4b0969796910593323c3b0b60ca816f2a938fb0d7bcb562f596", "0xc42a9d609c0bac5b1d91fdca1da86be796902b7be1990bde3a711d720eb0e367", "0x0dc661bf73cbd9b83b56a1122d7edb431993b3271e8d6ed17467130ca432816f", "0xc88d51e6a04362f45f0b8a550b33a468ae958f2018e143a295b6b867707688d9", "0xe0722964ee80138d3d16a7bb13c10b4c472ff6685244b884b6ab906ff38cf6ae", "0x9f46ce3f8d225182b09047d0c0f0e01085bbb51fb1bdd194cd0155e7543ceb2b", "0x46fc7058e2b154740d0c049e6d9730dad9dd1a45a33ddc9d9ef89ac32bbfe1f4", "0xdba7a8c2b4d90c4e2a04c7f500deeacd80670cffab96f4df01574f965a978418", "0xdc72664ddf9fd72554ea3a23bc2ec5c84a9d893a6cec0488ab9a138c30bc3e46", "0x5cdd96b3ea923246887057ededac7e5fd443b7acab2e4969195d6b2b206c80d4", "0xf8b785b944cf4d2ec01b553a9fe6c2821565496aec04601a11e5dbed47eeae6a", "0x788b53e9fb263765c91b539085d15a125649275d82ca5a7f9763039c473492e9", "0x1409fa39b9c5cb2275c53c13caf63551fdb42dfb37016d60459dd119ecbeecd2", "0x84542b65c9e5280f60bac640ce90807abae1a1fcaa66452c6d65035ac73eaf10", "0x4e0b52178228ed76ebeddb207ad8b736a36ae18c04bf2c9bf4658aba7b2e58a6", "0x6b7f21f8f28b99df7019ccf3713d948bd7ad0c333682fe3218cd8040d3f4c6b7", "0x416e7a921069bcd44aa1c9e1e71908ea84b455ba4fe3a780e54ae3d5d9bb75d6", "0x47dabc583d57ad3a76c6f92fcda8ef339809ba3dbb0d8abeff559bf80eaec92a", "0x243db458d30bbf33f8bb363b86373523a53bf6020609c753a88018cbff2cab57", "0xb6c52f12fc0f6d2505e425a7e0800d19ef2bb364f34024a5c3bd004ea37bbe2b", "0xd325781116caef391b51c25b360e8b3d6555277655ed26d9ab7767250304ce54", "0xda3eddfaeda14df7e4b764981aef139e7a89250d24a6bec2451db1615a65678e", "0x160259fc30aabae838bd1ef16f8901caeea8cea856ec60d18b340429d75e02a8", "0x27a87a5f3728a7b96efbc8c4ccbcdd9238b8b35ecdc85f171dcde05f1a244324", "0x21fba42ad5a1f5a45dadb70086485f1c97289d7dc8de49eebf490f53ff230dd7"],
    hash: "0xf81745232836e516bc1b4f6297ea39739b8335d8ac34503cf6378b0d9dcd3426",
    miner: "0x04d2252a3e0ca7c2aa81247ca33060855a34a808",
    mixHash: "0x000046d43e8286cf44b3c3e07c810646dc04c210268800a7ce005218663bf35b",
    nonce: "0x78b361ba385128ed",
    number: "0xd1",
    parentHash: "0x530de9d24cb1c49faac6f70ad6e52119026e6ad5af397895a27e7e68ca302b76",
    sha3Uncles: "0x1dcc4de8dec75d7aab85b567b6ccd41ad312451b948a7413f0a142fd40d49347",
    size: "0xe3bf",
    timestamp: "0x5be95387",
    uncles: []
}
```

### abey_snailBlockNumber

SnailBlockNumber returns the block number of the snailchain head.

| Client  | Method invocation                                    |
| ------- | ---------------------------------------------------- |
| Console | `abey.snailBlockNumber`                             |
| RPC     | `{"method": "abey_snailBlockNumber", "params": []}` |

#### Example

```
> abey.snailBlockNumber
7512
```

### abey_getCommittee

Returns information about the specified session of the committee

| Client  | Method invocation                                      |
| ------- | ------------------------------------------------------ |
| Console | `abey.getCommittee(number)`                           |
| RPC     | `{"method": "abey_getCommittee", "params": [number]}` |

#### Example

```
> abey.getCommittee(41)
{
  beginNumber: 475537,
  beginSnailNumber: 7189,
  endSnailNumber: 7368,
  id: 41,
  memberCount: 4,
  members: [{
      PKey: "04a3e174523b1054e14f123580bce258745e65591c2a4ee44764e55eb87a3782c9920d306e6121d4f10f8726800497ad9ca5a0bfdfe0832779dbaf7b95b3bf0111",
      coinbase: "0x76ea2f3a002431fede1141b660dbb75c26ba6d97"
  }, {
      PKey: "04f67ab0cd48f626da89c718bcd909a04dea393d632d3191891539ef2f5ff6bb1e5d340ebe94cb6d9126b26e1ec64bb4783e9e8ddf31346b53d651d15eb226142e",
      coinbase: "0x831151b7eb8e650dc442cd623fbc6ae20279df85"
  }, {
      PKey: "04b82f569c74364daf1767b251608950ac49c83256f5dcde771255919c8c8489da550a5c24dcfc8a735b335dff5715ca424672c81bb10e5a55ddac5204f38ab94c",
      coinbase: "0x1074f7deccf8c66efcd0106e034d3356b7db3f2c"
  }, {
      PKey: "0490bc06f4e4e5af179ea599b0793282070b9abd023a283cdda9b4edf3e596996f32843d37dcb1c3ee3dfebfe05d03dc13cfa19abe7b88a259619ea233d63778f8",
      coinbase: "0xd985e9871d1be109af5a7f6407b1d6b686901fff"
  }]
}
```

### abey_getCurrentState

Returns the current membership of the committee

| Client  | Method invocation                                         |
| ------- | --------------------------------------------------------- |
| Console | `abey.getCurrentState()`                                 |
| RPC     | `{"method": "abey_getCurrentState", "params": [number]}` |

#### Example

```
> abey.getCurrentState()
null
```

### abey_getFruit

Returns the specified fruit information

| Client  | Method invocation                                  |
| ------- | -------------------------------------------------- |
| Console | `abey.getFruit(number)`                           |
| RPC     | `{"method": "abey_getFruit", "params": [number]}` |

#### Example

```
> abey.getFruit(6)
{
  difficulty: "0x4e20",
  extraData: "0xd98209008667657472756588676f312e31302e34856c696e7578",
  fastHash: "0x8597125fd0f47fa439120ebe74b4c351f5b7c5cc6a1caafdb4c00c8b75454941",
  fastNumber: 6,
  fruitDifficulty: "0x64",
  hash: "0x2634dd54c2e3a391b986ab8966373d1fe5b9d808244d52e141cf26124d9a2714",
  miner: "0x1074f7deccf8c66efcd0106e034d3356b7db3f2c",
  mixHash: "0x1819250f9a0c71736da23f43c7f25fbd0087bd375bdc83d4dc8a7343e23d2586",
  nonce: "0x049e8f65e6e153ff",
  number: "0x1",
  sha3Uncles: "0x1dcc4de8dec75d7aab85b567b6ccd41ad312451b948a7413f0a142fd40d49347",
  signs: 3,
  size: "0x3b7",
  timestamp: "0x5be53627"
}
```

### abey_getRewardBlock

GetRewardBlock return the fast block position where the given snail block is rewarded.

| Client  | Method invocation                                        |
| ------- | -------------------------------------------------------- |
| Console | `abey.getRewardBlock(number)`                           |
| RPC     | `{"method": "abey_getRewardBlock", "params": [number]}` |

#### Example

```
> abey.getRewardBlock(11)
```

### abey_getSnail

Returns the requested snail block. When blockNr is -1 the chain head is returned.

| Client  | Method invocation                                  |
| ------- | -------------------------------------------------- |
| Console | `abey.getSnail(number)`                           |
| RPC     | `{"method": "abey_getSnail", "params": [number]}` |

#### Example

```
> abey.getSnail(1)
abey.getSnail(1)
{
  beginFruitNumber: 1,
  difficulty: 256,
  endFruitNumber: 60,
  extraData: "0xda8209018667657472756588676f312e31302e338664617277696e",
  fruitDifficulty: 50,
  fruits: 60,
  hash: "0xb0c1cd521377ba71784573af8b50192c0319fada9d5d381109ea2467c1e2c6c0",
  miner: "0xbe7e39fa0645f6eaf501ad02318a7fda5c4df6c6",
  mixHash: "0x00974c04ddfe45ae0519128a5926d478ee16d2556af6013b61b3c2723c8c0b94",
  nonce: "0x7b2d50ad27c3ec29",
  number: 1,
  parentHash: "0x5b445e49ca5b67eeca43a46420d6400a9ba90a3a3520927114d7d734683fa0e2",
  sha3Uncles: "0x1dcc4de8dec75d7aab85b567b6ccd41ad312451b948a7413f0a142fd40d49347",
  size: 45220,
  timestamp: 1544164238,
  uncles: []
}

```

### abey_snailBlockNumber

SnailBlockNumber returns the block number of the snailchain head.

| Client  | Method invocation                                    |
| ------- | ---------------------------------------------------- |
| Console | `abey.snailBlockNumber`                             |
| RPC     | `{"method": "abey_snailBlockNumber", "params": []}` |

#### Example

```
> abey.snailBlockNumber
227
```


