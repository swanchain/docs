# Config and Receive UBI Tasks(optional)

The [Universal Basic Income (UBI)](../../../getting-started/protocol-stack/economic-system/swan-universal-basic-income-ubi.md) model is implemented to provide a foundational level of compensation to qualified providers.

Configuring your Computing Provider (CP) to receive UBI tasks within the Swan decentralized computing ecosystem is crucial for participating in the incentive mechanism and earning fair compensation for your contributions.

{% hint style="info" %}
**UBI Task Types:** The UBI tasks are categorized based on Filecoin zk-stark proof mechanisms. Providers can earn rewards by completing Commit2 proof processes within specific time frames. The task types and their corresponding rewards are as follows:

* **512M Sector CPU Task:** 1 SWAN
* **512M Sector GPU Task:** 2 SWAN
* **32G Sector CPU Task:** 10 SWAN
* **32G Sector GPU Task:** 50 SWAN

These tasks offer varying UBI rewards, and the UBI engine periodically checks for idle resources, randomly sending UBI tasks that the computing provider (CP) can undertake.
{% endhint %}

For more details please refer to[ ](../../../getting-started/protocol-stack/economic-system/swan-universal-basic-income-ubi.md)[Universal Basic Income here](../../../getting-started/protocol-stack/economic-system/swan-universal-basic-income-ubi.md)

### **Step 1: Prerequisites:** Perform Filecoin Commit2 (fil-c2) UBI tasks.

1.  Download the tool for Filecoin Commit2 task parameters:

    ```bash
    wget  https://github.com/swanchain/ubi-benchmark/releases/download/v0.0.1/lotus-shed
    ```
2.  Download parameters (specify path with FIL\_PROOFS\_PARAMETER\_CACHE variable):

    ```bash
    export FIL_PROOFS_PARAMETER_CACHE=/var/tmp/filecoin-proof-parameters
    lotus-shed fetch-params --proving-params 512MiB # 512MiB represent sector size
    lotus-shed fetch-params --proving-params 32GiB # 32GiB represent sector size
    ```
3.  Configure environment variables in `fil-c2.env` under CP repo ($CP\_PATH):

    ```bash
    FIL_PROOFS_PARAMETER_CACHE="/var/tmp/filecoin-proof-parameters"
    RUST_GPU_TOOLS_CUSTOM_GPU="GeForce RTX 3080:8704" 
    ```

* Adjust the value of `RUST_GPU_TOOLS_CUSTOM_GPU` based on the GPU used by the CP's Kubernetes cluster for fil-c2 tasks.
* For more device choices, please refer to this page:[https://github.com/filecoin-project/bellperson](https://github.com/filecoin-project/bellperson)

### **Step 2: Enable UBI tasks in CP's** `config.toml`**:**

```ini
[API]
UbiTask = true
```

### Step 3: Initialize a Wallet and Deposit Swan-ETH

1.  Generate a new wallet address:

    ```bash
    computing-provider wallet new
    ```

    Example output:

    ```
    0x7791f48931DB81668854921fA70bFf0eB85B8211
    ```
2.  Deposit Swan-ETH to the generated wallet address as a gas fee:

    ```bash
    computing-provider wallet send --from 0xFbc1d38a2127D81BFe3EA347bec7310a1cfa2373 0x7791f48931DB81668854921fA70bFf0eB85B8211 0.001
    ```

    Example output:

    ```
    0xa255d9046eff7c7c7ef6f4b55efcf97b62c79aeece748ab2188de21da620f29b
    ```



Note: Follow [this guide](https://app.gitbook.com/s/y5iPODl9iwLxyYirHs2D/api-reference) to claim Swan-ETH and bridge it to Swan Saturn Chain.

### **Step 4: Initialization**

1.  Deploy a contract with CP's basic info:

    ```bash
    computing-provider init --ownerAddress 0xFbc1d38a2127D81BFe3EA347bec7310a1cfa2373
    ```

    _Output:_

    ```
    Contract deployed! Address: 0x3091c9647Ea5248079273B52C3707c958a3f2658
    Transaction hash: 0xb8fd9cc9bfac2b2890230b4f14999b9d449e050339b252273379ab11fac15926
    The height of the block: 44900354
    ```

### **Step 5: Account Management**

Use `computing-provider account` subcommands to update CP details:

```
computing-provider account -h
NAME:
   computing-provider account - Manage account info of CP

USAGE:
   computing-provider account command [command options] [arguments...]

COMMANDS:
   changeMultiAddress        Update MultiAddress of CP
   changeOwnerAddress        Update OwnerAddress of CP
   changeBeneficiaryAddress  Update BeneficiaryAddress of CP
   changeUbiFlag             Update UbiFlag of CP
   help, h                   Shows a list of commands or help for one command

OPTIONS:
   --help, -h  show help
```

### Step 6: Check the Status of UBI-Task&#x20;

To check the UBI task list, use the following command:

```
computing-provider ubi-task list
```

Example output:

```
TASK ID TASK TYPE       ZK TYPE         TRANSACTION HASH                                                        STATUS  REWARD  CREATE TIME         
2       CPU             fil-c2-512M     0xb06b3a8c2b2b96b564777a3866e27ce7c61631f77e5de3196e93eb916b0d2575      success 2.0     2024-01-20 03:30:30
33      CPU             fil-c2-512M     0x7567435e83a4a019a6356da8cf33e64a071f2d3355fce5289b9c17cf0144f282      success 2.0     2024-01-18 15:58:21
13      CPU             fil-c2-512M     0x7b3081314891aad3788c84935c67f9be0a8acc6b4fc77c5aa6fdfda728877fde      success 2.0     2024-01-20 04:27:40
238     CPU             fil-c2-512M     0xb8eb1f7b3cfc8210fa5546adc528f230241110e5cc9b4900725a9da28895aad9      success 2.0     2024-01-18 17:08:21
```
