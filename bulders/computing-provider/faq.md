# FAQ

### **Q: What is CU?**

CU, short for **Computing Unit**, is a key parameter in Swan Chain. It is a virtual unit used to measure the computing resource contributions of **Computing Providers (CPs)**. CU plays a crucial role in determining the collateral requirements for CPs.

The primary function of CU is to help calculate the collateral requirements for CPs. This ensures that CPs have sufficient staking to qualify for **UBI rewards** and **task allocations** on Swan Chain. Each CP’s CU value can be checked via the [**Provider Dashboard**](https://provider.swanchain.io/).

### **Q: How is the CP collateral calculated?**

The required **collateral** is calculated using the following formula:

$$
Collateral=CU×Base Collateral
$$

Currently, the **Base Collateral** is set at **3,533**.

**Example Calculation**

If a CP has a CU of **100**, the collateral requirement would be:

$$
100×3,533=353,300
$$

### **Q: How to Change Owner Address**

To change the `owner address` of your CP account, use the following command:

```bash
computing-provider --repo <YOUR_CP_REPO> account changeOwnerAddress --ownerAddress <YOUR_OLD_OWNER_ADDRESS> <YOUR_NEW_OWNER_ADDRESS>
```

**Note**: The `YOUR_NEW_OWNER_ADDRESS` does not need to be on the CP machine, but the owner’s private key is required for this operation.

### **Q: How to Change Worker Address**

To change the `worker address`, run:

```bash
computing-provider --repo <YOUR_CP_REPO> account changeWorkerAddress --ownerAddress <YOUR_OWNER_ADDRESS> <YOUR_NEW_WORKER_ADDRESS>
```

**Note**: The `YOUR_NEW_WORKER_ADDRESS` must be stored on the CP machine for two essential purposes:

* Verification of bidirectional signatures by the UBI engine when receiving UBI tasks. Tasks will not be accepted if verification fails.
* Proof submission to the chain. While this direct submission method provides faster confirmation, it requires the worker wallet to maintain an ETH balance for gas fees. Alternatively, using the sequencer method for submission can help reduce gas costs

### **Q: How to Change Beneficiary Address**

Update the `beneficiary address` with:

```bash
computing-provider --repo <YOUR_CP_REPO> account changeBeneficiaryAddress --ownerAddress <YOUR_OWNER_ADDRESS> <YOUR_NEW_BENEFICIARY_ADDRESS>
```

**Note**: The beneficiary address is for receiving rewards, and no private key is required on the CP machine.

* For CP maintenance personnel, this separation enhances security as they can manage the CP without direct access to the beneficiary wallet. Additionally, all CP rewards (including UBI rewards, application deployment rewards, etc.) are distributed to this address.

### **Q: How to Change Task Types**

CP currently supports five task types, each corresponding to different workloads and associated rewards. The more task types you set, the more diverse tasks your CP can accept.

There are two CP roles: FCP and ECP.

* **For FCP**: The default task types are 3 and 4. To accept more tasks, additional configurations are needed, such as enabling [NodePort](https://github.com/swanchain/go-computing-provider/tree/releases?tab=readme-ov-file#optional-install-node-port-dependency).
* **For ECP**: The default task types are 1 and 2. To accept more tasks, additional configurations are needed, such as enabling [Inference](https://github.com/swanchain/go-computing-provider/blob/releases/ubi/README.md#config-and-receive-inference-task).

You can set the task types using the following command:

```bash
computing-provider --repo <YOUR_CP_REPO> account changeTaskTypes --ownerAddress <YOUR_OWNER_ADDRESS> <task_type1>,<task_type2>,...
```

**Note**: Separate task types with commas. The full list of task types can be found in the Swan Chain documentation [here](https://docs.swanchain.io/bulders/computing-provider/fog-computing-provider-fcp/computing-provider-setup#step-3-change-the-tasktypes).

**Task Types**:

1. **Fil-C2**: Time-space proof tasks for Filecoin.
2. **Mining**: Mining-related tasks.
3. **AI**: AI and web service tasks.
4. **Inference**: Large model inference API tasks.
5. **NodePort**: VM-like services or multi-port services.

### **Q: How to Change Multi-Address**

MultiAddress defines how your CP is accessible on the network.

1. Update network configuration:

* For cloud providers: Configure in provider console
* For private datacenters: Configure on network egress devices

2. To update the public IP or internal port mapping, modify the `config.toml` file:

```toml
[API]
MultiAddress = "/ip4/<public_ip>/tcp/<port>"
```

3. Then run the update command:

```bash
computing-provider --repo <YOUR_CP_REPO> account changeMultiAddress --ownerAddress <YOUR_OLD_OWNER_ADDRESS> <YOUR_NEW_MULTI_ADDRESS>
```

4. Finally, verify the update:

```bash
# Step 1: Check local configuration
computing-provider --repo <YOUR_CP_REPO> info

# Step 2: Verify API accessibility
# For FCP:
curl -k https://<PUBLIC_IP>:<PORT>/api/v1/computing/cp
# For ECP:
curl http://<PUBLIC_IP>:<PORT>/api/v1/computing/cp
```

***

## ECP FAQ

### **Q: How to Initialize Repository**

The first step in setting up an ECP is initializing the repository. This creates a `config.toml` file containing essential configuration settings and defines data storage parameters.

```bash
computing-provider --repo <YOUR_CP_REPO> init --multi-address=/ip4/<YOUR_PUBLIC_IP>/tcp/<YOUR_PORT> --node-name=<YOUR_NODE_NAME>
```

### **Q: How to Create Account**

In the Swan network, each CP exists as a contract address. The CP account serves as your unique identifier for all operations, including task distribution, reward collection, and collateral management. It represents your primary token of identity within the Swan network.

```bash
computing-provider --repo <YOUR_CP_REPO> account create \
    --ownerAddress <YOUR_OWNER_WALLET_ADDRESS> \
    --workerAddress <YOUR_WORKER_WALLET_ADDRESS> \
    --beneficiaryAddress <YOUR_BENEFICIARY_WALLET_ADDRESS> \
    --task-types 1,2
```

### **Q: How to Add Collateral**

To participate in the Swan network and receive tasks, ECPs must stake SWAN tokens as collateral. This stake serves as a security mechanism - if service availability falls below acceptable levels, penalties may be applied against this collateral.

```bash
computing-provider --repo <YOUR_CP_REPO> collateral add --ecp --from <YOUR_WALLET_ADDRESS> <amount>
```

check details about collateral [here](https://docs.swanchain.io/core-concepts/token/computing-provider-collateral/collateral-requirement-and-earning-multiplier).

### **Q: How to Withdraw Collateral**

There are two methods for withdrawing collateral, each with different processing times:

1.  **Direct Withdrawal from `Collateral Balance`**

    ```bash
    computing-provider --repo <YOUR_CP_REPO> collateral withdraw --ecp --owner <YOUR_WALLET_ADDRESS> --account <YOUR_CP_ACCOUNT> <amount>
    ```

    _Note: Funds are transferred immediately to the owner address._
2.  **Withdrawal from `Escrow Balance`** This process requires a waiting period and multiple steps:

    ```bash
    # Submit withdrawal request
    computing-provider --repo <YOUR_CP_REPO> collateral withdraw-request --ecp --owner <YOUR_OWNER_ADDRESS> <AMOUNT>

    # Confirm withdrawal (after 7-day waiting period)
    computing-provider --repo <YOUR_CP_REPO> collateral withdraw-confirm --ecp --owner <YOUR_OWNER_ADDRESS>

    # Check withdrawal status
    computing-provider --repo <YOUR_CP_REPO> collateral withdraw-view --ecp
    ```

    _Note: After submitting the withdrawal request, there is **a mandatory 7-day waiting period** before confirmation can be processed. Use the command `withdraw-view` to monitor the withdrawal status._

### **Q: How to Manag Sequencer Balance**

1.  **Adding Funds to Sequencer**

    ```bash
    computing-provider --repo <YOUR_CP_REPO> sequencer add --from <YOUR_WALLET_ADDRESS> <amount>
    ```
2.  **Withdrawing from Sequencer**

    ```bash
    computing-provider --repo <YOUR_CP_REPO> sequencer withdraw --owner <YOUR_OWNER_ADDRESS> <amount>
    ```

***

## FCP FAQ

### **Q: How to Initialize Repository**

The first step in setting up an FCP is initializing the repository. This creates a `config.toml` file that contains essential configuration settings and defines data storage parameters.

```bash
computing-provider --repo <YOUR_CP_REPO> init --multi-address=/ip4/<YOUR_PUBLIC_IP>/tcp/<YOUR_PORT> --node-name=<YOUR_NODE_NAME>
```

### **Q: How to Create Account**

In the Swan network, each CP exists as a contract address, with the CP account serving as the unique identifier for all operations. This account handles task distribution, reward collection, and collateral management. It's your primary token of identity within the Swan network.

```bash
computing-provider --repo <YOUR_CP_REPO> account create \
    --ownerAddress <YOUR_OWNER_WALLET_ADDRESS> \
    --workerAddress <YOUR_WORKER_WALLET_ADDRESS> \
    --beneficiaryAddress <YOUR_BENEFICIARY_WALLET_ADDRESS> \
    --task-types 3
```

### **Q: How to Add Collateral**

To participate in the Swan network and receive tasks, FCPs must stake SWAN tokens as collateral. This stake serves as a security mechanism - if service availability falls below acceptable levels, penalties may be applied against this collateral.

```bash
computing-provider --repo <YOUR_CP_REPO> collateral add --fcp --from <YOUR_WALLET_ADDRESS> <amount>
```

check details about collateral [here](https://docs.swanchain.io/core-concepts/token/computing-provider-collateral/collateral-requirement-and-earning-multiplier).

### **Q: How to Withdraw Collateral**

There are two methods for withdrawing collateral, each with different processing times:

1.  **Direct Withdrawal from `Collateral Balance`**

    ```bash
    computing-provider --repo <YOUR_CP_REPO> collateral withdraw --fcp --owner <YOUR_WALLET_ADDRESS> --account <YOUR_CP_ACCOUNT> <amount>
    ```

    _Note: Funds are transferred immediately to the owner address._
2.  **Withdrawal from `Escrow Balance`** This process requires a waiting period and multiple steps:

    ```bash
    # Submit withdrawal request
    computing-provider --repo <YOUR_CP_REPO> collateral withdraw-request --fcp --owner <YOUR_OWNER_ADDRESS> <AMOUNT>

    # Confirm withdrawal (after 7-day waiting period)
    computing-provider --repo <YOUR_CP_REPO> collateral withdraw-confirm --fcp --owner <YOUR_OWNER_ADDRESS> 

    # Check withdrawal status
    computing-provider --repo <YOUR_CP_REPO> collateral withdraw-view --fcp
    ```

    _Note: After submitting the withdrawal request, there is **a mandatory 7-day waiting period** before confirmation can be processed. Use the view command to monitor the withdrawal status._



### **Q: What is the difference between ECP and FCP, and what are the minimum configuration requirements?**

**FCP:**

* For applications deployed with Kubernetes or large model inference API services. Users can directly offer services via a running URL. Kubernetes deployment and maintenance are technically more challenging but provide higher rewards.
* **Configuration Requirements**:
  * A public IP
  * A wildcard domain (e.g., `*.example.com`)
  * An SSL certificate, primarily for Kubernetes Ingress reverse proxy
  * At least 1 GPU
  * At least 8 vCPUs
  * At least 100 GB SSD storage
  * At least 64 GB RAM
  * At least 50 MB bandwidth

**ECP:**

* For applications deployed with Docker or Mining-type services. Easier to deploy with scripts available for quick setup.
* **Configuration Requirements**:
  * A public IP
  * At least 1 GPU
  * At least 4 vCPUs
  * At least 300 GB HDD storage
  * At least 32 GB RAM
  * At least 20 MB bandwidth

***

### **Q: What do the `Owner`, `Worker`, and `Beneficiary` addresses represent, and what are the differences?**

* **ownerAddress**: The owner of the `cpAccount`. Only the owner has permission to change account details, such as `multi-address`, `worker address`, `beneficiary address`, etc. The owner's private key should not appear on the server under normal circumstances.
* **workerAddress**: The working address, used for submitting messages (e.g., `submitProof`). A certain amount of sETH is required for gas fees.
* **beneficiaryAddress**: The beneficiary address where earnings from the `cpAccount` are paid. This address is only used to receive funds, so its private key should not be stored on the server for security reasons. The purpose of these three address types is to ensure the security of the `cpAccount`. Typically, the server should only need to maintain the private key for the worker address, not the owner or beneficiary addresses.

***

### **Q: What should I do if my server's public IP changes?**

Follow the instructions in the **CP Common Account Operations** section to change the `multiAddress`.

### **Q: How can I check how much collateral my CP needs, and is my collateral sufficient?**

* **Collateral Calculation**:
  * First, visit the [Provider Dashboard](https://provider.swanchain.io/overview) to query your `cpAccount` and find the CU value.
  * The collateral formula is: `cu * base_collateral`. Currently, `base_collateral = 3533`. It’s recommended to add 10% extra for safety.
* You can check your collateral status via the CP Dashboard:

**Sufficient collateral** (Example):

![image](https://github.com/user-attachments/assets/21b014b6-73fa-4bdf-844a-938fa18a796a)

**Insufficient collateral** (Example):

&#x20;![image](https://github.com/user-attachments/assets/bba214de-355e-48b2-b165-1ec0bfddffd7)

### **Q: What is the current slashing mechanism for CP?**

When you deploy an application to CP, the CP runs the service, and the SWAN monitoring system will penalize the CP if the service's availability is deemed poor. For more details, refer to the [Slashing Mechanism](https://docs.swanchain.io/core-concepts/token/computing-provider-collateral#slashing-mechanism).

### **Q: How to verify if FCP is running properly?**

* **Step 1: Check Collateral**:

```
computing-provider --repo info
```

![image](https://github.com/user-attachments/assets/61fb8bc2-71cd-4dc1-bcf0-fb9fd073d4b0)

Ensure that the sum of `Collateral` and `Escrow` under the `FCP Balance` is greater than the calculated collateral amount for proper staking.

* **Step 2: Check Versions**:
  * **CP Version**:

```
computing-provider -v
```

Verify the CP version by checking the commit number in the official [go-computing-provider](https://github.com/swanchain/go-computing-provider) repository to confirm if your version is up-to-date or the recommended stable version.

* **Resource-Exporter Version**:

```
kubectl get daemonset resource-exporter-ds -n kube-system -o yaml | grep image:
```

Verify the version of `resource-exporter` by checking the required version in the [official repository](https://github.com/swanchain/go-computing-provider/tree/releases?tab=readme-ov-file#Install-the-Hardware-resource-exporter).

* **Step 3: Check Resource Endpoint**:

```
curl -k https://<cp-ip>:<port>/api/v1/computing/cp
```

> **Note**: Replace `<cp-ip>` and `<port>` with the corresponding information from your `CP_REPO` configuration file, where `MultiAddress = "/ip4/<ip>/tcp/<port>"`.

* **Step 4: Self-check by submitting a job**:

```
curl -k --location --request POST 'https://<cp-ip>:<port>/api/v1/computing/lagrange/jobs' \
--header 'Content-Type: application/json' \
--data-raw '{
"uuid": "5641877b-dc94-469a-bb3b-ecab6d10f7dd",
"name": "Job-5641877b-dc94-469a-bb3b-ecab6d10f7dd",
"status": "Submitted",
"duration": 900,
"job_source_uri": "https://api.lagrangedao.org/spaces/51d6abbb-f928-43e4-91fd-79e93e2b276f",
"storage_source": "lagrange",
"task_uuid": "92cd5595-9789-4af3-9100-7c7e4aacb456"
}'
```

* **Step 5: Dashboard Status Check**: In the [Provider Dashboard](https://provider.swanchain.io/), locate your CP using the `cpAccount`. Check that the status is `Online`, and ensure collateral is sufficient (as shown in the example). If so, your CP is running properly. ![image](https://github.com/user-attachments/assets/47049468-d9bb-4aac-8870-6dafa84a3866)

### **Q: How to verify if ECP is running properly?**

* **Step 1: Check Collateral**:

```
computing-provider --repo info
```

![image](https://github.com/user-attachments/assets/92e14ebd-cb46-4b3e-98f7-aa50ad544f0c)

Ensure that the sum of `Collateral` and `Escrow` under the `ECP Balance` is greater than the calculated collateral amount for proper staking.

* **Step 2:Check Versions**:
  * **CP Version**:

```
computing-provider -v
```

Verify the CP version by checking the commit number in the official [go-computing-provider](https://github.com/swanchain/go-computing-provider) repository to confirm if your version is up-to-date or the recommended stable version.

* **Resource-Exporter Version**:

```
kubectl get daemonset resource-exporter-ds -n kube-system -o yaml | grep image:
```

Verify the version of `resource-exporter` by checking the required version in the [official repository](https://github.com/swanchain/go-computing-provider/tree/releases?tab=readme-ov-file#Install-the-Hardware-resource-exporter).

* **Step 3: Check Resource Endpoint**:

```
curl http://<cp-ip>:<port>/api/v1/computing/cp
```

> **Note**: Replace `<cp-ip>` and `<port>` with the corresponding information from your `CP_REPO` configuration file, where `MultiAddress = "/ip4/<ip>/tcp/<port>"`.

* **Step 4: Self-check by submitting a job**:

```
curl --location --request POST 'http://<cp-ip>:<port>/api/v1/computing/cp/ubi' \
--header 'Content-Type: application/json' \
--data-raw '{
    "id": 26384,
    "name": "1000-0-7-26381",
    "type": 1,
    "resource_type": 0,
    "deadline": 2000000,
    "check_code": "check_code-demo",
    "input_param": "https://286cb2c989.acl.swanipfs.com/ipfs/QmTgoX6LkzZTsTjSjXvujzgJEHBLTEg3KMUadQGnyTrNFG",
    "verify_param": "https://286cb2c989.acl.swanipfs.com/ipfs/QmTgoX6LkzZTsTjSjXvujzgJEHBLTEg3KMUadQGnyTrNFG.verify",
    "resource":{"cpu":"10","memory":"5.00 GiB","storage":"10.00 GiB"}
    "signature":"Signing_cpAccountAddress_and_id_with_ownerAddress"
}'
```

* **Dashboard Status Check**: In the [Provider Dashboard](https://provider.swanchain.io/), locate your CP using the `cpAccount`. Check that the status is `Online`, and ensure collateral is sufficient (as shown in the example). If so, your CP is running properly. ![image](https://github.com/user-attachments/assets/087a96f3-a1c3-4607-b802-b343a51246e6)

