# FCP Funding Operations Guide

## Introduction

This guide provides detailed instructions on how to manage token-related operations required for Fog Computing Providers (FCP) in the Swan Chain network. Follow this guide to ensure you have the necessary tokens and collateral for your tasks.

### 1. Preparation

#### 1.1 Obtaining ETH for Gas Fees

FCP requires `ETH` as transaction gas fees. Follow these steps to get `ETH`:

* Visit Swan Chain's official bridge website: [bridge.swanchain.io](https://bridge.swanchain.io)
* Cross-chain your `ETH` to Swan Chain to get `Swan ETH`.
* It is recommended to prepare enough `ETH` to account for fluctuations in network gas fees.

#### 1.2 Obtaining  SWAN  for Collateral

FCP requires SWAN as collateral, you can buy it from CEXs.

SWAN is currently listed on [Gate.io](https://www.gate.io/trade/SWAN_USDT), [MEXC](https://www.mexc.com/exchange/SWAN_USDT), and [LBank](https://www.lbank.com/trade/swan_usdt).&#x20;

Contract Address: 0xBb4eC1b56cB624863298740Fd264ef2f910d5564

### 2. Collateral Management

#### 2.1 Collateral Requirements

* Collateral amounts dynamically adjust based on network computing power.&#x20;
  * Review our[ comprehensive collateral documentation](https://docs.swanchain.io/core-concepts/token/computing-provider-collateral/collateral-requirement-and-earning-multiplier) for detailed information
* Monitor the [Swan dashboard](https://provider.swanchain.io/overview) for computing units and base collateral trends(upcoming feature)
* Maintain sufficient collateral to ensure continuous task eligibility

**2.2 Slash Collateral**&#x20;

To maintain network performance and accountability, CPs are subject to a precise slashing mechanism that penalizes inefficient or unreliable computing services. For each failed task, CPs face graduated penalties:

* Fog Computing Providers (FCP) lose 0.1% of their current full collateral amount per failed task (approximately 3.533 SWAN for a 3080 GPU), with around 14 tasks processed daily.

If a CP's collateral amount falls below the required threshold, they become ineligible to receive Universal Basic Income (UBI) tasks. To mitigate the risk of unexpected task exclusion, CPs are advised to maintain a buffer in their collateral amount.

More details about Slash Collateral, check here: [https://docs.swanchain.io/core-concepts/token/computing-provider-collateral#slashing-mechanism](https://docs.swanchain.io/core-concepts/token/computing-provider-collateral#slashing-mechanism)

### 3. Reward Mechanism

#### 3.1 Task Completion Reward

* FCPs can earn rewards after completing tasks.
* Users are billed hourly based on the resources provided by the FCP.
* Different configurations have different pricing, check the price [here](https://docs.lagrangedao.org/spaces/space-settings/space-hardware).

#### 3.2 Revenue Distribution (coming soon)

* The revenue distribution for each deployment is as follows:
  * The platform takes a 5% service fee.
  * The remaining 95% is divided among up to three FCPs (task processors).

***
