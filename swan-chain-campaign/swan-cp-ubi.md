# Swan CP UBI

### Overview

Swan Chain's Universal Basic Income (UBI) program serves as a cornerstone of our provider incentivization system, designed to reward Computing Providers (CPs) who contribute to the network's robustness and functionality.

### Provider Types and Responsibilities

#### Edge Computing Providers (ECP)

ECPs play a crucial role in maintaining network integrity through:

* Daily Zero-Knowledge (ZK) validation tasks
* GPU task deployment operation
* Network performance maintenance
* Collateral management

#### Fog Computing Providers (FCP)

FCPs contribute to network functionality by:

* Executing daily sampling tasks for health verification
* Managing application deployment requests
* Maintaining required collateral and performance standards
* Processing diverse computational workloads

***

## Participation Process

### 1. Token Acquisition

#### 1.1 Obtaining ETH for Gas Fees

ETH is required for transaction gas fees. Follow these steps:

1. Visit Swan Chain's official bridge website:[ https://bridge.swanchain.io](https://bridge.swanchain.io)
2. Cross-chain your ETH to Swan Chain to obtain ETH
3. Prepare sufficient ETH to account for potential fluctuations in network gas fees

#### 1.2 Obtaining SWAN for Collateral

CP requires SWAN as collateral. You can purchase SWAN from centralized exchanges (CEXs).&#x20;

SWAN is currently listed on [Gate.io](https://www.gate.io/trade/SWAN_USDT), [MEXC](https://www.mexc.com/exchange/SWAN_USDT), and [LBank](https://www.lbank.com/trade/swan_usdt).&#x20;

Contract Address: 0xBb4eC1b56cB624863298740Fd264ef2f910d5564

### 2. Provider Setup

#### ECP Setup

1. Follow [ECP documentation](https://docs.swanchain.io/bulders/computing-provider/edge-computing-provider-ecp/ecp-setup)
2. Configure ZK validation systems
3. Prepare for GPU task capabilities (upcoming)

#### FCP Setup

1. Follow [FCP documentation](https://docs.swanchain.io/bulders/computing-provider/fog-computing-provider-fcp)
2. Set up sampling task infrastructure
3. Configure application deployment systems

### 3. Collateral Management

* Access [Provider Dashboard](https://provider.swanchain.io/overview) for deposits
* Dynamic collateral requirements based on network computing power
* Separate escrow accounts for ECP and FCP operations
* Locked collateral for UBI qualification and order acceptance

***

## Program Structure

**Continuous Rewards System**:

* The CP UBI program operates as an ongoing incentive mechanism
* Active providers continuously earn SWANU tokens based on their contributions

**Performance-Based**:

* Rewards scale with provider performance and resource contribution
* Immediate Activation: New providers start earning once setup and verification are complete

***

## Reward Distribution Mechanism

#### Earning Mechanism

Providers earn SWANU tokens through:

* **UBI Rewards**
  * Daily distribution based on:
    * GPU specifications and quantity
    * Bench task completion rates
    * Resource availability metrics
    * Service quality indicators
* **Paid Job Income**
  * Market-driven compensation for:
    * User-requested computational tasks
    * Application deployments (FCP)
    * GPU task deployment (ECP)

**Token Conversion**:

* All earned SWANU tokens convert 1:1 to SWAN tokens at TGE
* Direct conversion system replaces the previous pool-based distribution
* Providers know exactly how many SWAN tokens they'll receive at TGE

**Anti-Gaming Measures**:\
The system includes robust anti-gaming protections:

* **Resource Manipulation**: Using UBI-earned SWANU to create artificial tasks for your own resources is strictly prohibited. Such attempts will NOT increase your earnings.
* **Penalties**: Manipulation attempts will result in:
  * Reduced bench task success rates
  * Lower overall CP earnings
  * Potential disqualification from the program

***

### Additional Resources

#### Documentation

* [UBI Mechanism Details](https://docs.swanchain.io/core-concepts/protocol-stack/token/swan-universal-basic-income-ubi)
* [Provider Income Guide](https://docs.swanchain.io/core-concepts/token/swan-provider-income#conditions-for-cp-to-receive-ubi)
* [Provider Setup Guides](../bulders/computing-provider/)

#### Support Channels

* [Telegram CP commuity](https://t.me/swan_chain)
* [Discord CP channels](https://discord.gg/swanchain)

This documentation will be updated as new features and optimizations are introduced to the Swan Chain network.
