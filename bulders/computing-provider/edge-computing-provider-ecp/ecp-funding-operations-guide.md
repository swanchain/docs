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
  * [Task Types and Rewards](ecp-funding-operations-guide.md#id-4.1-task-types-and-rewards)
  * [Reward Distribution](ecp-funding-operations-guide.md#id-4.2-reward-distribution)
  * [Viewing Rewards](ecp-funding-operations-guide.md#id-4.3-viewing-rewards)
  * [Slash Mechanism](ecp-funding-operations-guide.md#id-4.4-slash-mechanism)
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

#### 1.2 Obtaining SwanC for Collateral

A minimum of 100 SwanC (Swan Credit Token) is required as collateral. To obtain SwanC:

1. Visit Swan Chain's faucet website: [https://faucet.swanchain.io](https://faucet.swanchain.io/)
2. Claim 25 SwanC per transaction
3. Multiple claims are allowed, but each claim incurs a gas fee
4. It's recommended to claim as much SwanC as possible in one go to ensure sufficient collateral for tasks

### 2. Account Setup

Depositing  SwanC to the Collateral Account: Use the following command:

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

When using the sequencer, pre-fund the CP's sequencer account with ETH on Swan Chain. Each task requires 0.00001 ETH as gas:

```
computing-provider sequencer add --from <WALLET_ADDRESS> --account <CP_ACCOUNT> <Amount>
```

_**Note**: The sequencer periodically processes tasks and deducts from the sequencer account. If the balance is insufficient during settlement, it may become negative. ECPs need to refund to receive new tasks._

### 4. Task Execution and Rewards

#### 4.1 Task Types and Rewards

* fil-512M-CPU: 5 SwanC/task, 5-minute completion time
* fil-512M-GPU: 10 SwanC/task, 5-minute completion time
* fil-32G-GPU: 15 SwanC/task, 20-minute completion time

_**Note**: Each task must be completed and submitted within the specified time._

#### 4.2 Reward Distribution

Rewards are distributed during the next settlement cycle after 24 hours, typically within 48 hours of task proof submission, to the beneficiary account.

#### 4.3 Viewing Rewards

Check the transaction list of the beneficiary account on the blockchain explorer: [https://swanscan.io](https://swanscan.io).

Alternatively, you can use the CP command to view rewards:

```
computing-provider ubi list
```

The result will look like this:

```
TASK ID TASK CONTRACT                             TASK TYPE ZK TYPE    STATUS  REWARD SEQUENCER CREATE TIME         
724273 0xe4402f76D37Fb708650a5a8357e411ADfd0ac874 GPU      fil-c2-512M rewarded 2.00  YES      2024-07-29 11:19:57
724286 0xe4402f76D37Fb708650a5a8357e411ADfd0ac874 GPU      fil-c2-512M rewarded 2.00  YES      2024-07-29 11:42:17
735375 0xe4402f76D37Fb708650a5a8357e411ADfd0ac874 GPU      fil-c2-32G  rewarded 15.0  YES      2024-07-29 12:04:01
735382 0xe4402f76D37Fb708650a5a8357e411ADfd0ac874 GPU      fil-c2-32G  rewarded 15.0  YES      2024-07-29 12:57:50
735390 0xe4402f76D37Fb708650a5a8357e411ADfd0ac874 GPU      fil-c2-32G  rewarded 15.0  YES      2024-07-29 13:27:50
735381 0xe4402f76D37Fb708650a5a8357e411ADfd0ac874 GPU      fil-c2-32G  rewarded 15.0  YES      2024-07-29 13:57:50
735376 0xe4402f76D37Fb708650a5a8357e411ADfd0ac874 GPU      fil-c2-32G  rewarded 15.0  YES      2024-07-29 14:27:50
735380 0xe4402f76D37Fb708650a5a8357e411ADfd0ac874 GPU      fil-c2-32G  rewarded 15.0  YES      2024-07-29 14:57:50
```

#### 4.4 Slash Mechanism

If a task proof fails validation (late submission, invalid proof, etc.), 5 SwanC will be deducted during the periodic settlement.

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

\\
