# Swan CP UBI-0

## Overview

Swan Chain's Universal Basic Income (UBI-0) program represents the inaugural phase of our innovative provider incentivization system. This comprehensive guide details the program's structure, participation requirements, and reward mechanisms.

### Program Details

* **Duration**: October 29, 2024 - November 29, 2024 (EST)
* **Total Reward Pool**: 1,200,000 Swan tokens
* **Eligible Participants**: Edge Computing Providers (ECPs) and Fog Computing Providers (FCPs)

### Provider Types and Responsibilities

#### Edge Computing Providers (ECP)

ECPs play a crucial role in maintaining network integrity through:

* Daily Zero-Knowledge (ZK) validation tasks
* Future mining operations (upcoming feature)
* Network performance maintenance
* Collateral management

**Important Update**: ZK proof submission costs have been optimized to 0.0000005 ETH per proof, representing a 95% reduction from previous rates.

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
  * Required upgrade to [Computing Provider v0.7.0](https://github.com/swanchain/go-computing-provider/releases/tag/v0.7.0)
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

## Reward Distribution Mechanism

The final reward distribution for Computing Providers (CPs) in the UBI-0 program is calculated based on the following mechanism:

### Final Reward Calculation

The program will track all SWANU tokens received by each CP during the campaign period (Oct 29, 2024 - Nov 29, 2024). This includes:

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
    * Mining operations (ECP, upcoming)

{% hint style="info" %}
**Important**: Collateral amounts are NOT included in the calculation
{% endhint %}

#### Total Earnings Calculation Formula

```
CP's Share of 1.2M Swan = (CP's Total SWANU Earnings / All CPs' Total SWANU Earnings) × 1,200,000
```

### Critical Requirements

#### Beneficiary Address Management

1. **DO NOT** modify your beneficiary address during the campaign period
2. **DO NOT** transfer SWANU tokens from your beneficiary address
3. **MAINTAIN** the same beneficiary address throughout the entire program

{% hint style="danger" %}
&#x20;Any changes to your beneficiary address or unauthorized SWANU transfers may significantly impact your final reward calculation.
{% endhint %}

### Anti-Gaming Measures

The system includes robust anti-gaming protections:

1. **Resource Manipulation**
   * Using UBI-earned SWANU to create artificial tasks for your own resources is **strictly prohibited**
   * Such attempts will **NOT** increase your earnings
2. **Penalties**
   * Manipulation attempts will result in:
     * Reduced bench task success rates
     * Lower overall CP earnings
     * Potential disqualification from the program

### Best Practices

To maximize your legitimate earnings:

1. Maintain consistent resource availability
2. Focus on genuine user tasks
3. Keep your beneficiary address stable
4. Retain all SWANU earnings during the campaign period

The most effective strategy is to maintain genuine, high-quality service provision rather than attempting to manipulate the system.

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
