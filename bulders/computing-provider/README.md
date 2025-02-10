# Computing Provider

A Computing Provider (CP) is a third-party individual or organization that offers scalable computing resources, which businesses can access on demand over Swan network. These resources include cloud-based compute, storage, platform, and application services.

As a resource provider, you can run a **ECP** (Edge Computing Provider) and **FCP** (Fog Computing Provider) to contribute yourcomputing resource.

* **ECP (Edge Computing Provider)** specializes in processing data at the source of data generation, using minimal latency setups ideal for real-time applications. This provider handles specific, localized tasks directly on devices at the network’s edge, such as IoT devices. At the current stage, ECP supports the generation of **ZK-Snark proof of Filecoin network**, and more ZK proof types will be gradually supported, such as Aleo, Scroll, starkNet, etc. Check  [Install Guideline](https://docs.swanchain.io/bulders/computing-provider/edge-computing-provider-ecp/ecp-setup) here.
* **FCP (Fog Computing Provider)** Offers a layered network that extends cloud capabilities to the edge of the network, providing services such as AI model training and deployment. This provider utilizes infrastructure like Kubernetes (K8S) to support scalable, distributed computing tasks. **FCP** will execute tasks assigned by Market Provider, like [Orchestrator](https://orchestrator.swanchain.io/) on the [Swan chain](https://swanchain.io/). Check [Install Guideline](https://docs.swanchain.io/bulders/computing-provider/fog-computing-provider-fcp/computing-provider-setup) here.

For the current status of Swan Provider Network, please check here: [https://provider.swanchain.io](https://provider.swanchain.io/overview)

### **Address Types**

A CP account has three different wallet addresses to ensure security and separation of responsibilities:

* `ownerAddress`: This is the owner account of the CP account. The owner has permission to change account information such as the multi-address, worker address, and beneficiary address. In most situations, the private key of the `ownerAddress` does not need to be present on the server for security reasons.
* `workerAddress`: This is the actual working address used for submitting proofs (`submitProof`). It needs to be funded with a certain amount of ETH to pay for gas fees when submitting proofs.
* `beneficiaryAddress`: This is the address where all earnings from the CP account will be sent. It is solely used for receiving funds. For security purposes, the private key of the `beneficiaryAddress` should not be stored on the server to maintain isolation.

By separating these address, the system ensures that only the necessary `workerAddress` private key is present on the server, while the more sensitive `ownerAddress` and `beneficiaryAddress` private keys are kept separate, enhancing the overall security of the system.

## **Exit Procedure for a Computing Provider (CP)**

If a CP wishes to exit and stop providing computing services, two steps are required:

### **Step 1**

Set the `taskTypes` to exit status (taskTypes = 100). The specific command is:

Execute the following command:

```
computing-provider --repo <YOUR_CP_REPO> account changeTaskTypes --ownerAddress <YOUR_OWNER_ADDRESS> 100
```

### **Step 2**

Withdraw collateral from both **Collateral and Escrow accounts.** The amount in the **Collateral account** can be directly withdrawn, while the amount in the **Escrow account** requires 7 days to complete the withdrawal. The process is as follows:

#### **For an ECP:**

To withdraw from the **Collateral account:**

```
computing-provider --repo <YOUR_CP_REPO> collateral withdraw --ecp --owner <YOUR_WALLET_ADDRESS> --account <YOUR_CP_ACCOUNT> <amount>
```

To withdraw from the **Escrow account:**

Submit withdrawal request:

```
computing-provider --repo <YOUR_CP_REPO> collateral withdraw-request --ecp --owner <YOUR_OWNER_ADDRESS> <AMOUNT>
```

Confirm withdrawal (after 7-day waiting period):

```
computing-provider --repo <YOUR_CP_REPO> collateral withdraw-confirm --ecp --owner <YOUR_OWNER_ADDRESS>
```

#### For an FCP:

To withdraw from the **Collateral account:**

```
computing-provider --repo <YOUR_CP_REPO> collateral withdraw --fcp --owner <YOUR_WALLET_ADDRESS> --account <YOUR_CP_ACCOUNT> <amount>
```

To withdraw from the **Escrow account:**

Submit withdrawal request:

```
computing-provider --repo <YOUR_CP_REPO> collateral withdraw-request --fcp --owner <YOUR_OWNER_ADDRESS> <AMOUNT>
```

Confirm withdrawal (after 7-day waiting period):

```
computing-provider --repo <YOUR_CP_REPO> collateral withdraw-confirm --fcp --owner <YOUR_OWNER_ADDRESS>
```

### **Notes**

* It’s recommended to perform Step 2 (withdrawing from Escrow) 24 hours after completing Step 1, as the engine periodically settles CP accounts. This includes actions like locking collaterals, slashing collaterals, sending UBI rewards, etc., all of which may impact the success rate of your withdrawal.
* After completing Step 1, the CP will no longer receive any tasks, including UBI tasks and regular AI tasks.
