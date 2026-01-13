Abey adopts DPoS. The DPoS committee members use pbft protocol to produce fast blocks which include transactions and to confirm the previous snail blocks. The fPoW miners mine fruits (point to fast blocks) and snail blocks (include fruits) to resit the long range attack and to prevent tampering.

## Validator
Validators are the DPoS committee members to process transactions. The minimum amount to be validator is 20,000 abey.

The recommended specifications for running a validator node is: 
- 4 cores CPU
- 16G RAM
- 200G disk space
- 4Mpbs bandwidth
- public ip address  to participate the committee

## Locked Time
Assets will freeze after staking. And 25,000 fast blocks(about 15 days) need to unfreeze.

## Stake Guide
Stake/withdraw option is based a system contract, everyone can call the contract functions to do these actions. And the node provide some RPC APIs to request the stake and reward information.
We also provide a [staking CLI](#Staking-CLI) to stake easily.

### Staking Contract
Abey staking contract is deployed at address:
```
0x000000000000000000747275657374616b696E67
```
[Definitions of all contract functions here](https://github.com/abeyfoundation/go-abey/wiki/Staking-Contract)

### RPC and SDK
The RPC APIs definitions are [here](https://github.com/abeyfoundation/go-abey/wiki/RPC-API)


## Building CLI

`Abey Staking CLI` is a tool, which can call deposit contract participate in POS. See [this](https://github.com/abeyfoundation/go-abey/wiki/Impawn-CLI) page for more information.

### Building-Abey

Follow the appropriate link below to find installation instructions for your platform.
* [Installation Instructions for Mac OS X](https://github.com/abeyfoundation/go-abey/wiki/Installation-Instructions-for-Mac-OS-X)
* [Installation Instructions for Windows](https://github.com/abeyfoundation/go-abey/wiki/Installation-Instructions-for-Windows)
* Installation Instructions for Linux/Unix
  * [Ubuntu](https://github.com/abeyfoundation/go-abey/wiki/Installation-Instructions-for-Ubuntu)
  * [Centos](https://github.com/abeyfoundation/go-abey/wiki/Installation-Instructions-for-Centos)
* Usage instructions for Docker
  * [Docker](https://github.com/abeyfoundation/go-abey/wiki/Running-in-Docker)


### PrePare
**`gabey` support `rpc`**

```
./gabey  --rpc --rpcaddr 127.0.01 --rpcport 8545 --rpcapi "abey,net,web3,impawn"  console
```

`gabey` will listen all ip address when giving `--rpcaddr 0.0.0.0`, you can give the exact ip address that want to connect, or `--rpcaddr 127.0.0.1` only allow running on the host to connect `gabey`.

**`gabey` support `BFT`**
```
./gabey  --bftip 39.98.251.xx console
```
`bftip` must be `public ip`, open `firewall` port 8545(`rpc`),30310(`bftport`),30311(`bftport2`)

**start normal params** 
```shell
$ ./gabey --datadir data  --bftip "39.98.251.xxx" --rpc --rpcaddr "127.0.0.1"  --rpcapi "eth,abey,net,web3,impawn"  console 
```

### Building the source

Building `impawn` requires both a Go and a C compiler. Once the dependencies are installed, run

```shell
    cd go-abey/cmd/impawn
    go build -o impawn  main.go query_stake.go impawn.go
```

## Staking-CLI
The Abey Staking-CLI process is divided into three parts. First, on the premise of ensuring the balance of the account, launch a [Impawn](#Impawn) transaction, 
only if the amount of deposit(`value`) is greater than 2W `abey` can participate in the election of the committee. If you are ready to withdraw from the committee's POS consensus next epoch,
Launch a [Cancel](#cancel) transaction in the current epoch. After waiting for 15 days, we need to `actively` launch [Withdraw](#Withdraw) transaction. The extracted ABEY will be immediately transferred to the specified account.

* Since [Impawn](#Impawn) needs to be greater than 2W which can invoked successfully, [Append](#Append) can be used if the impawn amount is less than 2W.
* Update fee can invoke [UpdateFee](#UpdateFee)
* Update pk can invoke [UpdatePK](#UpdatePK)
* Query balance or reward info use [QueryReward](#QueryReward)


### Impawn

```bash
$ tree
.
├── impawn
├── impawn.go
├── main.go
├── query_stake.go
├── README.md
└── UTC--2018-09-07T07-45-16.954721700Z--xxxxxxxxxx

$ ./impawn --keystore UTC--2018-09-07T07-45-16.954721700Z--xxxxxxxxxx --rpcaddr 39.100.97.*** --rpcport 8545 --value 20000 --fee 5000
```

This command explain:
 * `--keystore` flag show load private key in UTC--2018-09-07T07-45-16.954721700Z--xxxxxxxxxx file.
 * `--rpcaddr` `--rpcport` flag show connect node ip + port,this node should you run validator node, because of it will use your local bft pk to election.
 * `--value`  deposit **>=20000** abey to staking address, the validator can be selected as a candidate.
 * `--fee`  fee(0-10000) set to 5000, smaller the value, lower benefit to the delegate, higher the value, higher benefit, the rate = fee / 10000.

**Output Log**
 ```shell
Please enter the password for 'UTC--2018-09-07T07-45-16.954721700Z--xxxxxxxxxxx': 
Connect url  http://39.100.97.***:8545  current number  4467994  address  0x7C357530174275Dd30E46319B89f71186256E4f7
Your wallet balance is  1.7779439190971997852e+07 'abey   current Total Stake  700401
Fee 5000  Pubkey  04f7a84c02fd576545c102d73cee71097813c255d5791cdcb600b82aedbf6f05dfda801e88985b3bfecc21592a7211ad4b551071f0bed8b357ea949e53fc9c5e8c  value  20000000000000000000000
TX data nonce  17  transfer value  20000000000000000000000  gasLimit  826392  gasPrice  1000000  chainID  18928
Please waiting   txHash  0xf1532026e8ffe44a1ea85c5e0772ffb7e6210d2f4bceed15c07078ddb48043a4
Transaction Success  block Number 4467996  block txs 3 blockhash 0xab6af69d64ff17affa2460a7bd39552463611c09eab2cf5a769889d25e5afb96
Staked  50000000000000000000000 wei = 20000 abey Locked  0  wei = 0 abey Unlocked  0  wei = 0 abey
```
  
### Cancel

```
$ ./impawn --keystore UTC--2018-09-07T07-45-16.954721700Z--xxxxxxxxxx --rpcaddr 39.100.97.xxx --rpcport 8545 --value 10  cancel

```

This command explain:
 * `--keystore` flag show load private key in UTC--2018-09-07T07-45-16.954721700Z--xxxxxxxxxx file.
 * `--rpcaddr` `--rpcport` flag show connect node ip + port,this node should you run validator node, because of it will use your local bft pk to election.
 * `--value`  want cancel count abey.
 * `cancel`  withdraw must call cancel first, sub command cancel represent you want cancel 10 abey to locked state, next epoch can withdraw. .

**Output Log**
```shell
Please enter the password for 'UTC--2018-09-07T07-45-16.954721700Z--xxxxxxxxxxx': 
Connect url  http://39.100.97.xxx:8545  current number  4468624  address  0x7C357530174275Dd30E46319B89f71186256E4f7
TX data nonce  19  transfer value  0  gasLimit  821784  gasPrice  1000000  chainID  18928
Please waiting   txHash  0x1bd50e4755cac0b2ae69f080d645c7a78a24c51ad31570f6afcc6f853a820b10
Transaction Success  block Number 4468626  block txs 3 blockhash 0x0248f6467a7d50962115a210e815737bb1d0be5012adc96212cf9b458dc65f05
Staked  4999000000000000000000 wei = 49990 abey Locked  10000000000000000000  wei = 10 abey Unlocked  0  wei = 0 abey
```

### QueryStaking

```
$ ./impawn --keystore UTC--2018-09-07T07-45-16.954721700Z--xxxxxxxxxx --rpcaddr 39.100.97.xxx --rpcport 8545 querystaking

```

This command explain:
 * `--keystore` flag show load private key in UTC--2018-09-07T07-45-16.954721700Z--xxxxxxxxxx file.
 * `--rpcaddr` `--rpcport` flag show connect node ip + port,this node should you run validator node, because of it will use your local bft pk to election.
 * `querystaking`  print in staking count(`Staked`), already cancel count(`Locked`), can withdraw count(`Unlocked`)..
 * Print `withdraw height`, after this, you can call withdraw, if `lock` equal `false`, can instant withdraw . 
 * If `Unlocked` not equal zero, can instant withdraw print amount.
 
**Output Log**
```shell
Please enter the password for 'UTC--2018-09-07T07-45-16.954721700Z--xxxxxxxxxxx': 
Connect url  http://39.100.97.xxx:8545  current number  4468689  address  0x7C357530174275Dd30E46319B89f71186256E4f7
Staked  49990000000000000000000 wei = 49990 abey Locked  10000000000000000000  wei = 10 abey Unlocked  0  wei = 0 abey
Your can withdraw after height 4471006  count value  10  abey  index 0  lock  abey
```

### Withdraw

*if you use [query staking](#QueryStaking) find `Unlocked` not equal zero.*
```shell
Staked  1000000000000000000 wei = 1 abey Locked  0  wei = 0 abey Unlocked  10000000000000000000  wei = 10 abey
```
**use this command**
```
$ ./impawn --keystore UTC--2018-09-07T07-45-16.954721700Z--xxxxxxxxxx --rpcaddr 39.100.97.xxx --rpcport 8545 --value 10 withdraw

```

This command explain:
 * `--keystore` flag show load private key in UTC--2018-09-07T07-45-16.954721700Z--xxxxxxxxxx file.
 * `--rpcaddr` `--rpcport` flag show connect node ip + port,this node should you run validator node, because of it will use your local bft pk to election.
 * `--value`  want withdraw count abey.
 * `withdraw`  Sub command append represent you want withdraw 10 abey to your address .

**Output Log**
```shell
Please enter the password for 'UTC--2018-09-07T07-45-16.954721700Z--xxxxxxxxxxx': 
Connect url  http://39.100.84.72:8545  current number  4501013  address  0x7C357530174275Dd30E46319B89f71186256E4f7
Your wallet balance is  1.7779438190969483348e+07 'abey   current Total Stake  700402
TX data nonce  20  transfer value  0  gasLimit  861784  gasPrice  1000000  chainID  18928
Please waiting   txHash  0x659dbaf0a920aceed810647d3e2f113b508e8748dd82d2b0dae067f952214449
Transaction Success  block Number 4501014  block txs 3 blockhash 0x177e1c2af3c6dc1eecc6139c1b438fde86cb5c30f015d611d0191af7e96230de
Staked  1000000000000000000 wei = 1 abey Locked  0  wei = 0 abey Unlocked  0  wei = 0 abey
Your wallet balance is  1.7779448190968621564e+07 'abey   current Total Stake  700392
```

### Append
```
$ ./impawn --keystore UTC--2018-09-07T07-45-16.954721700Z--xxxxxxxxxx --rpcaddr 39.100.97.xxx --rpcport 8545 --value 10 append

```

This command explain:
 * `--keystore` flag show load private key in UTC--2018-09-07T07-45-16.954721700Z--xxxxxxxxxx file.
 * `--rpcaddr` `--rpcport` flag show connect node ip + port,this node should you run validator node, because of it will use your local bft pk to election.
 * `--value`  want append count abey.
 * `append`  Sub command append represent you want c6uontinue staking after already having deposit.

**Output Log**
```shell
Please enter the password for 'UTC--2018-09-07T07-45-16.954721700Z--7c357530174275dd30e46319b89f71186256e4f7': 
Connect url  http://39.100.84.72:8545  current number  4501430  address  0x7C357530174275Dd30E46319B89f71186256E4f7
TX data nonce  22  transfer value  10000000000000000000  gasLimit  821272  gasPrice  1000000  chainID  18928
Please waiting   txHash  0x01a4d6bb8c85113118b47e27373b92dab5914f8103d3a16e46b3b4a65d15bbd3
Transaction Success  block Number 4501432  block txs 1 blockhash 0xf78f0d87e4178e22effd6715999368df8af44c1fea08c401cbd2ed23a9d7ccb8
Staked  11000000000000000000 wei = 11 abey Locked  0  wei = 0 abey Unlocked  0  wei = 0 abey
```

### UpdateFee
```
$ ./impawn --keystore UTC--2018-09-07T07-45-16.954721700Z--xxxxxxxxxx --rpcaddr 39.100.97.xxx --rpcport 8545 --fee 10 updatefee

```

This command explain:
 * `--keystore` flag show load private key in UTC--2018-09-07T07-45-16.954721700Z--xxxxxxxxxx file.
 * `--rpcaddr` `--rpcport` flag show connect node ip + port,this node should you run validator node, because of it will use your local bft pk to election.
 * `--fee`  fee(0-10000) set to 5000, smaller the value, lower benefit to the delegate, higher the value, higher benefit, the rate = fee / 10000.
 * `updatefee`  Sub command append represent only update validator fee(0-10000), which will influence delegator benefit.

**Output Log**
```shell
Please enter the password for 'UTC--2018-09-07T07-45-16.954721700Z--7c357530174275dd30e46319b89f71186256e4f7': 
Connect url  http://39.100.84.72:8545  current number  4503733  address  0x7C357530174275Dd30E46319B89f71186256E4f7
Fee 6000
TX data nonce  25  transfer value  0  gasLimit  821528  gasPrice  1000000  chainID  18928
Please waiting   txHash  0x9ba1a83f8e4a074d311ef24993cc6a3baf82936c6b73f76b77eac95204bfd772
Transaction Success  block Number 4503743  block txs 5 blockhash 0xc3a188dd5da8b47efd239cd0f47fb81ee576242c5c819830cb89fee397fe06fc
```


### UpdatePK
```
$ ./impawn --keystore UTC--2018-09-07T07-45-16.954721700Z--xxxxxxxxxx --rpcaddr 39.100.97.xxx --rpcport 8545 --bftkey 647eeeb80193a47a02d31939af29efa006dbe6db45c8806af764c18b262bb90b  updatepk

```

This command explain:
 * `--keystore` flag show load private key in UTC--2018-09-07T07-45-16.954721700Z--xxxxxxxxxx file.
 * `--rpcaddr` `--rpcport` flag show connect node ip + port,this node should you run validator node, because of it will use your local bft pk to election.
 * `--bftkey`   `647eeeb80193a47a02d31939af29efa006dbe6db45c8806af764c18b262bb90b` private key or use `--pubkey` + public key.
 * `updatepk`   Sub command updatepk represent only update validator pk, committee communication identify.

**Output Log**
```shell
Connect url  http://localhost:8545  current number  5048960  address  0xDaa07f97034916517afFF28b672A61B0027346a2
 Pubkey  045772b765ed192fdd53dd4a579dc53e37682bd975001071ff232f8cdad05734cbbdded8d8fb758845d315115f012e136739f6f3e1e9654eff45b36cb06ce8f9f6
TX data nonce  9155  transfer value  0  gasLimit  2426136  gasPrice  1000000000  chainID  18928
Please waiting   txHash  0xe157cbac55796b4a97dd28421fa1621edb562a5b95aadebe5d868879fe51e5e2
Transaction Success  block Number 5048961  block txs 1 blockhash 0xf55332105a203d92df53fc5609e8465dbae5a0ef166e3548b1c25d5c4d6e473a
Staked  100020000000000000000000 wei = 100020 abey Locked  0  wei = 0 abey Unlocked  0  wei = 0 abey
```

### QueryReward

```
$ ./impawn --keystore UTC--2018-09-07T07-45-16.954721700Z--xxxxxxxxxx --rpcaddr 39.100.97.xxx --rpcport 8545 queryreward

```

This command explain:
 * `--keystore` flag show load private key in UTC--2018-09-07T07-45-16.954721700Z--xxxxxxxxxx file.
 * `--rpcaddr` `--rpcport` flag show connect node ip + port,this node should you run validator node, because of it will use your local bft pk to election.
 * `queryreward`  print `valid` balance and `lock` balance, and every snail block your reward.
 
**Output Log**
```shell
Please enter the password for 'UTC--2018-09-07T07-45-16.954721700Z--xxxxxxxxxxx': 
Connect url  http://39.100.97.xxx:8545  current number  4468689  address  0x7C357530174275Dd30E46319B89f71186256E4f7
Your wallet valid balance is  11120.337165556650564 'abey   lock balance is  100000 'abey
queryRewardInfo [map[Address:0xa4ab123ab418197ab0b5e3c49269f5d530ef87f0 Amount:2.8086168529799127e+18]]
```

### Send
```
$ ./impawn --keystore UTC--2018-09-07T07-45-16.954721700Z--xxxxxxxxxx --rpcaddr 39.100.97.xxx --rpcport 8545 --value 10 --address 0x3f944d3f12e904e1A647E5FF9f531B8deE2346B2 send 

```

This command explain:
 * `--keystore` flag show load private key in UTC--2018-09-07T07-45-16.954721700Z--xxxxxxxxxx file.
 * `--rpcaddr` `--rpcport` flag show connect node ip + port,this node should you run validator node, because of it will use your local bft pk to election.
  * `--address`  to address that you want send.
 * `--value`  want transfer count abey.
 * `send`  Sub command append represent send normal transaction not contract,value is transfer value.

**Output Log**
```shell
Please enter the password for 'UTC--2018-09-07T07-45-16.954721700Z--xxxxxxxxxx': 
Connect url  http://39.100.84.72:8545  current number  4502701  address  0x7C357530174275Dd30E46319B89f71186256E4f7
Your wallet balance is  1.7779429190966933964e+07 'abey   current Total Stake  700411
TX data nonce  23  transfer value  10000000000000000000  gasLimit  21000  gasPrice  1000000  chainID  18928
Please waiting   txHash  0x09b401884282f32f083d75ad537d1b461dc451c77a25e44f2d1fd859410561d0
Transaction Success  block Number 4502702  block txs 5 blockhash 0xc3a188dd5da8b47efd239cd0f47fb81ee576242c5c819830cb89fee397fe06fc
```

### QueryTx
```
$ ./impawn  --rpcaddr 39.100.97.xxx --rpcport 8545 --txhash 0x40c78769add225421c45fa2e9dc206c1d9a03199f78c34644f3c0bf274f3066b querytx

```

This command explain:
 * `--rpcaddr` `--rpcport` flag show connect node ip + port,this node should you run validator node, because of it will use your local bft pk to election.
 * `--txhash`   send transaction hash.
 * `querytx`  Sub command append represent specify txhash to query.
 
**Output Log**
```
Connect url  http://39.100.84.xxx:8545  current number  4501518  address  0x0000000000000000000000000000000000000000
Transaction Success  block Number 4501432  block txs 1 blockhash 0xf78f0d87e4178e22effd6715999368df8af44c1fea08c401cbd2ed23a9d7ccb8
```
