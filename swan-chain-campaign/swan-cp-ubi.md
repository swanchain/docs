# Swan CP UBI

### Overview

Swan Chain's Universal Basic Income (UBI) program serves as a cornerstone of our provider incentivization system, designed to reward Computing Providers (CPs) who contribute to the network's robustness and functionality.

### Provider Types and Responsibilities

#### Edge Computing Providers (ECP)

ECPs play a crucial role in maintaining network integrity through:

* Daily Zero-Knowledge (ZK) validation tasks
* Mining operation
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

* **Token**: SWANU (Contract: 0x39cBBeaF88a91404618d45a16e0977Adab4d1Af1)
* **Existing Providers**:
  * Automatic SWANU collateral deposit based on GPU specifications
  * Required upgrade to [Computing Provider v0.7.1](https://github.com/swanchain/go-computing-provider/releases/tag/v0.7.1)
  * Status verification via `computing-provider info` command
* **New Providers**: Submit application through official form for SWANU allocation here:[https://docs.google.com/forms/d/e/1FAIpQLSdnd8H4ab1eBr0D4e2QBLvBRj6H\_xo7C8gW8ItewvHJRzYVVg/viewform?usp=sf\_link](https://docs.google.com/forms/d/e/1FAIpQLSdnd8H4ab1eBr0D4e2QBLvBRj6H\_xo7C8gW8ItewvHJRzYVVg/viewform?usp=sf\_link)

### 2. Provider Setup

#### ECP Setup

1. Follow [ECP documentation](https://docs.swanchain.io/bulders/computing-provider/edge-computing-provider-ecp/ecp-setup)
2. Configure ZK validation systems
3. Prepare for mining capabilities (upcoming)

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
    * Mining operations (ECP)

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

* [Telegram CP commuity](https://t.me/swan\_chain)
* [Discord CP channels](https://discord.gg/swanchain)

This documentation will be updated as new features and optimizations are introduced to the Swan Chain network.
