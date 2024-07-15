# FCP Token Operations Guide

## Introduction

This guide provides detailed instructions on how to manage token-related operations required for Fog Computing Providers (FCP) in the Swan Chain network. Follow this guide to ensure you have the necessary tokens and collateral for your tasks.

### 1. Preparation

#### 1.1 Obtaining SETH for Gas Fees

FCP requires `SETH` as transaction gas fees. Follow these steps to get `SETH`:

* Visit Swan Chain's official bridge website: [bridge.swanchain.io](https://bridge.swanchain.io)
* Cross-chain your `ETH` to Swan Chain to get `SETH`.
* It is recommended to prepare enough `SETH` to account for fluctuations in network gas fees.

#### 1.2 Obtaining SwanC for Collateral

FCP requires `SwanC` (Swan Credit Token) as collateral. Follow these steps to obtain `SwanC`:

* Visit Swan Chain's faucet website: [faucet.swanchain.io](https://faucet.swanchain.io)
* You can claim 24 `SwanC` per transaction.
* Multiple claims are allowed, but each claim requires a gas fee.
* It is recommended that you apply for as many `SwanC` as possible at one time to ensure you have enough collateral for tasks.

### 2. Collateral Management

#### 2. Collateral Management

**2.1 Collateral Requirements**

* Each task requires 5 `SwanC` as collateral.
* Collateral must be associated with the corresponding FCP account address.
* It is recommended to maintain enough collateral to ensure you can always receive tasks.

**2.2 Adding Collateral**

* Visit the FCP deployment document for detailed steps: [Collateral Setup Guide](computing-provider-setup.md#collateral-swanc-for-fcp)
* Enter the amount of `SwanC` you wish to add as collateral (at least 5 `SwanC`).

**2.3 Withdrawing Collateral**

* Visit the FCP [FAQ ](https://docs.swanchain.io/swan-provider/cp-computing-provider/fcp-fog-computing-provider/faq)for detailed steps on withdrawing collateral: [Withdrawal Guide](computing-provider-setup.md#withdraw-swanc-from-fcp)
* Enter the amount of `SwanC` you wish to withdraw.

> **Note:**&#x20;
>
> Withdrawals may be affected by network congestion and could take some time.

**2.4 Collateral Release**

* Collateral for each task will be released after the task is completed and confirmed to have no disputes.
* Due to network gas fluctuations, there may be delays in the release of collateral.

**2.5 Slash Collateral (coming soon)**

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

### 3. Reward Mechanism&#x20;

#### 3.1 Task Completion Reward

* FCPs can earn rewards after completing tasks.
* Users are billed hourly based on the resources provided by the FCP.
* Different configurations have different pricing.

#### 3.2 Revenue Distribution (coming soon)

* The revenue distribution for each deployment is as follows:
  * The platform takes a 5% service fee.
  * The remaining 95% is divided among up to three FCPs (task processors).

### 4. Important Notes

* Always ensure you have enough `SETH` for gas fees.
* Maintain sufficient `SwanC` collateral to continuously receive and complete tasks.
* Stay updated with platform announcements to be aware of the latest changes in collateral and reward policies.
* If you encounter any issues, join the official [Discord](https://discord.com/invite/swanchain) community for support or open a ticket for 1-on-1 assistance.

***
