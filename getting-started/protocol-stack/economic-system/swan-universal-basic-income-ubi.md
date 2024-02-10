---
description: Swan UBI is designed for Computing Provider's base benefit
---

# Universal Basic Income (UBI)

#### **1. Introduction**

The Universal Basic Income (UBI) model within the Swan Chain ecosystem represents a groundbreaking approach to ensuring equitable compensation for participants contributing computational resources. This model is designed to provide a basic level of income to all eligible computing providers within the network, thereby fostering a more inclusive and sustainable community. Here's a summary of the key aspects of Swan Chain's UBI model:

#### 2. Purpose and Vision

* **Inclusivity and Fair Compensation**: The UBI model is motivated by the desire to create a fair and equitable ecosystem where contributors are guaranteed a baseline compensation for their participation, irrespective of the fluctuating demand for computing tasks.
* **Encouraging Participation**: By assuring a basic income, Swan Chain aims to encourage more individuals and organizations to contribute their computing resources to the network, enhancing the ecosystem's computational capacity and diversity.

#### **3. UBI Eligibility**

**3.1 Hardware Performance**

* Providers must meet a minimum hardware performance threshold to qualify for UBI.
* The performance can be benchmarked using algorithms like zk-proof, ensuring that the provider's hardware is capable of supporting the ecosystem's computational needs.

**3.2 Proof of Ownership**

* Providers must demonstrate proof of ownership of their hardware resources.
* This proof should be posted on-chain, ensuring transparency and verifiability.

#### **4. UBI Calculation**

**4.1 Base UBI**

Every qualified provider in the Swan ecosystem is eligible to receive a base UBI amount.

**Equation**:&#x20;

$$
UBI_{base} = V_{ubi}
$$



Where:

* ( UBI\_{base} ) is the base UBI for the provider.
* ( V\_{ubi} ) is the value of the UBI in Swan Tokens.

**4.2 Job Income Deduction**

If a provider earns income from jobs, this income will be deducted from their UBI. If their job income exceeds the average UBI, they will not receive any UBI.

**Equation**:



$$
UBI_{net} = UBI_{base} - I_{job}
$$

Where:

* ( UBI\_{net} ) is the net UBI after deducting job income.
* ( I\_{job} ) is the income earned from jobs.

**Condition**:&#x20;



$$
if ( I_{job} ) >= ( UBI_{avg} ), then ( UBI_{net} ) = 0
$$

Where:

* ( UBI\_{avg} ) is the average UBI income.

#### **5. Funding UBI**

The funds for UBI can be sourced from:

* A portion of the fees collected from job creators.
* A reserve fund set up by Swan during its initial token offering or fundraising.
* Voluntary contributions from larger providers or stakeholders.

#### **6. Periodic Review**

The UBI model, eligibility criteria, and funding sources should be reviewed periodically. This ensures that the system remains fair, sustainable, and aligned with the evolving needs of the Swan ecosystem.

#### **6. Conclusion**

Swan Chain's UBI model is more than a financial mechanism; it's a commitment to fostering a robust, equitable, and dynamic ecosystem. By ensuring fair compensation for computing providers, Swan Chain not only enhances its own network but also sets a precedent for the broader blockchain and AI industries, showcasing how technology can be leveraged to create more inclusive economic models.
