# ECP Funding Operations Guide

### Introduction

**ECP (Edge Computing Provider)** is a crucial component in the Swan Chain ecosystem and is responsible for executing Zero-Knowledge (ZK) computation tasks. The Sequencer is an optimization component aggregating proofs submitted by multiple ECPs, processing them in batches to reduce gas fees and improve overall efficiency.

### Account Overview

* **CP Account (Computing Provider Account)**: An independent on-chain contract account for CP operations and fund management. FCP and ECP have the same account structure.
  * **Owner Address**: Hold the highest authority to manage the CP account.
  * **Beneficiary Address**: Receives rewards for ZK tasks.
  * **Worker Address**: Used for submitting proofs.
* **Collateral Account**: ECP's collateral account in the collateral contract (SwanC).
  * **Escrow Account**: A account for storing ECP's collateral in collateral contarct, ensuring that ECP can receive ZK tasks.
* **Sequencer Account**: ECP's account in the sequencer contract, used to pay for gas fees required when submitting ZK proofs.

### Table of Content:

* [Initial Setup](ecp-funding-operations-guide.md#id-1.-initial-setup)
  * [Obtaining ETH for Gas Fees](ecp-funding-operations-guide.md#id-1.1-obtaining-eth-for-gas-fees)
  * [Obtaining SwanC for Collateral](ecp-funding-operations-guide.md#id-1.2-obtaining-swanc-for-collateral)
* [Account Setup](ecp-funding-operations-guide.md#id-2.-account-setup)
* [Configuring Sequencer (Recommended)](ecp-funding-operations-guide.md#id-3.-configuring-sequencer-recommended)
  * [Enabling Sequencer Functionality](ecp-funding-operations-guide.md#id-3.1-enabling-sequencer-functionality)
  * [Funding the Sequencer Account](ecp-funding-operations-guide.md#id-3.2-funding-the-sequencer-account)
* [Task Execution and Rewards](ecp-funding-operations-guide.md#id-4.-task-execution-and-rewards)
  * [Reward Distribution](ecp-funding-operations-guide.md#id-4.1-reward-distribution)
  * [Viewing UBI tasks](ecp-funding-operations-guide.md#id-4.2-viewing-ubi-tasks)
  * [Slash Mechanism](ecp-funding-operations-guide.md#id-4.3-slash-mechanism)
* [Exit Procedure](ecp-funding-operations-guide.md#id-5.-exit-procedure)
  * [Stop Receiving New Tasks](ecp-funding-operations-guide.md#id-5.1-stop-receiving-new-tasks)
  * [Data Backup](ecp-funding-operations-guide.md#id-5.2-data-backup)
  * [Withdrawal Process](ecp-funding-operations-guide.md#id-5.3-withdrawal-process)

### 1. Initial Setup

#### 1.1 Obtaining ETH for Gas Fees

ETH is required for transaction gas fees. Follow these steps:

1. Visit Swan Chain's official bridge website:[ https://bridge.swanchain.io](https://bridge.swanchain.io)
2. Cross-chain your ETH to Swan Chain to obtain ETH
3. Prepare sufficient ETH to account for potential fluctuations in network gas fees

#### 1.2 Obtaining SWAN for Collateral

ECP requires SWAN as collateral, you can buy from CEXs after TGE.

#### 1.3 Collateral Requirements

* Collateral amounts dynamically adjust based on network computing power.&#x20;
  * Review our[ comprehensive collateral documentation](https://docs.swanchain.io/core-concepts/token/computing-provider-collateral/collateral-requirement-and-earning-multiplier) for detailed information
* Monitor the [Swan dashboard](https://provider.swanchain.io/overview) for computing units and base collateral trends(upcoming feature)
* Maintain sufficient collateral to ensure continuous task eligibility

> **During CP UBI-0 stage:**
>
> * Collateral will be directly deposited to corresponding escrow account. **No manual deposit required.**
> * Funds locked for UBI qualification and order acceptance. **No withdrawal option available**

### 2. Account Setup <a href="#id-2.-account-setup" id="id-2.-account-setup"></a>

Depositing SWANU to the Collateral Account: Use the following command:

```
computing-provider collateral add --ecp --from <WALLET_ADDRESS> --account <CP_ACCOUNT> <Amount>
```

### 3. Configuring Sequencer (Recommended)

#### 3.1 Enabling Sequencer Functionality

In the ECP configuration file, set `EnableSequencer = true` and `autoChainProof = true`

When `autoChainProof` is true, ECP will prioritize submitting proofs via the sequencer. If the sequencer lacks sufficient gas, it will automatically submit proofs on-chain. If set to false, ECP won't submit tasks when the sequencer lacks gas.

{% hint style="info" %}
_Note: ECPs can submit proofs in two ways:_

* _Directly to Swan Chain as a contract (higher gas consumption per transaction)_
* _To the zk-sequencer for batch aggregation (lower gas consumption, recommended)_
{% endhint %}

#### 3.2 Funding the Sequencer Account

When using the sequencer, pre-fund the CP's sequencer account with ETH on Swan Chain. Currently, the gas is decided by the [Dynamic Pricing Strategy](https://docs.swanchain.io/bulders/market-provider/web3-zk-computing-market/sequencer). For more detailed information, see [here](https://docs.swanchain.io/swan-provider/market-provider-mp/zk-engine/sequencer).

```
computing-provider sequencer add --from <WALLET_ADDRESS> --account <CP_ACCOUNT> <Amount>
```

_**Note**: The sequencer periodically processes tasks and deducts from the sequencer account. If the balance is insufficient during settlement, it may become negative. ECPs need to refund to receive new tasks._

### 4. Task Execution and Rewards

#### 4.1 Reward Distribution

Rewards are distributed during the next settlement cycle after 24 hours following the [CP UBI-0](../../../swan-chain-campaign/swan-cp-ubi.md) event rules, typically within 48 hours of task proof submission, to the beneficiary account.

Read [swan-provider-income.md](../../../core-concepts/token/swan-provider-income.md "mention") to learn more.

#### 4.2 Viewing UBI tasks

Check the transaction list of the beneficiary account on the blockchain explorer: [https://swanscan.io](https://swanscan.io).

Alternatively, you can use the CP command to view UBI tasks:

```
computing-provider ubi list
```

The result will look like this:

```
TASK ID  TASK CONTRACT                               TASK TYPE  ZK TYPE      STATUS    REWARD  SEQUENCER  CREATE TIME         
1081868  0x23AC299fA44aa7Df2ddDFE09cDB0331DD1945043  GPU        fil-c2-32G   verified  0.00    YES        2024-11-03 10:40:31  
1081072  0x23AC299fA44aa7Df2ddDFE09cDB0331DD1945043  GPU        fil-c2-32G   verified  0.00    YES        2024-11-03 11:10:30  
1081749  0x23AC299fA44aa7Df2ddDFE09cDB0331DD1945043  GPU        fil-c2-32G   verified  0.00    YES        2024-11-03 11:40:33  
1081904  0x23AC299fA44aa7Df2ddDFE09cDB0331DD1945043  GPU        fil-c2-32G   verified  0.00    YES        2024-11-03 12:10:34  
1081748  0x23AC299fA44aa7Df2ddDFE09cDB0331DD1945043  GPU        fil-c2-32G   verified  0.00    YES        2024-11-03 13:10:26  
1081562  0x23AC299fA44aa7Df2ddDFE09cDB0331DD1945043  GPU        fil-c2-32G   verified  0.00    YES        2024-11-03 13:40:29  
1081980  0x008E2334B34737D42128d13Ee50A78322663d7a7  GPU        fil-c2-32G   verified  0.00    YES        2024-11-03 14:10:31  
1081861  0x008E2334B34737D42128d13Ee50A78322663d7a7  GPU        fil-c2-32G   received  0.00    YES        2024-11-03 14:40:27  
```

#### 4.3 Slash Mechanism

If a task proof fails validation (late submission, invalid proof, etc.), 5 SWANU will be deducted during the periodic settlement.

### 5. Exit Procedure

#### 5.1 Stop Receiving New Tasks

```
computing-provider account changeTaskTypes --ownerAddress=<OWNER_ADDRESS> 0
```

#### 5.2 Data Backup

Backup the CP\_PATH directory (default: \~/.swan/computing)

#### 5.3 Withdrawal Process

**a) Withdraw from collateral account:**

```
computing-provider collateral withdraw --ecp --owner=<OWNER_ADDRESS> <AMOUNT>
```

**b) Withdraw from sequencer account:**

```
computing-provider sequencer withdraw --owner=<OWNER_ADDRESS> <AMOUNT>
```

**c) Withdraw from escrow account:**

* Initiate withdrawal request:

```
computing-provider collateral withdraw-request --owner=<OWNER_ADDRESS> --account=<CP_ACCOUNT> <AMOUNT>
```

* Confirm withdrawal after 48 hours:

```
computing-provider collateral withdraw-confirm --owner=<OWNER_ADDRESS> --account=<CP_ACCOUNT>
```

_**Note:** Escrow account balance may fluctuate due to periodic settlements. It's advisable to wait 24 hours after changing `taskTypes` before requesting the withdrawal. Confirmation can only be made 48 hours after the initial request. Funds will be withdrawn to the CP's `ownerAddress` upon confirmation._
