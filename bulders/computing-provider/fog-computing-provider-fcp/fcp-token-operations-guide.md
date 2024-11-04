# FCP Funding Operations Guide

## Introduction

This guide provides detailed instructions on how to manage token-related operations required for Fog Computing Providers (FCP) in the Swan Chain network. Follow this guide to ensure you have the necessary tokens and collateral for your tasks.

### 1. Preparation

#### 1.1 Obtaining ETH for Gas Fees

FCP requires `ETH` as transaction gas fees. Follow these steps to get `ETH`:

* Visit Swan Chain's official bridge website: [bridge.swanchain.io](https://bridge.swanchain.io)
* Cross-chain your `ETH` to Swan Chain to get `Swan ETH`.
* It is recommended to prepare enough `ETH` to account for fluctuations in network gas fees.

#### 1.2 Obtaining  SWANU  for Collateral

FCP requires `SWANU` (Swan Compute Unit) as collateral.&#x20;

* **Token**: SWANU  Contract: 0x39cBBeaF88a91404618d45a16e0977Adab4d1Af1
* **Existing Providers**:
  * Automatic SWANU collateral deposit based on GPU specifications
  * Required upgrade to [Computing Provider v0.7.0](https://github.com/swanchain/go-computing-provider/releases/tag/v0.7.0)
  * Status verification via `computing-provider info` command
* **New Providers**: Submit application through official form for SWANU allocation here:[https://docs.google.com/forms/d/e/1FAIpQLSdnd8H4ab1eBr0D4e2QBLvBRj6H\_xo7C8gW8ItewvHJRzYVVg/viewform?usp=sf\_link](https://docs.google.com/forms/d/e/1FAIpQLSdnd8H4ab1eBr0D4e2QBLvBRj6H\_xo7C8gW8ItewvHJRzYVVg/viewform?usp=sf\_link)

### 2. Collateral Management

#### 2.1 Collateral Requirements

* Collateral amounts dynamically adjust based on network computing power.&#x20;
  * Review our[ comprehensive collateral documentation](https://docs.swanchain.io/core-concepts/token/computing-provider-collateral/collateral-requirement-and-earning-multiplier) for detailed information
* Monitor the [Swan dashboard](https://provider.swanchain.io/overview) for computing units and base collateral trends(upcoming feature)
* Maintain sufficient collateral to ensure continuous task eligibility

> **During CP UBI-0 stage:**
>
> * Collateral will be directly deposited to corresponding escrow account. **No manual deposit required.**
> * Funds locked for UBI qualification and order acceptance. **No withdrawal option available**

**2.2 Slash Collateral**&#x20;

* Collateral may be forfeited under the following circumstances:
  * User complaints or appeals:
    * Can be submitted during or after the task.
    * Evidence must be provided (e.g., screenshots, log files, etc.).
    * Reasons for complaints/appeals may include but are not limited to:
      * Task completion does not meet expectations.
      * Service quality issues.
      * Other violations of service agreements.
  * Handling process:
    * The platform will review the complaint/appeal upon receipt.
    * If FCP is found to be at fault:
      * The platform will slash the corresponding collateral (equal to the task collateral amount).
      * The task reward will also be forfeited.

### 3. Reward Mechanism

#### 3.1 Task Completion Reward

* FCPs can earn rewards after completing tasks.
* Users are billed hourly based on the resources provided by the FCP.
* Different configurations have different pricing, check the price [here](https://docs.lagrangedao.org/spaces/space-settings/space-hardware).

#### 3.2 Revenue Distribution (coming soon)

* The revenue distribution for each deployment is as follows:
  * The platform takes a 5% service fee.
  * The remaining 95% is divided among up to three FCPs (task processors).

### 4. Important Notes

#### Critical Requirements

* Maintain stable beneficiary address during UBI-0 program
* Do not transfer SWANU tokens unnecessarily
* Keep sufficient ETH for gas fees

#### Best Practices

* Regular monitoring of collateral requirements
* Maintain high task completion rates
* Ensure consistent resource availability
* Monitor dashboard for performance metrics

#### Anti-Gaming Protection

* Do not use UBI earnings for artificial task creation
* Manipulation attempts will reduce bench task success rates
* Focus on legitimate service provision

***
