# Decentralized AI Marketplace

The Decentralized Auction Marketplace is an innovative system designed to enable efficient and secure interactions between users and providers in a decentralized environment. This system utilizes smart contracts, a bidding engine, and decentralized storage to create a transparent and efficient marketplace for various tasks.

Providers in the system contribute their resources, such as storage, network bandwidth, CPU, and GPU, to offer services to users who publish tasks on the platform. These tasks are stored on decentralized storage like IPFS/Filecoin, ensuring data security and transparency.

The bidding engine manages a competitive bidding process, where providers with different resource capabilities bid on tasks. Smart contracts on the Ethereum blockchain govern the bidding rules, enforce fair competition, and ensure the security of transactions.

Upon completion of a task by the selected provider, the final results are uploaded to the decentralized storage, maintaining data integrity and tamper-proofing. The smart contract then facilitates the automatic transfer of rewards from the task publisher to the provider as compensation for their services.

In summary, the Decentralized Auction Marketplace offers a secure and efficient platform for users to outsource tasks to a global network of providers with various resources. By leveraging blockchain technology, smart contracts, and decentralized storage, the system ensures transparency, security, and fairness in the bidding process and the execution of tasks.

### Key concepts

**Bidder (Provider)**: A bidder, also known as a provider, is a participant in the marketplace who offers their services to complete tasks. These services may include storage, network bandwidth, CPU, and GPU resources. Bidders are typically composed of blockchain nodes worldwide, and they actively participate in the competitive bidding process to secure tasks. Once assigned a task, bidders work to complete it to the best of their abilities, and the one with the best performance is rewarded by the task publisher.

**Publisher (User)**: A publisher, also known as a user, is an individual or organization that creates tasks in the marketplace. Publishers are responsible for defining the tasks, providing necessary details, and setting the rewards. They rely on the Decentralized Auction Marketplace to find suitable providers (bidders) to complete their tasks. Once the bidding process is complete and a provider has successfully delivered the task, the publisher rewards the provider through the smart contract system.

**Decentralized Storage**: Decentralized storage systems like IPFS/Filecoin are used to store tasks and their associated data. This ensures that the data remains secure, transparent, and tamper-proof, while also allowing for easy access by authorized parties.

**Bidding Engine**: The bidding engine manages the competitive bidding process, where providers bid on tasks based on their resource capabilities and offered prices. This component ensures that tasks are allocated to providers in a fair and efficient manner.

**Smart Contracts**: Smart contracts are self-executing contracts on the EVM blockchain that govern the bidding rules, enforce fair competition, and ensure the security of transactions. They facilitate the automatic transfer of rewards from users to providers upon task completion and manage other aspects of the bidding process.

## Auction Engine

The auction engine is a critical component of the LagrangeDAO system. It manages the bidding process for tasks, ensuring that tasks are assigned to the most suitable computing providers. Here's a breakdown of its key functionalities:

1. **Load Provider Pool**: The auction engine initially loads all active computing providers into a pool. These providers are potential bidders for tasks.
2. **Place Bid**: When a task is open for bidding, the auction engine allows a computing provider (bidder) to place a bid on the task. The bid is only successful if the task is currently accepting bids, the bidder has not already placed a bid, and the bidder's collateral is sufficient.
3. **Load Tasks from Redis**: The auction engine fetches all tasks from Redis that are in a state where they can accept bids. It also handles state transitions for tasks, such as moving a task from the 'accepting\_bids' state to the 'bidding\_closed' state when the bidding period ends.
4. **Select Bidders**: The auction engine selects bidders based on certain criteria. For example, it might select the bidders with the highest collateral.
5. **Run Bidding Process**: For each task that is open for bidding, the auction engine runs the bidding process. It allows the selected bidders to place their bids on the task.
6. **List Tasks Available for Bidding**: The auction engine can provide a list of all tasks that are currently open for bidding.

The auction engine is designed to be fair and efficient, ensuring that tasks are distributed evenly among computing providers and that the bidding process is competitive. It plays a crucial role in the operation of the LagrangeDAO network.

&#x20;The data structure for each task in the platform includes:

* uuid: A unique identifier for the task.
* status: The current status of the task (e.g., open, closed, in progress, completed).
* task\_detail\_cid: A content identifier for the task details, stored on a decentralized storage system like IPFS.
* type: The type or category of the task.
* reference\_id: A reference ID for linking related tasks or resources.
* name: The name or title of the task.
* leading\_job\_id: The ID of the job currently in the leading processing status, used for tracking purposes.
* created\_at: The timestamp when the task was created.
* updated\_at: The timestamp when the task was last updated.
* user\_id: The ID of the user who created the task.

