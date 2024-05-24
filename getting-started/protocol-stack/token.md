---
description: SwanChain Tokenomics and Governance Framework
---

# Tokenomics

## Overview

SwanChain introduces a comprehensive tokenomics structure designed to support its ecosystem's growth, incentivize participation, and ensure operational sustainability. This document outlines the strategic allocation, governance mechanisms, planned token release schedule, and specific formulas for the Foundation Community Pool (FCP), Edge Computing Providers (ECP), and Market Providers (MP) to provide a clear understanding of how SwanChain utilizes its native tokens to drive network engagement and development.

### Total Token Supply

* **Total Supply:** 1,000,000,000 Swan Tokens

<figure><img src="../../.gitbook/assets/image (171).png" alt=""><figcaption></figcaption></figure>

### Token Allocation and Distribution Breakdown

#### 1. Investors (20%)

* **Purpose:** To fund the initial development and expansion phases of SwanChain.
* **Details:** Investors provide essential capital for foundational buildout and scaling operations. This funding supports technical development, operational infrastructure, and initial market strategies.

#### 2. DAO Treasury (20%)

* **Purpose:** To manage investments, cover legal costs, and other financial necessities.
* **Details:** The DAO Treasury is critical for financial management and operational expenditures. It is funded by contributions from market providers and is used for strategic investments and covering essential expenses such as legal fees.

#### 3. Ecosystem Fund (25%)

* **Purpose:** To support ecosystem growth through grants, projects, and other initiatives.
* **Details:** This fund is crucial for nurturing innovation within the SwanChain ecosystem, providing financial support to developers, startups, and community projects that contribute to the network’s enhancement.

#### 4. Core Contributors (15%)

* **Purpose:** To reward and retain the developers and team members who are integral to the development and maintenance of SwanChain.
* **Details:** This allocation ensures that the developers and personnel central to SwanChain’s operations are motivated and retained, facilitating ongoing innovation and network maintenance.

#### 5. Airdrops (20%)

* **Purpose:** To incentivize broad network participation and engagement.
* **Details:** Airdrops distribute Swan tokens widely across the community to stimulate network activity and growth, enhancing user adoption and community vibrancy.

### Governance and Incentives

#### Governance Participation

* **Staking Requirement:** Community members must stake tokens to participate in governance decisions.
* **Governance Rewards:** Active participants may receive rewards, such as 1% of the DAO Treasury, to incentivize involvement and wise decision-making.

#### Annual Inflation and UBI

* **Annual Inflation Rate:** Ranges between 5% and 10%, determined by the governance team.
* **Universal Basic Income (UBI):** Tokens are distributed as UBI to eligible network participants, promoting sustained engagement and rewarding contributions.

### Collateral and Slashing Mechanism

#### Collateral Requirements

To ensure commitment and mitigate potential risks associated with fraudulent activities, all providers on the network must post collateral to participate. There are three types of providers:

**Fog Computing Provider (FCP)**

* **Role:** Offers a layered network extending cloud capabilities to the edge, providing services like AI model training and deployment.
* **Job-Based Collateral:** Required to stake a significant amount of tokens based on the number and complexity of jobs they handle.

**Edge Computing Provider (ECP)**

* **Role:** Focuses on processing data for near real-time applications at the network’s edge, such as IoT sensor data analysis and image recognition.
* **Job-Based Collateral:** Required to stake a substantial amount of tokens based on the number and complexity of jobs they handle.

**Market Provider (MP)**

* **Role:** Facilitates the allocation of computing jobs to providers using an auction engine for job distribution and a payment engine for financial transactions.
* **Job-Based Collateral:** Required to stake a large amount of tokens based on the number and volume of transactions they manage.

#### Slashing Mechanism

* **Fraud Detection:** If a provider’s computing job is proven fraudulent, the collateral is subject to a 2x slashing.
* **Token Burn:** The slashed tokens are sent to a burning address, effectively removing them from circulation and penalizing dishonest behavior.

### Token Flow Diagram

