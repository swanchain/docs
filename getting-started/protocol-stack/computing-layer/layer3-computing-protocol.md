---
description: A Layer3 designed for high performance AI computing
---

# Layer3 Computing Protocol

**Introduction**

The Layer3 Computing Protocol aims to aggregate computing jobs submission with reduced gas fees, ensuring efficient data management and scalability. This protocol utilizes IPLD for data storage, generates unique CIDs, and employs a Sequencer for batching and submitting tasks to Swan Chain.

**Key Components**

1. **Data Storage and CID Generation**
2. **Submission of Blob CIDs**
3. **Role of the Sequencer**
   * Receiving Proofs
   * Validating Proofs
   * Batching Proofs
   * Submitting Data
   * Gas Fee Management

#### Detailed Process Flow

**1. Data Storage and CID Generation**

* **Step 1:** Data related to computing tasks and results are stored using the IPLD format.
* **Step 2:** Unique CIDs are generated for these data entries.

**2. Submission of Blob CIDs**

* **Step 1:** Generated CIDs are submitted to smart contracts as blobs.
* **Step 2:** This ensures long-term persistence of job data and results.

**3. Role of the Sequencer**

<figure><img src="../../../.gitbook/assets/image (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

* **Step 1:** CPs submit proofs of completed tasks to the Sequencer.
* **Step 2:** Sequencer validates the received proofs.
* **Step 3:** Valid proofs are batched together by the Sequencer.
* **Step 4:** Sequencer submits the batched data to Swan Chain.
* **Step 5:** AggregateTask contracts are created with blob CIDs.
* **Step 6:** Sequencer manages gas fees, maintaining separate accounts for gas costs.

#### Benefits

* **Reduced Gas Costs:** By batching tasks and submitting them as aggregated blobs, gas costs are minimized, making the system more efficient and cost-effective.
* **Efficient Data Management:** Using IPLD and unique CIDs ensures that data is stored efficiently and can be easily retrieved and validated.
* **Secure and Scalable:** The Sequencer enhances security by validating proofs and aggregating tasks, allowing for scalable and secure job submissions to the Swan Chain.



The Layer3 Computing Protocol provides a robust solution for decentralized computing by optimizing job submissions, reducing gas fees, and ensuring efficient data management. By leveraging IPLD for data storage and employing a Sequencer for validation and batching, the protocol enhances security and scalability, making it a key component of the Swan Chain ecosystem.

#### Example Use Case

* **AI/ML Workloads:** Aggregate multiple AI/ML model training tasks and submit them as a single batch to reduce gas costs.
* **Data Archiving:** Use IPLD to store large datasets, generating unique CIDs for efficient retrieval and long-term persistence.
* **Content Delivery:** Manage and aggregate content delivery tasks, ensuring secure and scalable distribution across the network.

#### Glossary

* **IPLD:** InterPlanetary Linked Data, a format for storing and referencing data.
* **CID:** Content Identifier, a unique identifier for data stored in IPLD.
* **Sequencer:** A component that receives, validates, batches, and submits tasks to the blockchain.
* **CP:** Computing Provider, an entity that provides computing resources and submits proofs of completed tasks.
* **AggregateTask:** A contract on Swan Chain that references aggregated task data using blob CIDs.
