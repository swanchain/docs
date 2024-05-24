---
description: SwanChain Tokenomics and Governance Framework
---

# Tokenomics

## Overview

SwanChain introduces a comprehensive tokenomics structure designed to support its ecosystem's growth, incentivize participation, and ensure operational sustainability. This document outlines the strategic allocation, governance mechanisms, planned token release schedule, and specific formulas for the Foundation Community Pool (FCP), Edge Computing Providers (ECP), and Market Providers (MP) to provide a clear understanding of how SwanChain utilizes its native tokens to drive network engagement and development.&#x20;

### Total Token Supply

* **Total Supply:** 1,000,000,000 Swan Tokens

<figure><img src="../../.gitbook/assets/image (1).png" alt=""><figcaption><p>Swan Chain Tokenomics</p></figcaption></figure>

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

#### Calculation of MP Commission ($$MPC$$ )&#x20;

$$
MPC = TV × CR
$$

$$
MPC=1000SWAN ×0.05=50SWAN
$$

Where:

* $$TV$$ represents the Task Value.
* $$CR$$ represents the Commission Rate.

#### Example Monthly Income for MP

Assuming an MP manages 100 such tasks per month, the total monthly income from commissions ($$TMPC_{\text{Mon}}$$ ) would be:

$$
TMPC_{\text{Mon}} = \text{Number of Tasks} \times MPC
$$

$$
T\text{MPC}_{\text{Mon}} = 100 \times 50 \, \text{SWAN} = 5,000 \, \text{SWAN}
$$

#### Income from Computing Jobs for Providers

**Income Calculation**

The income for providers (FCPs and ECPs) is derived from successfully completing and delivering computing tasks. The income per task is determined by the auction process, where the provider with the winning bid executes the task.

#### Example Calculation for Provider Income

1. **Winning Bid:** Assume an FCP wins a bid for a computing task valued at 1000 Swan Tokens.
2. **Monthly Task Completion:** Assume the FCP completes 20 such tasks per month.

#### Calculation of Monthly Income for FCP

$$
FCP_{Mon}=\sum_{i}task_i× V_{i}
$$

$$
FCP_{Mon}=\sum_{i=1}^{20} task_{i}*V_{i}
$$

Where:

* $$FCP_{Mon}$$ represents the monthly income for the Fog Computing Provider (FCP).
* $$task$$ represents the number of tasks performed for in a month.
* $$V$$ represents the value of task.

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

*   **Collateral Requirement (**$$C_{FCP}$$ **):**

    $$
    C_{FCP}=\sum_{i}^{} J_{F}*JCF
    $$

    Where:

    * $$C_{FCP}$$ represents the Collateral Requirement for the FCP
    * $$J_{F}$$ represents the job handled by the FCP.
    * $$JCF$$ represents the Job Complexity Factor, which is a factor based on the complexity and resources required for the jobs.

#### Edge Computing Provider (ECP) Formula

*   **Collateral Requirement (**$$C_{ECP}$$ **):**

    $$
    C_{ECP} = \sum_{i}^{} J_{E}*JCF
    $$



    Where:

    * $$C_{ECP}$$ represents the Collateral Requirement for the ECP
    * $$J_{E}$$ represents the job handled by the ECP.
    * $$JCF$$ represents the Job Complexity Factor, which is a factor based on the complexity and resources required for the jobs.

#### Market Provider (MP) Formula

*   **Collateral Requirement  (**$$C_{MP}$$ **):**

    $$
    C_{MP}=200×(BC+CF)
    $$

    Where:

    * BC (Base Collateral): A fixed minimum collateral required
    * CF: Total transaction volume handled by the Market Provider

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

$$
UBI_{totalAannual}=CS_{swan}×RF_{annual }
$$

Where:

* $$UBI_{\text{totalAnnual}}$$ represents the Total Annual Universal Basic Income (UBI).
* $$CS_{\text{swan}}$$ represents the Circulating Supply of Swan tokens.
* $$RF_{\text{annual}}$$ represents the Annual Inflation Rate.

$$
UBI_{\text{totalAnnual}}=50,000,000×0.05=2,500,000 SWAN
$$

1. **Determine the allocation for each provider category:**
   *   **ECP Allocation:**

       $$
       ECP_{\text{Allocation}} = UBI_{\text{totalAnnual}} \times 0.40 = 2{,}500{,}000 \times 0.40 = 1{,}000{,}000 \text{ SWAN}
       $$
   *   **FCP Allocation:**

       $$
       FCP_{\text{Allocation}} = UBI_{\text{totalAnnual}} \times 0.40 = 2{,}500{,}000 \times 0.40 = 1{,}000{,}000 \text{ SWAN}
       $$
   *   **MP Allocation:**

       $$
       MP_{\text{Allocation}} =UBI_{\text{totalAnnual}} \times 0.20 = 2{,}500{,}000 \times 0.20 = 500{,}000 \text{ Swan Tokens}
       $$
2. **Calculate the UBI per provider in each category:**
   *   **UBI per ECP:**

       $$
       \text{UBI per ECP} = \frac{ECP_{\text{Allocation}}}{\text{Number of ECPs}} = \frac{1{,}000{,}000}{400} = 2{,}500 \text{ SWAN per ECP}
       $$
   *   **UBI per FCP:**

       $$
       \text{UBI per FCP} = \frac{FCP_{\text{Allocation}}}{\text{Number of FCPs}} = \frac{1{,}000{,}000}{100} = 10{,}000 \text{ Swan Tokens per FCP}
       $$
   *   **UBI per MP:**

       $$
       \text{UBI per MP} = \frac{MP_{\text{Allocation}}}{\text{Number of MPs}} = \frac{500{,}000}{3} \approx 166{,}666.67 \text{ Swan Tokens per MP}
       $$
3. **Calculate the monthly UBI per provider:**
   *   **Monthly UBI per ECP:**

       $$
       UBI_{\text{monthly}}\text{per ECP} = \frac{\text{UBI per ECP}}{12} = \frac{2{,}500}{12} \approx 208.33 \text{ SWAN}
       $$
   *   **Monthly UBI per FCP:**

       $$
       UBI_{\text{monthly}}\text{per FCP} = \frac{\text{UBI per FCP}}{12} = \frac{10{,}000}{12} \approx 833.33 \text{ SWAN}
       $$
   *   **Monthly UBI per MP:**

       $$
       UBI_{\text{monthly}}\text{per MP}= \frac{\text{UBI per MP}}{12} = \frac{166{,}666.67}{12} \approx 13{,}888.89 \text{ SWAN}
       $$
