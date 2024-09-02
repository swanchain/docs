# Market Provider (MP)

Participants can submit your code that facilitates the allocation of computing jobs to providers, utilizing an auction engine for job distribution and a payment engine for financial transactions.

### **How to Participate**

To participate, follow these steps:

1. **Submission**: Submit your code to the [GitHub repository](https://github.com/swanchain/market-providers) provided.
2. **Review Criteria**: Ensure your code meets the [Minimum Viable Product (MVP) criteria and other requirements](market-provider-mp.md#minimum-viable-product-mvp) listed below.
3. **Fill Out the Form:** Remember to fill out this [form](https://docs.google.com/forms/d/e/1FAIpQLSctZrpy\_VlSMO0-XSKFK-rVDGoeEoxnQCHwqOuQxPMIq-6E-w/viewform?usp=sf\_link) when you finish this task. Code submission form will require:
   * Source Code Repository
   * Contract Code
   * Deployment Documentation
   * MP Operational Website
   * APIs Documentation

### **Rules and Acceptance Criteria**

#### **Minimum Viable Product (MVP)**

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

#### Evaluation Process <a href="#evaluation-process" id="evaluation-process"></a>

Submissions will be evaluated based on:

* Compliance with MVP criteria and additional requirements.
* Quality and readability of the code.
* Thoroughness of deployment documentation.
* Clarity and completeness of API documentation.
