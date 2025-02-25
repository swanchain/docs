# CP Acceleration Program

The Computing Provider Acceleration Program is an exciting event designed to attract more computing providers to the Swan Chain network, boosting its overall computing power and capabilities. This program is part of the larger Atom Accelerator Race, which aims to drive ecosystem growth and adoption before the mainnet launch.

## Duration

April 29th , 00:00 (EST) - July 15, 23:59 (EST)

## How to Participate

### As an ECP (Edge Computing Provider)

* Participants will need to deploy a ECP according to the instructions provided and follow this [documentation](https://docs.swanchain.io/orchestrator/as-a-computing-provider/ecp-edge-computing-provider)[ ](https://docs.swanchain.io/orchestrator/as-a-computing-provider/ecp-edge-computing-provider/ecp-setup)to complete ZK tasks. Credits will be earned based on their performance in ZK jobs.

{% hint style="info" %}
**ECP hardware requirements:**

* Possess a public IP
* Have at least one GPU&#x20;
* At least 4 vCPUs&#x20;
* Minimum 300GB HDD storage&#x20;
* Minimum 32GB memory&#x20;
* Minimum 20MB bandwidth
{% endhint %}

### As an FCP (Fog Computing Provider)

* Participants will be required to deploy a FCP following the [documentation.](https://docs.swanchain.io/orchestrator/as-a-computing-provider/fcp-fog-computing-provider/computing-provider-setup)
* The longer their GPUs and CPUs are utilized, and the higher their uptime, the more credits they'll earn.
* **Boost:** Using high-density GPUs such as Nvidia H100s, A100s, or top consumer models can yield a substantial 3-10x acceleration in reward calculations.
  * High-density GPU Boost Details
    * RTX 4090: 3X
    * H100: 4X
    * A100: 4X
    * A6000: 4X
    * A10: 4X
    * A6000: 4X
    * H800: 6X
    * L40: 7X
    * L40S: 8X
    * H200: 10X

{% hint style="info" %}
It's important to note that this enhancement doesn't directly translate to a 3-10x increase in SWAN credits rewards; rather, it amplifies your contribution by the same factor.
{% endhint %}

{% hint style="info" %}
**FCP hardware requirements:**

* Possess a public IP
* Have a domain name (\*.example.com)
* Have an SSL certificate
* Have at least one GPU&#x20;
* At least 8 vCPUs&#x20;
* Minimum 100GB SSD storage
* Minimum 64GB memory
* Minimum 50MB bandwidth
{% endhint %}

### As a Market Provider:

Participants can submit your code that facilitates the allocation of computing jobs to providers, utilizing an auction engine for job distribution and a payment engine for financial transactions.

#### How to Participate

To participate, follow these steps:

1. **Submission**: Submit your code to the [GitHub repository](https://github.com/swanchain/market-providers) provided.
2. **Review Criteria**: Ensure your code meets the [Minimum Viable Product (MVP) criteria and other requirements](cp-acceleration-program.md#rules-and-acceptance-criteria) listed below.
3. **Fill Out the Form:** Remember to fill out this [form](https://forms.gle/2NitUKKsZyCg2Ntq9) when you finish this task. Code submission form will require:
   * Source Code Repository
   * Contract Code
   * Deployment Documentation
   * MP Operational Website
   * APIs Documentation

#### Rules and Acceptance Criteria

**Minimum Viable Product (MVP)**

Your code must include at least the following functionalities:

1. **Network Scan**: Scan the entire network for CP accounts, retrieve basic CP information, and display available CPs.
2. **Resource Information**: Retrieve detailed CP resource information.
3. **Whitelist**: Retrieve and manage the whitelist.
4. **Task Management**: Send tasks (including AI tasks and ZK-tasks) to CPs, with options to renew or terminate tasks.
5. **Task Lifecycle Management**: Ensure complete management of the task lifecycle.
6. **Collateral Functionality**: Implement CP-compatible collateral functionality.
7. **Payment Mechanism**: Establish a comprehensive payment mechanism involving User -> MP -> CP transactions.
8. **Platform Integration**: Integrate seamlessly with the LagrangeDAO platform.

**Code and Deployment**

* **Source Code**: Provide the complete source code.
* **Deployment Documentation**: Include a full deployment document, ensuring both frontend and backend services are operational.

**Collateral Functionality**

* **Collateral Contract**: Submit the source code for the collateral contract.

**APIs Documentation**

* **Comprehensive Documentation**: Provide detailed documentation for platform integration with the Market Provider, including API details and parameter descriptions.

### Evaluation Process

Submissions will be evaluated based on:

* Compliance with MVP criteria and additional requirements.
* Quality and readability of the code.
* Thoroughness of deployment documentation.
* Clarity and completeness of API documentation.

### Key Components

To fully understand and participate in the Computing Provider Acceleration Program, it's important to familiarize yourself with the following key components:

* FCP (Fog Computing Provider)
* ECP (Edge Computing Provider)
* MP (Market Provider)
* ZK task

[Click here](https://docs.swanchain.io/getting-started/protocol-stack/glossary) to access the detailed explanation and get started.

### Rules and Eligibility

* The program is open to individuals and organizations globally.
* Participants must comply with all applicable laws and regulations in their respective jurisdictions.
* Swan Chain reserves the right to modify or terminate the program at any time, with or without notice.
* Rewards will be distributed in the form of credits.