When a user publishes a task, multiple providers (blockchain nodes worldwide) can bid on the task. The Bidding Engine evaluates these bids and assigns the task to several bidders with the potential to complete the task effectively. Once they complete the task, the Bidding Engine assesses the quality of their work, updating the leading\_job\_id as necessary to keep track of the best-performing bidder.

Finally, the provider who delivers the highest-quality work is marked as successful and receives a reward from the task publisher. By employing this mechanism, the Bidding Engine promotes efficiency and transparency in the Decentralized Bidding Marketplace, ensuring that tasks are matched with the most suitable providers and completed to the highest standards.

### Autobid

The Decentralized Bidding Marketplace can be configured to include an auto-bid mode for providers, which allows them to automatically participate in all bids without manual intervention. This feature can be particularly useful for providers who want to streamline their bidding process and maximize their chances of securing tasks.

To enable the auto-bid mode, providers need to set up their capability and resource availability for bidding. This information includes the type of resources they can offer (such as storage, network bandwidth, CPU, and GPU), their capacity for each resource, and any other relevant details that may impact their ability to complete tasks.

When auto-bid mode is enabled, the Bidding Engine automatically pushes tasks to the provider based on their configured capabilities and resource availability. The Bidding Engine evaluates the provider's suitability for each task and includes their bid in the competitive bidding process. This automatic participation ensures that providers have a constant presence in the marketplace and can secure tasks that match their expertise and resources.

## Task State Machine

The bidding task state machine is a system designed to manage the bidding process for tasks, taking into account task details such as price and timeout. In this setup, each task allows a maximum of three bidders to compete simultaneously, with each bidder being assigned a job to complete.

Bidders have the ability to set a limit on the number of bids they can process at the same time. This feature prevents them from accepting new bids once they reach their specified limit, enabling bidders to effectively manage their workload and participate in multiple tasks without overextending themselves.

<figure><img src="../../../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

### States

The Bidding State Machine has several predefined states:

1. **created**: This is the initial state when a task is first created. The task stays in this state until bidding is opened.
2. **accepting\_bids**: In this state, the task is open for bidders to place their bids. The task remains in this state until bidding is closed, the bid is cancelled, or the bid fails.
3. **bidding\_closed**: This state indicates that the bidding process for the task has ended. The task transitions to this state from the 'accepting\_bids' state. From here, the task can either be marked as 'submitted' or 'failed'.
4. **submitted**: This state signifies that the task has been submitted successfully. The task moves to this state from the 'bidding\_closed' state. Once a task is in the 'submitted' state, it can then be completed.
5. **completed**: This is the final state indicating that the task has been completed successfully. The task transitions to this state from the 'submitted' state.
6. **failed**: This state indicates that the task has failed. The task can enter this state from the 'bidding\_closed' state. If a task fails, it can be reset to the 'created' state.
7. **cancelled**: This state signifies that the bid for the task has been cancelled. The task can enter this state from the 'accepting\_bids' state. If a bid is cancelled or fails, the task can be reset to the 'created' state.

The transitions between these states are managed by the state machine, which ensures that the task moves through its lifecycle in a controlled and predictable manner.

* `open_bidding`: Transition from Created to Accepting\_Bids.
* `close_bidding`: Transition from Accepting\_Bids to Processing.
* `cancel_bid`: Transition from Accepting\_Bids to Cancelled.
* `failed_bids`: Transition from Accepting\_Bids to Cancelled.
* `complete_task`: Transition from Submitted to Completed.
* `mark_as_submitted`: Transition from Processing to Submitted.
* `mark_as_failed`: Transition from Processing to Failed.
* `reset_accepting_bids_to_created`: Transition from Accepting\_Bids to Created.
* `reset_failed_bids_to_created`: Transition from Failed to Created.

The Bidding State Machine has defined transitions between states:

### Transition Between States

1. open\_bidding: Transition from 'Created' to 'Accepting\_Bids'.
2. close\_bidding: Transition from 'Accepting\_Bids' to 'Processing'.
3. cancel\_bid: Transition from 'Accepting\_Bids' to 'Cancelled'.
4. failed\_bids: Transition from 'Accepting\_Bids' to 'Cancelled'.
5. complete\_task: Transition from 'Submitted' to 'Completed'.
6. mark\_as\_submitted: Transition from 'Processing' to 'Submitted'.
7. mark\_as\_failed: Transition from 'Processing' to 'Failed'.
8. reset\_accepting\_bids\_to\_created: Transition from 'Accepting\_Bids' to 'Created'.
9. reset\_failed\_bids\_to\_created: Transition from 'Failed' to 'Created'.

### Rules

The bidding task state machine should include the following rules:

* A bidder cannot place a bid if they have exceeded their limit on the number of jobs they can process simultaneously.
* Once a bidder has completed a job, they cannot be assigned any further jobs on the same task.

If the task is cancelled, all bids and jobs associated with the task are cancelled as well
