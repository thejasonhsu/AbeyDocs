## Table of Contents
- [Abstract](#abstract)
- [1. Introduction](#1-introduction)
- [2. Background](#2-background)
  - [2.1 Related Works](#21-related-works)
- [3. Consensus](#3-consensus)
  - [3.1 Design Overview](#31-design-overview)
  - [3.2 Recap of DPoS Consensus Protocol](#32-recap-of-dpos-consensus-protocol)
    - [3.2.1 Daily Off-chain Consensus Protocol](#321-daily-off-chain-consensus-protocol)
    - [3.2.2 Mempool Subprotocol](#322-mempool-subprotocol)
    - [3.2.3 Main Consensus Protocol](#323-main-consensus-protocol)
  - [3.3 Variant Day Length and Committee Election](#33-variant-day-length-and-committee-election)
  - [3.4 Application-Specific Design](#34-application-specific-design)
    - [3.4.1 Physical Timing Restriction](#341-physical-timing-restriction)
  - [3.5 Computation and Data Sharding, and Speculative Transaction Execution](#35-computation-and-data-sharding-and-speculative-transaction-execution)
- [4. Smart Contracts in Virtual Machines](#4-smart-contracts-in-virtual-machines)
  - [4.1 Design Rationale](#41-design-rationale)
    - [4.1.1 Containers vs. Virtual Machines](#411-containers-vs-virtual-machines)
    - [4.1.2 Containers in Serverless Architectures](#412-containers-in-serverless-architectures)
  - [4.2 Abey Virtual Machine (AVM)](#42-abey-virtual-machine-avm)
- [5. Blocks, State, and Transactions](#5-blocks-state-and-transactions)
  - [5.1 Block Structure in Abey 3.0](#51-block-structure-in-abey-30)
  - [5.2 Parallel Transaction Execution](#52-parallel-transaction-execution)
- [6. Incentive Design](#6-incentive-design)
  - [6.1 Gas Market and Sharding](#61-gas-market-and-sharding)
  - [6.2 Compensation Infrastructure](#62-compensation-infrastructure)
  - [6.3 Hybrid Infrastructure for Storage and Execution](#63-hybrid-infrastructure-for-storage-and-execution)
- [7. Future Direction](#7-future-direction)
- [8. Conclusions](#8-conclusions)
- [References](#references)

---

## Abstract

In this paper, we present the initial design of **Abey 3.0** and other technical details. Briefly, our consensus design enjoys the same consistency, liveness, transaction finality, and security guarantees. We discuss optimizations, such as the frequency of rotating committee members and physical timestamp restrictions. The primary focus of Abey is to advance these concepts and build a blockchain specifically designed for the Abey ecosystem. We also utilize:

1. Data sharding and speculative transactions.  
2. Evaluation of smart contracts in a hybrid cloud infrastructure.  
3. Usage of existing volunteer computing protocols for a compensation infrastructure.  

---

## 1. Introduction

With the surging popularity of cryptocurrencies, blockchain technology has caught the attention of both industry and academia. One can think of blockchain as a shared computing environment where peers join and quit freely, with a commonly agreed consensus protocol. The decentralized nature of blockchain, together with transaction transparency, autonomy, and immutability, is critical to cryptocurrencies.

However, earlier-designed cryptocurrencies, such as Bitcoin [[1]](#ref-1) and Ethereum [[2]](#ref-2), have been recognized as unscalable in terms of transaction rates and not economically viable due to their energy consumption and computational power requirements.

With the growing demand for apps and platforms utilizing public blockchains, a viable protocol that enables higher transaction rates is the primary focus of new systems. For example, a public blockchain hosting computationally intensive peer-to-peer gaming applications with a large user base would face delays in transaction confirmation if it also hosted ICO contracts.

Other models, such as Delegated Proof of Stake (PoS) and Hybrid Consensus, also exist. Abey 2.0 adopted Hybrid Consensus, incorporating PBFT [[3]](#ref-3) and Proof of Work (PoW). Abey 3.0 phases out PoW for a more energy-efficient Proof-of-Stake protocol. PoS ensures safety as long as no more than one-third of validators are malicious. Validators achieve consensus using a modified PBFT [[4]](#ref-4), ensuring incentivization, instant finality, high throughput, and fair committee rotation.

---

## 2. Background

The core strength of this proposal lies in recognizing DPoS principles and utilizing DailyBFT rotating committees for validator fairness.

### 2.1 Related Works

In PoS systems, the native token stores both value and voting power. PoS achieves Sybil resistance through BFT while consuming less energy. Unlike PoW, PoS selects validators proportionally to staked holdings, reducing computation costs while offering scalability and security.

---
## 3. Consensus

### 3.1 Design Overview

Our consensus design is a proof-of-stake model that has been extended to meet the requirements of applications that demand high throughput and fairness. The protocol builds on Hybrid Consensus [[5]](#ref-5). Our adversary model follows Pass and Shi [[6]](#ref-6), where adversaries can adaptively corrupt nodes with some delay. Future work will formalize these modifications under the Universal Composability framework [[7]](#ref-7).

### 3.2 Recap of DPoS Consensus Protocol

#### 3.2.1 Daily Off-chain Consensus Protocol

In **DailyBFT**, committee members execute an **off-chain Byzantine Fault Tolerant (BFT) instance** to establish a daily transaction log, while non-members verify progress by counting signatures from the committee. This design extends security guarantees to both non-members and late-joining nodes. It enforces a **termination agreement**, requiring that all honest participants converge on the same final log once the protocol terminates.  

Committee members produce **signed daily log hashes**, which are consumed by the Proof-of-Stake consensus protocol. These signed hashes guarantee **completeness** (no valid transaction is omitted) and **unforgeability** (signatures cannot be fabricated).  

During **key generation**, each node’s public key is added to a maintained key list. Upon receiving a **committee signal**, nodes may be conditionally elected as committee members. The system selectively opens participation to the committee through this controlled election mechanism.  

- **If the node is a BFT committee member:**  
  A **BFT virtual node** is instantiated, denoted as *BFTpk*, which begins receiving transactions (*TXs*). The log’s completion status is continuously monitored, and the process halts once a **stop signal** is signed by at least one-third of the initially designated committee public keys. Throughout execution, the protocol performs a recurring *“Until Done”* check: after each gossip round completes, all stop log entries are removed, ensuring freshness and consistency.  

- **If the node is not a BFT committee member:**  
  Upon receiving a transaction, the node appends it to its local history and verifies that the transaction has been signed by at least one-third of the distinct committee public keys. This guarantees the correctness and validity of the received log.  

Finally, to prevent **namespace collisions**, the signing algorithm uses unique prefixes:  
- `"0"` for messages belonging to the inner BFT instance.  
- `"1"` for messages belonging to the outer DailyBFT layer.  

#### 3.2.2 Mempool Subprotocol

The mempool maintains a set of pending transactions. When a new transaction arrives, it is initialized with status zero and added to the set. On receipt of a proposal, transactions are grouped into blocks and disseminated by gossip. Confirmed transactions are removed. Query interfaces allow validation of transaction state.

#### 3.2.3 Main Consensus Protocol

A new node inherits transcript history and implicit routing from peers. It interacts with mempools, preprocessing functions, the off-chain DailyBFT consensus, and on-chain validation.

### 3.3 Variant Day Length and Committee Election

Committees rotate after fixed time periods, with the chain serving as a logical clock [[8]](#ref-8). Committee members are selected as miners of the most recent blocks (of size `csize`). Reducing rotation frequency minimizes switching overhead but enforced switches guarantee fairness. If misbehavior is detected, authenticated evidence is submitted on-chain, triggering a forced switch. This approach is inspired by Thunderella [[9]](#ref-9).

### 3.4 Application-Specific Design

#### 3.4.1 Physical Timing Restriction

In conventional consensus designs, miners, committee members, or leaders are often able to **reorder transactions** within a short timing window. While this flexibility may not affect some applications, it creates a critical fairness problem for decentralized systems such as **commercial exchanges**, where the exact timing of trades must be preserved. If reordering is permitted, malicious or even rational participants may reorder or insert their own transactions to gain unfair profit. This incentive becomes even stronger under conditions of **high throughput**.  

A further complication arises because malicious reordering is difficult to detect: natural **network latency** already causes reordering, and latency values are only observable by the receiving node. Thus, nodes can always claim latency-related delays, making it impossible to prove malicious intent.  

To mitigate these problems—particularly for use cases like decentralized exchanges, Abey introduces a mechanism called the **sticky timestamp**. With this approach, a client must attach a physical timestamp (*Tp*) to each transaction at the moment it is created. This timestamp, along with other transaction data, is digitally signed. Validators then enforce additional checks using a heuristic parameter (*TΔ*) to ensure the validity of the timestamp. The procedure is shown in **Algorithm 1**.  

During log materialization within the BFT process, the leader orders the batch of transactions primarily by their physical timestamps, using sequence numbers to break ties (a rare case). While this ordering step could technically be deferred until evaluation and verification, performing it early simplifies the workflow.  

This modification introduces several important properties:  

1. **Strict intra-node ordering:** The order of transactions from any node *Ni* is preserved according to their physical timestamps. This eliminates the risk of malicious reordering between two or more transactions from the same node.  
2. **Batch-level ordering:** Within any batch of transactions output by the BFT committee, transactions are strictly ordered by timestamp.  
3. **Resistance to forged timestamps:** Nodes cannot freely manipulate timestamps due to the *TΔ* restriction window, preventing unrealistic backdating or future-dating of transactions.  

However, this design also introduces some trade-offs:  

- **Reduced throughput:** If the parameter *TΔ* is set poorly relative to varying network latencies, transactions may be unnecessarily aborted.  
- **Clock-related rejection:** Committee members can still exploit unsynchronized clocks by rejecting certain transactions. Nevertheless, members already possess the ability to reject transactions selectively.  
- **Risk of honest-node rejection:** Honest nodes with poorly synchronized clocks may reject valid transactions. This risk can be mitigated by enforcing stricter requirements for committee eligibility, such as proof of synchronized timekeeping.  

**Algorithm 1: Extra Verification Regarding Physical Timestamp**
```pseudo
Data: Input Transaction TX
Result: Boolean (verification passed or failed)

current_time ← Time.Now()
if |current_time - TX.Tp| > TΔ then
    return false
var txn_history = static dictionary of lists
if txn_history[TX.from] == NULL then
    txn_history[TX.from] = [TX]
else
    if txn_history[TX.from][-1].Tp - TX.Tp > 0 then
        return false
    else
        txn_history[TX.from].append(TX)
        return true
```

### 3.5 Computation and Data Sharding, and Speculative Transaction Execution

Abey introduces sharding and speculative execution. Normal shards handle subsets of transactions, while a primary shard coordinates and finalizes. Shards do not overlap in membership. Each shard maintains state partitions. Transactions execute speculatively with lower and upper bounds, validated through two-phase commit across shards.

#### Shard Structure

In Hybrid Consensus, DailyBFT instances are indexed into a deterministic sequence, DailyBFT\[1 … R\]. Our modification allows **multiple sequences of DailyBFT instances** to exist concurrently. Specifically, we denote the *t*-th DailyBFT sequence as shard *St*.  

- The number of shards is fixed as *C*.  
- Each DailyBFT instance functions as a **normal shard**.  
- In addition to these *C* normal shards, we introduce a **primary shard Sp**, composed of *csize* nodes.  

The **primary shard** serves two roles:  
1. It finalizes the ordering of outputs from all normal shards.  
2. It acts as the **coordinator** in distributed transaction processing.  

Normal shards do not directly interact with the Hybrid Consensus component. Instead, they **submit logs to the primary shard**, which then communicates with Hybrid Consensus on their behalf.  

#### Shard Independence and Election

To ensure fairness and security, **no two shards (normal or primary) may share nodes**. This constraint is enforced during the committee selection process. The election of multiple shards follows the same procedure described in [Section 3.3](#33-variant-day-length-and-committee-election).  

#### State Partitioning

The **global state** is uniformly partitioned into *C* shards, each responsible for a specific account range. This ensures that queries directed to a shard always yield a **consistent state**.  

- Data is split into **data sectors**, each identified by an address.  
- Each data sector `DS[addr]` contains metadata, including:  
  - *rts* (read timestamp)  
  - *wts* (write timestamp)  
  - *readers*  
  - *writers*  

The mapping from data position to sector address is public, and the host shard for any address can be determined by the function `host(addr)`.  

#### Speculative Execution with Logical Timestamps

By treating each normal shard as a distributed processing unit, we incorporate the design of **logical timestamps** [[10]](#ref-10) within distributed transaction processing systems [[11]](#ref-11). This enables parallel speculative execution. Our approach builds on a simplified version of **MaaT**, without auto-adjustment of timestamps for other transactions.  

Normal shards operate like DailyBFT instances, but with the following modifications to support parallel, speculative execution.  

#### Primary Shard Responsibilities

The primary shard collects and processes outputs from all normal shards:  

1. **Batch Collection:**  
   When the primary shard receives a batch of transactions from a shard, it waits for the corresponding batches from all other shards.  
   - If a shard fails to provide its batch within a timeout, the batch is deemed failed.  
   - In this case, a **committee switch** is triggered at the beginning of the next day.  

2. **Ordering Transactions:**  
   After receiving all shard logs, the primary shard sorts the transactions by their **commit timestamps**.  
   - If a transaction belongs to an earlier batch, the batch number is used as the first key for ordering.  
   - If a transaction’s physical timestamp conflicts with timestamps from other shards, the entire batch is considered invalid, and **all transactions in that batch are aborted**.  

3. **Filtering Transactions:**  
   After sorting, the primary shard filters the transactions to retain the **longest non-decreasing sequence** in terms of physical timestamps.  

4. **Final Log:**  
   The primary shard outputs the finalized log to the DPoS consensus component as that day’s log.  

#### Limitations

While this sharding and speculative execution scheme significantly improves parallelism and throughput, it also introduces trade-offs:  

- **Delayed confirmation:** Unlike instant-finality protocols, this design requires additional coordination and ordering steps, leading to longer confirmation times.  
- **Optimization opportunities:** Further refinements are possible to reduce latency and improve batch recovery.  

**Algorithm 2: Sharding and Speculative Transaction Processing**
```pseudo
On BecomeShard:
  Initialize state data: lastReaderTS = -1, lastWriterTS = -1

On transaction TX at shard Si:
  TX.lowerBound = 0
  TX.upperBound = ∞
  TX.state = RUNNING
  TX.before = []
  TX.after = []
  TX.ID = rand

On Read Address(addr):
  if host(addr) == Si:
    return local read
  else:
    broadcast readRemote(addr)
    wait for 2f+1 replies, else abort
  update TX.before and TX.lowerBound

On Write Address(addr):
  if host(addr) == Si:
    local write
  else:
    broadcast writeRemote(addr)
    wait for 2f+1 replies, else abort
  update TX.after and TX.lowerBound

On Finish Execution:
  for every TX' in TX.before do
      TX.lowerBound = max(TX.lowerBound, TX'.upperBound)

  for every TX' in TX.after do
      TX.upperBound = min(TX.upperBound, TX'.lowerBound)

  if TX.lowerBound > TX.upperBound then
      Abort TX

  Broadcast Precommit(TX.ID, (TX.lowerBound + TX.upperBound) / 2) 
      to all remote shards previously accessed by TX

// Note: If TX.upperBound = ∞, assign an arbitrary number greater than TX.lowerBound.

On receive readRemote(addr, ID):
  if host(addr) == Si then
      DS[addr].readers.append(ID)
      return DS[addr].value, DS[addr].wts, DS[addr].writers
  else
      Ignore

On receive writeRemote(addr, ID):
  if host(addr) == Si then
      DS[addr].writers.append(ID)
      Write to a local copy
      return DS[addr].rts, DS[addr].readers
  else
      Ignore
```

**Algorithm 3: Speculative Commit**
```pseudo
On receive Precommit(ID, cts):
  Look up TX by ID
  if Found and cts not in [TX.lowerBound, TX.upperBound] then
      Broadcast Abort(ID) to the sender’s shard
  else
      TX.lowerBound = TX.upperBound = cts
      For every data sector DS[addr] TX reads:
          DS[addr].rts = max(DS[addr].rts, cts)
      For every data sector DS[addr] TX writes:
          DS[addr].wts = max(DS[addr].wts, cts)
      Broadcast Commit(ID, batchCounter) to the sender’s shard

// batchCounter is a counter that increases by 1 
// whenever the shard submits a batch of logs to the primary shard.

On receive 2f + 1 Commit(ID, batchCounter) 
from each remote shard accessed by TX:
  TX.lowerBound = TX.upperBound = cts
  For every data sector DS[addr] TX reads:
      DS[addr].rts = max(DS[addr].rts, cts)
  For every data sector DS[addr] TX writes:
      DS[addr].wts = max(DS[addr].wts, cts)
  Mark TX committed
  Set TX.metadata = [ShardID, batchCounter]

On output log:
  Sort transactions by commit timestamp (cts)
  Break ties using physical timestamp
```

This design ensures speculative execution remains serializable across shards.

---
## 4. Smart Contracts in Virtual Machines

### 4.1 Design Rationale

Because Abey blockchain is built as a **hybrid model**, we have the opportunity to explore a broader design space, including the potential of a **hybrid cloud ecosystem** to support scalability and performance.  

A recurring challenge in blockchain documentation has been the **notation-heavy style** of Ethereum’s Yellow Paper [[12]](#ref-12). While rigorous, its mathematical format can make specifications difficult to interpret and apply. To address this, we adopt an approach similar to the **KEVM Jello Paper** [[13]](#ref-13), which provides formal semantics in a clear and accessible format. Our goal is to document both the **Ethereum Virtual Machine (EVM)** and the **Abey Virtual Machine (AVM)** (see [Section 4.2](#42-abey-virtual-machine-avm)) in a similarly transparent way.  

---

### 4.1.1 Containers vs. Virtual Machines

A critical design question is whether **containers** can effectively replace **virtual machines (VMs)** as the execution environment for smart contracts.  

One framework that closely aligns with this idea is **Hyperledger Fabric** [[14]](#ref-14). However, converting Fabric’s **permissioned architecture** into a **permissionless public blockchain** raises significant challenges, most notably the **chaincode issue**.  

In Fabric, a chaincode (a smart contract) can be deployed within a single container. While this works in a permissioned setting, it is **not scalable for public blockchains**. Because each node in a public chain must maintain a copy of all smart contracts, the system would require **thousands of containers per node** — an impractical and resource-intensive approach.  

The community has already examined container scalability limits:  
- **Kubernetes** supports approximately **100 pods per node**, which equates to around **250 containers per node** [[15]](#ref-15).  
- **Red Hat OpenShift Container Platform 3.9** defines similar cluster limits [[16]](#ref-16).  
- Even with advanced storage optimizations such as **brick multiplexing** [[17]](#ref-17), the maximum number of containers per node (*MAX_CONTR*) cannot realistically reach the **1,000+ range** needed to handle large-scale workloads.  

Further discussion on this limitation can be found in **Kubernetes GitHub issue reports** [[18]](#ref-18), which highlight how workload-specific configurations typically determine the maximum number of pods per node. In practice, those aiming to scale containers generally prefer **horizontal scaling** (adding more nodes) over **vertical scaling** (stacking more workload onto a single node) [[19]](#ref-19). Vertical scaling significantly increases design complexity and offers diminishing returns.  

Because decentralized blockchains inherently carry heavier workloads than centralized systems, simply scaling containers is not a convincing solution for sustainability. Ethereum itself already supports **over 1,000 deployed smart contracts**, and applying a container-only model here would amount to little more than a **crude optimization attempt** within the current container ecosystem.  

---

### 4.1.2 Containers in Serverless Architectures

A possible alternative is to use **containers in a serverless architecture**. In such a system, containers are spun up only when needed, reducing persistent overhead.  

However, consider a scenario where more than **2,000 contracts** are online and the concurrent invocation calls (within a moving time window) exceed the *MAX_CONTR* limit. At that point, the same scalability bottleneck reappears.  

The only practical safeguard would be to enforce **throttling mechanisms** on the maximum number of concurrent requests. While this prevents system overload, it **severely limits the Transactions Per Second (TPS)** achievable at the consensus layer. In effect, engineering constraints become the bottleneck, limiting what the blockchain could otherwise achieve.  

For this reason, we choose to **retain the EVM design**, albeit with modifications tailored to Abey’s requirements. This approach provides a more stable and scalable foundation for executing smart contracts while avoiding the pitfalls of over-reliance on containers.  

---

### 4.2 Abey Virtual Machine (AVM)

The AVM is a deterministic, stack-based architecture modeled after the EVM [[20]](#ref-20), but adapted for Abey. It provides:

- **Determinism:** Contract execution yields identical results across all nodes.  
- **Compatibility:** Supports most EVM bytecode, easing migration.  
- **Native Cryptography:** Built-in elliptic curve cryptography.  
- **Hybrid Cloud Execution:** Offloads computationally intensive tasks with verifiable results.  
- **Scalability:** Deployable alongside Kubernetes [[15]](#ref-15) and OpenShift [[16]](#ref-16).  
- **Interfaces:** Works with Tendermint-style ABCI, DailyBFT consensus, permissioned EVMs, and RPC gateways.  

---

## 5. Blocks, State, and Transactions

### 5.1 Block Structure in Abey 3.0

In Abey 3.0, blocks are generated by the **Proof-of-Stake (PoS) committee** once consensus is reached. Each block primarily contains **transactions and smart contracts**.  

Similar to Ethereum blocks, an Abey block provides:  
- **TxHash** – the hash of included transactions.  
- **Root** – the state root after execution.  
- **ReceiptHash** – the hash of transaction receipts.  

These fields allow non-committee members to efficiently verify the validity of the transactions included in the block body.  

However, unlike Ethereum, each Abey block also includes:  
- **Committee Information:** A record of the PoS committee members responsible for block generation. This information appears in the **first block of every epoch** through the `commitInfo` field.  
- **Validator Signatures:** Each block contains validator signatures. The block header includes a `SignHash` derived only from the parent block signatures, ensuring security while minimizing overhead.  

---

### 5.2 Parallel Transaction Execution

Modern processors are designed with **multi-core architectures**, making parallel execution an industry standard. Abey leverages this trend with a **Parallel Transaction Execution (PTE) mechanism**, which maximizes CPU utilization by enabling independent transactions within a block to execute concurrently.  

#### Traditional Sequential Execution
In most blockchains, transactions are executed **sequentially**:  
1. Transactions are read one at a time from the block.  
2. Each transaction updates the state machine.  
3. The system moves to the next transaction until all are processed.  

This sequential execution leads to inefficiencies:  
- Transactions that could be executed independently are delayed.  
- Conflicts between dependent transactions can cause inconsistent final states if parallelism is not managed carefully.  

#### Dependency Problem
Determining transaction **dependencies** is critical to safe parallel execution.  

Example:  
- Transactions: `A → B`, `C → D`, `D → E`.  
- Here, `D → E` depends on the result of `C → D`, so they cannot run in parallel.  
- However, `A → B` is unrelated and can safely run in parallel with the other two.  

This logic works well in **simple payment systems**, but becomes complex in **Turing-complete blockchains** with smart contracts. For instance:  
- Transaction `A → B` may appear independent from accounts `C` and `D`.  
- However, if account `A` is a special contract requiring transaction fees to be deducted from `C`, then all three transactions (`A → B`, `C → D`, `D → E`) are interdependent.  
- In this case, parallel execution would break correctness.  

#### Trial-and-Error Method
To address these uncertainties, Abey adopts a **trial-and-error approach** for parallel execution:  

1. **Preparation:** Convert all transactions into “message” format.  
2. **Grouping:** Cluster transactions based on associated addresses.  
3. **Execution:** Execute each group of transactions in parallel.  
4. **Conflict Checking:** After execution, detect conflicts between groups.  
   - If conflicts exist, roll back the affected transactions, regroup them, and re-execute.  
5. **Result Collection:** Once conflict-free, collect execution results, update the global state tree, and generate a new **state root**.  

#### Trade-offs
- This method significantly improves performance by enabling parallelism in most cases, since the majority of transactions are **independent**.  
- Inefficiencies can occur when grouping is inaccurate or when rollbacks are frequent.  
- However, because unrelated transactions dominate in practice, the benefits of parallel execution outweigh the occasional overhead.  

---
## 6. Incentive Design

The **Proof-of-Work (PoW)** protocol has historically demonstrated its ability to attract computational resources at an unprecedented scale. Networks such as Bitcoin and Ethereum are clear examples of this success. However, the vast computational power they harness has been largely wasted on **hash calculations**, consuming enormous amounts of electricity while producing little tangible utility beyond enhanced network security.

In contrast, Abey proposes a **compensation infrastructure** that balances the workload of PoS committee members and non-member full nodes. Rather than dedicating resources solely to mining, participating nodes can redirect computational capacity toward **useful tasks**, such as:  
- Scaling **transactions per second (TPS)**, and  
- Providing **on-chain data storage**.  

---

### 6.1 Gas Market and Sharding

In Ethereum, the **gas price** is determined through a spot market similar to the electricity spot market [[21]](#ref-21). This system lacks arbitrage opportunities and is considered **incomplete**, meaning the **Fundamental Theorem of Asset Pricing** [[22]](#ref-22) does not apply. Consequently, Ethereum gas prices exhibit high volatility, following a **shot-noise process**.

To address this, Abey introduces a **gas marketplace**, where gas can be traded as **futures contracts**, making the market complete in the infinitesimal limit. This mechanism is expected to significantly reduce volatility and provide greater stability compared to Ethereum’s current model.  

#### Gas Futures Execution Process
1. **Smart Contract Agreement:**  
   - Party A agrees to pay Party B *x* ABEY.  
   - Party B agrees to execute Party A’s smart contract between times *T0* and *T1*, requiring exactly one gas unit.  

2. **Contribution to Gas Pool:**  
   - Party B contributes *x* ABEY to a **gas pool** associated with the PBFT committee (*C*) responsible for executing the contract.  

3. **Distribution of Rewards:**  
   - Members of committee *C* receive equal shares of the pool.  
   - The pool also returns the **average cost per gas** (μ).  

4. **Settlement:**  
   - If B contributed **less than μ**, they must pay the difference to those who contributed more.  
   - If B contributed **more than μ**, they are compensated by others.  

This scheme rewards **liquidity providers** who accurately anticipate network stress, while the averaging mechanism in the gas pool smooths volatility. Thus, the **gas price itself becomes a reliable indicator of network stress**.  

#### Dynamic Committee Adjustment
- If the **moving average gas price** remains above a threshold, a **new PBFT committee** is spawned via a **quantum appearance process**.  
- If the price remains below a threshold, an existing PBFT committee will not be renewed after completing its term.  

#### Mining Reward Distribution
Let *n* represent the number of PBFT committees and α > 1. Then:  

- **PBFT nodes’ reward share** = *n / (α + n)*  
- **PoW nodes’ reward share** = *α / (α + n)*  

This ensures that as the system scales, **new nodes are incentivized to increase TPS**, preserving long-term scalability. The parameter α defines the threshold where rewards are split 50/50 between PBFT and PoW nodes.  

---

### 6.2 Compensation Infrastructure

Treating all shards as equal in terms of bandwidth and processing capacity can create skewed outcomes, such as:  
- Inconsistent TPS,  
- Cross-shard timeout issues, and  
- Bottlenecks in transaction ordering from the primary shard.  

To mitigate this, Abey introduces a **compensation infrastructure** modeled on distributed volunteer computing frameworks like:  
- **BOINC** (Berkeley Open Infrastructure for Network Computing) [[25]](#ref-25), which has been successfully applied in scientific computing projects such as **CERNVM** [[26]](#ref-26) and **LHC@Home** [[27]](#ref-27).  
- **Gridcoin** [[23]](#ref-23), which pioneered a blockchain-based volunteer computing model.  
- **Golem Network** [[24]](#ref-24), which provides a decentralized marketplace for computing power.  

These systems demonstrate the potential for incentivizing useful distributed computing. However, one known risk is **interest inflation**:  
- Early stakeholders may accumulate outsized rewards due to “algorithmic luck.”  
- Later participants are left competing for smaller pools, leading to reward condensation.  

Abey’s incentive model is designed to prevent this imbalance by ensuring **equitable long-term distribution of rewards**.  

---

### 6.3 Hybrid Infrastructure for Storage and Execution

Depending on transaction types and the need for **decentralized storage**, we propose a **hybrid infrastructure** that combines:  
- **BOINC** for distributed computing,  
- **IPFS/Swarm** for decentralized storage,  
- **EVM and AVM** for smart contract execution, and  
- **Linux Containers** for resource isolation.  

This hybrid model ensures scalability, efficient resource allocation, and strong incentivization for participants. Further refinements of this infrastructure will be detailed in future versions of this whitepaper.  


---

## 7. Future Direction

Abey continues to evolve. The following areas represent priorities for future development:

- **Improved Timestamp Synchronization:** Ensuring physical and logical time consistency across shards and committees to prevent unfair transaction ordering.  
- **Refined Incentive Design:** Introducing dynamic gas pricing, enhanced futures markets, and cross-shard liquidity incentives.  
- **Replica-Based Sharding:** Exploring sharding models where replicas further improve reliability, fault tolerance, and scalability.  
- **Zero-Knowledge Proofs:** Integrating zk-SNARKs and zk-STARKs to strengthen privacy, scalability, and verifiability of off-chain computations.  
- **Hybrid Execution Environments:** Combining AVM with Linux containers to allow greater flexibility for decentralized applications and microservices.  
- **Enhanced Governance:** On-chain governance mechanisms allowing stakeholders to propose, deliberate, and vote on protocol upgrades.  
- **Cross-Chain Interoperability:** Bridges and standards enabling Abey to interact with other major blockchain ecosystems.  

---

## 8. Conclusions

Abey 3.0 presents a robust, scalable proof-of-stake protocol designed for the future of decentralized applications. Its innovations include:

- **Consensus:** Rotating PoS committees with fair elections, instant finality, and resilience to adversarial behavior.  
- **Virtual Machine:** The Abey Virtual Machine (AVM), offering determinism, compatibility with EVM, and hybrid cloud execution.  
- **Sharding:** Computation and data sharding with speculative execution to maximize throughput and maintain serializability.  
- **Incentives:** A redesigned gas market model using futures and volunteer computing frameworks.  
- **Scalability:** Parallel execution of independent transactions and verifiable cross-shard communication.  
- **Extensibility:** Integration paths for zero-knowledge proofs, enhanced governance, and cross-chain communication.  

These features together establish Abey as a next-generation public blockchain infrastructure, combining efficiency, fairness, and extensibility to meet the needs of modern decentralized ecosystems.

---

## References

1. <a id="ref-1"></a> S. Nakamoto. *Bitcoin: A Peer-to-Peer Electronic Cash System*. [http://bitcoin.org/bitcoin.pdf](http://bitcoin.org/bitcoin.pdf), 2008.  
2. <a id="ref-2"></a> V. Buterin. *Ethereum White Paper*. [https://github.com/ethereum/wiki/wiki/White-Paper](https://github.com/ethereum/wiki/wiki/White-Paper), 2014.  
3. <a id="ref-3"></a> M. Castro, B. Liskov, et al. *Practical Byzantine Fault Tolerance*. In *OSDI*, vol. 99, pp. 173–186, 1999.  
4. <a id="ref-4"></a> Id. at 173–186.  
5. <a id="ref-5"></a> E. Androulaki, A. Barger, V. Bortnikov, et al. *Hyperledger Fabric: A Distributed Operating System for Permissioned Blockchains*. [https://arxiv.org/pdf/1801.10228v1.pdf](https://arxiv.org/pdf/1801.10228v1.pdf), 2018.  
6. <a id="ref-6"></a> R. Pass, E. Shi. *Hybrid Consensus: Efficient Consensus in the Permissionless Model*. In *LIPIcs - Leibniz International Proceedings in Informatics*, vol. 91. Schloss Dagstuhl–Leibniz-Zentrum für Informatik, 2017.  
7. <a id="ref-7"></a> R. Canetti. *Universally Composable Security: A New Paradigm for Cryptographic Protocols*. In *Proceedings of the 42nd IEEE Symposium on Foundations of Computer Science*, pp. 136–145, IEEE, 2001.  
8. <a id="ref-8"></a> R. Pass, E. Shi. *Hybrid Consensus: Efficient Consensus in the Permissionless Model*. In *LIPIcs - Leibniz International Proceedings in Informatics*, vol. 91. Schloss Dagstuhl–Leibniz-Zentrum für Informatik, 2017.  
9. <a id="ref-9"></a> R. Pass, E. Shi. *Thunderella: Blockchains with Optimistic Instant Confirmation*. [https://eprint.iacr.org/2017/913.pdf](https://eprint.iacr.org/2017/913.pdf), 2017.  
10. <a id="ref-10"></a> X. Yu, A. Pavlo, D. Sanchez, S. Devadas. *TicToc: Time Traveling Optimistic Concurrency Control*. In *SIGMOD*, pp. 1629–1642, ACM, 2016.  
11. <a id="ref-11"></a> H. A. Mahmoud, V. Arora, F. Nawab, D. Agrawal, A. El Abbadi. *MaaT: Effective and Scalable Coordination of Distributed Transactions in the Cloud*. *VLDB Endowment*, 7(5):329–340, 2014.  
12. <a id="ref-12"></a> G. Wood. *Ethereum: A Secure Decentralized Generalized Transaction Ledger (Yellow Paper)*. [https://ethereum.github.io/yellowpaper/paper.pdf](https://ethereum.github.io/yellowpaper/paper.pdf), 2018.  
13. <a id="ref-13"></a> E. Hildenbrandt, M. Saxena, X. Zhu, et al. *KEVM: A Complete Semantics of the Ethereum Virtual Machine*. [https://www.ideals.illinois.edu/handle/2142/97207](https://www.ideals.illinois.edu/handle/2142/97207), 2017.  
14. <a id="ref-14"></a> E. Androulaki, A. Barger, V. Bortnikov, et al. *Hyperledger Fabric: A Distributed Operating System for Permissioned Blockchains*. [https://arxiv.org/pdf/1801.10228v1.pdf](https://arxiv.org/pdf/1801.10228v1.pdf), 2018.  
15. <a id="ref-15"></a> Kubernetes Documentation. *Building Large Clusters*. [https://kubernetes.io/docs/admin/cluster-large/](https://kubernetes.io/docs/admin/cluster-large/).  
16. <a id="ref-16"></a> Red Hat. *OpenShift Container Platform: Cluster Limits*. [https://access.redhat.com/documentation/en-us/openshift_container_platform/3.9/html/scaling_and_performance_guide/](https://access.redhat.com/documentation/en-us/openshift_container_platform/3.9/html/scaling_and_performance_guide/).  
17. <a id="ref-17"></a> Red Hat Storage. *Container-Native Storage for the OpenShift Masses*. [https://redhatstorage.redhat.com/2017/10/05/containernative-storage-for-the-openshift-masses/](https://redhatstorage.redhat.com/2017/10/05/containernative-storage-for-the-openshift-masses/).  
18. <a id="ref-18"></a> Kubernetes GitHub Issue. *Increase Maximum Pods per Node: kubernetes/kubernetes#23349*. [https://github.com/kubernetes/kubernetes/issues/23349](https://github.com/kubernetes/kubernetes/issues/23349).  
19. <a id="ref-19"></a> OpenShift Blog. *Deploying 2048 OpenShift Nodes on the CNCF Cluster*. [https://blog.openshift.com/deploying-2048-openshift-nodes-cncf-cluster/](https://blog.openshift.com/deploying-2048-openshift-nodes-cncf-cluster/). See also: *Kubernetes Scalability Goals*. [https://github.com/kubernetes/community/blob/master/sig-scalability/goals.md](https://github.com/kubernetes/community/blob/master/sig-scalability/goals.md). 
20. <a id="ref-20"></a> G. Wood. *Ethereum: A Secure Decentralized Generalized Transaction Ledger (Yellow Paper)*. [https://ethereum.github.io/yellowpaper/paper.pdf](https://ethereum.github.io/yellowpaper/paper.pdf), 2018.  
21. <a id="ref-21"></a> T. Schmidt. *Modeling Energy Markets with Extreme Spikes*. In: A. Sarychev, A. Shiryaev, M. Guerra, M. R. Grossinho (eds). *Mathematical Control Theory and Finance*, pp. 359–375. Springer, Berlin, Heidelberg, 2008.  
22. <a id="ref-22"></a> W. Delbaen, F. Schachermayer. *A General Version of the Fundamental Theorem of Asset Pricing*. *Mathematische Annalen*, 300(1):463–520, 1994.  
23. <a id="ref-23"></a> Gridcoin. *Gridcoin Whitepaper: The Computation Power of a Blockchain Driving Science and Data Analysis*. [https://www.gridcoin.us/assets/img/whitepaper.pdf](https://www.gridcoin.us/assets/img/whitepaper.pdf).  
24. <a id="ref-24"></a> Golem Project Team. *The Golem Project Whitepaper*. [https://golem.network/doc/Golemwhitepaper.pdf](https://golem.network/doc/Golemwhitepaper.pdf), 2016.  
25. <a id="ref-25"></a> D. P. Anderson. *BOINC: A System for Public-Resource Computing and Storage*. [https://boinc.berkeley.edu/grid_paper_04.pdf](https://boinc.berkeley.edu/grid_paper_04.pdf), 2004.  
26. <a id="ref-26"></a> J. Blomer, L. Franco, A. Harutyunian, P. Mato, Y. Yao, C. Aguado Sanchez, P. Buncic. *CERNVM – A Virtual Software Appliance for LHC Applications*. [http://iopscience.iop.org/article/10.1088/1742-6596/219/4/042003/pdf](http://iopscience.iop.org/article/10.1088/1742-6596/219/4/042003/pdf), 2017.  
27. <a id="ref-27"></a> D. Lombraña González, et al. *LHC@Home: A Volunteer Computing System for Massive Numerical Simulations of Beam Dynamics and High-Energy Physics Events*. [http://inspirehep.net/record/1125350/](http://inspirehep.net/record/1125350/).  
