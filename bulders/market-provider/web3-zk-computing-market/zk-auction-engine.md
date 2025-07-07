# ZK Auction Engine

### 1. Introduction

The **ZK Computing Market** is a crucial component of the Swan Chain ecosystem, serving as the primary Market Provider (MP) for Zero-Knowledge (ZK) computations. One of its sub-components, the **ZK Sequencer**, plays a pivotal role in processing tasks and proofs.

### 2.  ZK Engine Component

**1. Functions**

* **Task Collection and Assignment**: The ZK Engine collects and assigns ZK tasks to appropriate CPs based on available resources.
* **Proof Validation and Settlement:** It validates proofs submitted to the Swan Chain, handling the financial settlement, including rewards and penalties.

**2. ZK Sequencer**

* **Proof Management:** The Sequencer verifies and batches proofs into blobs, storing them in the Swan IPFS Storage and creating unique identifiers (CIDs). **This step minimizes gas costs for the network**.
* **Collateral Management:** It ensures CPs have sufficient collateral and handles batch settlement of tasks, including reward distribution, slashing, and gas payments.
* **Data Integrity:** The Sequencer ensures the integrity of task data, checking for modifications and confirming the authenticity of proofs. It also manages the CID creation process and records it on the Swan Chain.

### 3. Workflow

<figure><img src="../../../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

1. Task Assignment and Pooling:
   * The ZK Engine aggregates various ZK tasks (such as FIL-C2-512M, FIL-C2-32G, ALEO, etc.) into the ZK Tasks Pool.
2. Proof Submission by ECPs:
   * Edge Computing Providers (ECPs) receive tasks from the ZK Tasks Pool.
   * After completing the required computations, ECPs generate proofs for the completed tasks.
   * ECPs have two options for submitting proofs:
     * Direct Submission: Proofs are submitted directly to the Swan Chain, incurring standard gas fees.
     * Submission via Sequencer: Proofs are submitted through the Sequencer, which aggregates the proofs, potentially reducing gas fees due to the more efficient batching and storage process.
3. Proof Processing and Verification:
   * Proof Engine: Processes the submitted proofs. It verifies the proofs for correctness.
   * If proof is valid, the process moves forward to collateral checking; if invalid, it may lead to penalties.
4. Collateral Checking:
   * The system checks whether the ECP has sufficient collateral locked.
   * This check determines the outcome for the ECP based on proof validity and collateral status.
5. Outcome Determination:
   * Reward Distribution: If the proof is valid and the collateral is sufficient, the Reward Engine processes rewards for the ECP.
   * Slashing: If the proof is invalid or collateral is insufficient, the Slash Engine applies penalties, potentially deducting funds from the ECP's CP Account.
6. Sequencer Operations:
   * For proofs submitted via the Sequencer, the Sequencer aggregates the proofs into blobs.
   * These blobs are stored in the Swan IPFS Storage, which optimizes gas costs and ensures data integrity.
7. Swan Chain Integration:
   * The Sequencer or direct submission mechanisms create an AggregateTask entry on the Swan Chain, which records the aggregated proofs and associated data.
   * The Swan Chain manages collateral, GAS fees, rewards, and slashing information, maintaining transparency and accountability.
8. Batch Settlement:
   * The ZK Engine periodically settles all submitted and verified proofs.
   * This settlement process includes distributing rewards to ECPs or imposing penalties for invalid proofs or inadequate collateral.

### 5. Interaction with External Modules

#### 1. Swan Chain Contracts

* Collateral Contract:
  * Manages CP stakes
  * Handles locking and unlocking of collateral
  * Executes slashing when necessary
* AggregatorTask Contract:
  * Records task blob CIDs on-chain
  * Provides a transparent, immutable record of processed tasks
* TaskRegister Contract:
  * Handles task registration and tracking

#### 2. Swan IPFS Storage&#x20;

* Stores aggregated task data as blobs
* Generates unique CIDs for each stored blob
* Provides efficient retrieval of task data when needed

#### 3.  Filecoin Network

* Acts as a backup storage solution for Swan IPFS Storage data
* Ensures long-term data availability and redundancy
