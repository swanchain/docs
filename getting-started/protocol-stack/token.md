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

### Token Flow Diagram

<figure><img src="../../.gitbook/assets/image (172).png" alt=""><figcaption></figcaption></figure>

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

### Income and Commission Structure for Computing Jobs Auction Engine and Market Providers (MP)

#### Computing Jobs Auction Engine

**Income Generation**

The computing jobs auction engine facilitates the distribution of computing tasks to providers (FCPs and ECPs) through a competitive bidding process. This system ensures that tasks are allocated efficiently, with providers offering the best rates and performance.

#### Commission Structure for Market Providers (MP)

Market Providers (MP) play a crucial role in this ecosystem by managing the auction process. They ensure that tasks are allocated fairly and efficiently to the most suitable providers. MPs earn commissions from the tasks auctioned through the following structure:

#### Commission Calculation

1. **Task Auction Commission Rate:** Let's assume the MP commission rate is set at 5% of the task value.
2. **Example Task Value:** Assume a computing task is valued at 1000 Swan Tokens.

#### Calculation of MP Commission

$$
MP Commission=Task Value×Commission RateMP Commission=Task Value×Commission Rate
$$

$$
MP Commission=1000 Swan Tokens×0.05=50 Swan TokensMP Commission=1000 Swan Tokens×0.05=50 Swan Tokens
$$

#### Example Monthly Income for MP

Assuming an MP manages 100 such tasks per month, the total monthly income from commissions would be:

$$
Total Monthly MP Commission=Number of Tasks×MP CommissionTotal Monthly MP Commission=Number of Tasks×MP Commission
$$

$$
Total Monthly MP Commission=100×50 Swan Tokens=5000 Swan TokensTotal Monthly MP Commission=100×50 Swan Tokens=5000 Swan Tokens
$$

#### Income from Computing Jobs for Providers

**Income Calculation**

The income for providers (FCPs and ECPs) is derived from successfully completing and delivering computing tasks. The income per task is determined by the auction process, where the provider with the winning bid executes the task.

#### Example Calculation for Provider Income

1. **Winning Bid:** Assume an FCP wins a bid for a computing task valued at 1000 Swan Tokens.
2. **Monthly Task Completion:** Assume the FCP completes 20 such tasks per month.

#### Calculation of Monthly Income for FCP

$$
Monthly Income for FCP=Number of Tasks×Task ValueMonthly Income for FCP=Number of Tasks×Task Value
$$

$$
Monthly Income for FCP=20×1000 Swan Tokens=20000 Swan TokensMonthly Income for FCP=20×1000 Swan Tokens=20000 Swan Tokens
$$



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

*   **Collateral Requirement (C\_FCP):**

    $$
    CFCP=100×(Number of Jobs+Job Complexity Factor)CFCP​=100×(Number of Jobs+Job Complexity Factor)
    $$

    Where:

    * Number of Jobs: Total number of jobs handled by the provider
    * Job Complexity Factor: A factor based on the complexity and resources required for the jobs

#### Edge Computing Provider (ECP) Formula

*   **Collateral Requirement (C\_ECP):**

    $$
    CECP=50×(Number of Jobs+Job Complexity Factor)
    $$

    Where:

    * Number of Jobs: Total number of jobs handled by the provider
    * Job Complexity Factor: A factor based on the complexity and resources required for the jobs

#### Market Provider (MP) Formula

*   **Collateral Requirement (C\_MP):**

    $$
    CMP=200×(Base Collateral+Transaction Volume/1000)
    $$

    Where:

    * Base Collateral: A fixed minimum collateral required
    * Transaction Volume: Total transaction volume handled by the Market Provider

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

1.  **Calculate the total annual UBI allocation:**

    $$
    Total Annual UBI=Circulating Supply×Annual Inflation Rate
    $$

    $$
    Total Annual UBI=50,000,000×0.05=2,500,000 Swan TokensTotal Annual UBI=50,000,000×0.05=2,500,000 Swan Tokens
    $$
2. **Determine the allocation for each provider category:**
   *   **ECP Allocation:**

       $$
       ECP Allocation=Total Annual UBI×0.40=2,500,000×0.40=1,000,000 Swan TokensECP Allocation=Total Annual UBI×0.40=2,500,000×0.40=1,000,000 Swan Tokens
       $$
   *   **FCP Allocation:**

       $$
       FCP Allocation=Total Annual UBI×0.40=2,500,000×0.40=1,000,000 Swan TokensFCP Allocation=Total Annual UBI×0.40=2,500,000×0.40=1,000,000 Swan Tokens
       $$
   *   **MP Allocation:**

       $$
       MP Allocation=Total Annual UBI×0.20=2,500,000×0.20=500,000 Swan TokensMP Allocation=Total Annual UBI×0.20=2,500,000×0.20=500,000 Swan Tokens
       $$
3. **Calculate the UBI per provider in each category:**
   *   **UBI per ECP:**

       $$
       UBI per ECP=ECP AllocationNumber of ECPs=1,000,000400=2,500 Swan Tokens per ECPUBI per ECP=Number of ECPsECP Allocation​=4001,000,000​=2,500 Swan Tokens per ECP
       $$
   *   **UBI per FCP:**

       $$
       UBI per FCP=FCP AllocationNumber of FCPs=1,000,000100=10,000 Swan Tokens per FCPUBI per FCP=Number of FCPsFCP Allocation​=1001,000,000​=10,000 Swan Tokens per FCP
       $$
   *   **UBI per MP:**

       $$
       UBI per MP=MP AllocationNumber of MPs=500,0003≈166,666.67 Swan Tokens per MPUBI per MP=Number of MPsMP Allocation​=3500,000​≈166,666.67 Swan Tokens per MP
       $$
4. **Calculate the monthly UBI per provider:**
   *   **Monthly UBI per ECP:**

       $$
       Monthly UBI per ECP=UBI per ECP12=2,50012≈208.33 Swan TokensMonthly UBI per ECP=12UBI per ECP​=122,500​≈208.33 Swan Tokens
       $$
   *   **Monthly UBI per FCP:**

       $$
       Monthly UBI per FCP=UBI per FCP12=10,00012≈833.33 Swan TokensMonthly UBI per FCP=12UBI per FCP​=1210,000​≈833.33 Swan Tokens
       $$
   *   **Monthly UBI per MP:**

       $$
       Monthly UBI per MP=UBI per MP12=166,666.6712≈13,888.89 Swan TokensMonthly UBI per MP=12UBI per MP​=12166,666.67​≈13,888.89 Swan Tokens
       $$