![](https://your-image-url)

<figure><img src="../../.gitbook/assets/swanchain.png" alt=""><figcaption><p>Token Flow Diagram</p></figcaption></figure>

#### Explanation:

* **Purchase Swan Products:** Users purchase products and pay in Swan ETH.
* **Match Compute Demand:** Demand is matched with supply via Proof-of-Work auction matching.
* **Fee Collection:** 75% of fees collected go to the Treasury, and 25% of fees go to Buyback.
* **Slashing:** Compute providers and auction engine validators can be slashed.
* **Staking:** Providers stake $SWAN to provide compute and participate in auction automation.
* **Rewards:** Rewards are paid in SWAN tokens to compute providers and auction engine validators.
* **Treasury Management:** DAO controls the treasury and allocates funds accordingly.

### Formulas for Collateral and Reward Calculation

#### Fog Computing Provider (FCP) Formula

* **Collateral Requirement (C\_FCP):**&#x20;

$$
C_{FCP} = 100 \times (vCPU + \frac{SSD_storage}{100} + \frac{memory}{10} + \frac{bandwidth}{10} + GPU)
$$

* &#x20;Where:
  * vCPU: Number of virtual CPUs
  * SSD\_storage: SSD storage in GB
  * memory: Memory in GB
  * bandwidth: Bandwidth in MB/s
  * GPU: Number of GPUs

#### Edge Computing Provider (ECP) Formula

* **Collateral Requirement (C\_ECP):**&#x20;

$$
C_{ECP} = 50 \times (vCPU + \frac{HDD_storage}{100} + \frac{memory}{10} + \frac{bandwidth}{10} + GPU)
$$

&#x20;Where:

* vCPU: Number of virtual CPUs
* HDD\_storage: HDD storage in GB
* memory: Memory in GB
* bandwidth: Bandwidth in MB/s
* GPU: Number of GPUs

#### Market Provider (MP) Formula

* **Collateral Requirement (C\_MP):**&#x20;

$$
C_{MP} = 200 \times (\text{Base Collateral} + \frac{\text{Transaction Volume}}{1000})
$$

Where:

* Base Collateral: A fixed minimum collateral required
* Transaction Volume: Total transaction volume handled by the Market Provider

### Sample Collateral Calculation for FCP and ECP

#### Fog Computing Provider (FCP) Collateral Calculation

#### Sample Hardware Configuration for FCP

* **vCPUs:** 8
* **SSD Storage:** 100 GB
* **Memory:** 64 GB
* **Bandwidth:** 50 MB/s
* **GPUs:** 2 (e.g., RTX 3080)

#### Collateral Requirement Formula

$$
CFCP=100×(vCPU+SSD_storage100+memory10+bandwidth10+GPU)CFCP​=100×(vCPU+100SSD_storage​+10memory​+10bandwidth​+GPU)
$$

#### Calculation Steps

1. **Calculate each component:**
   * vCPUs: $$8×100=8008×100=800$$
   * SSD Storage: $$100100×100=100100100​×100=100$$
   * Memory: $$6410×100=6401064​×100=640$$
   * Bandwidth: $$5010×100=5001050​×100=500$$
   * GPUs: $$2×100=2002×100=200$$
2.  **Sum all components:**

    $$
    CFCP=800+100+640+500+200=2240 Swan TokensCFCP​=800+100+640+500+200=2240 Swan Tokens
    $$

### Example Collateral for FCP:

$$Collateral for FCP=2240 Swan TokensCollateral for FCP=2240 Swan Tokens$$

#### Edge Computing Provider (ECP) Collateral Calculation

#### Sample Hardware Configuration for ECP

* **vCPUs:** 4
* **HDD Storage:** 300 GB
* **Memory:** 32 GB
* **Bandwidth:** 20 MB/s
* **GPUs:** 1 (e.g., RTX 3080)

#### Collateral Requirement Formula

$$
CECP=50×(vCPU+HDD_storage100+memory10+bandwidth10+GPU)CECP​=50×(vCPU+100HDD_storage​+10memory​+10bandwidth​+GPU)
$$

#### Calculation Steps

1. **Calculate each component:**
   * vCPUs: $$4×50=2004×50=200$$
   * HDD Storage: $$300100×50=150100300​×50=150$$
   * Memory: $$3210×50=1601032​×50=160$$
   * Bandwidth: $$2010×50=1001020​×50=100$$
   * GPUs: $$1×50=501×50=50$$
2.  **Sum all components:**

    $$
    CECP=200+150+160+100+50=660 Swan TokensCECP​=200+150+160+100+50=660 Swan Tokens
    $$

#### Example Collateral for ECP:

$$Collateral for ECP=660 Swan TokensCollateral for ECP=660 Swan Tokens$$

#### Summary

* **Fog Computing Provider (FCP) Collateral:** 2240 Swan Tokens
* **Edge Computing Provider (ECP) Collateral:** 660 Swan Tokens

### UBI Distribution Example

To illustrate how SwanChain's Universal Basic Income (UBI) system works with a 5% annual inflation rate and a governance-determined allocation, we will provide examples using the given parameters:

#### Parameters:

* **Annual Inflation Rate:** 5%
* **Circulating Supply for the First Year:** 50,000,000 Swan Tokens
* **UBI Allocation:**
  * 40% to Edge Computing Providers (ECP)
  * 40% to Fog Computing Providers (FCP)
  * 20% to Market Providers (MP)

#### Providers:

* **Number of ECPs:** 400
* **Number of FCPs:** 100
* **Number of MPs:** 3

#### Step-by-Step Calculation:

1. **Calculate the total annual UBI allocation:**

$$\text{Total Annual UBI} = \text{Circulating Supply} \times \text{Annual Inflation Rate}$$&#x20;

$$
\text{Total Annual UBI} = 50,000,000 \times 0.05 = 2,500,000 \text{ Swan Tokens}
$$

1.  **Determine the allocation for each provider category:**

    * **ECP Allocation:**&#x20;

    $$\text{ECP Allocation} = \text{Total Annual UBI} \times 0.40 = 2,500,000 \times 0.40 = 1,000,000 \text{ Swan Tokens}$$

* **FCP Allocation:** $$\text{FCP Allocation} = \text{Total Annual UBI} \times 0.40 = 2,500,000 \times 0.40 = 1,000,000 \text{ Swan Tokens}$$
* **MP Allocation:** $$\text{MP Allocation} = \text{Total Annual UBI} \times 0.20 = 2,500,000 \times 0.20 = 500,000 \text{ Swan Tokens}$$

1. **Calculate the UBI per provider in each category:**
   * **UBI per ECP:** $$\text{UBI per ECP} = \frac{\text{ECP Allocation}}{\text{Number of ECPs}} = \frac{1,000,000}{400} = 2,500 \text{ Swan Tokens per ECP}$$
   * **UBI per FCP:** $$\text{UBI per FCP} = \frac{\text{FCP Allocation}}{\text{Number of FCPs}} = \frac{1,000,000}{100} = 10,000 \text{ Swan Tokens per FCP}$$
   * **UBI per MP:** $$\text{UBI per MP} = \frac{\text{MP Allocation}}{\text{Number of MPs}} = \frac{500,000}{3} \approx 166,666.67 \text{ Swan Tokens per MP}$$
2.  **Calculate the monthly UBI per provider:**

    * **Monthly UBI per ECP:** $$\text{Monthly UBI per ECP} = \frac{\text{UBI per ECP}}{12} = \frac{2,500}{12} \approx 208.33 \text{ Swan Tokens}$$
    * **Monthly UBI per FCP:** $$\text{Monthly UBI per FCP} = \frac{\text{UBI per FCP}}{12} = \frac{10,000}{12} \approx 833.33 \text{ Swan Tokens}$$
    * **Monthly UBI per MP:** $$\text{Monthly UBI per MP} = \frac{\text{UBI per MP}}{12} = \frac{166,666.67}{12} \approx 13,888.89 \text{ Swan Tokens}$$

