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

### **Step1: Prerequisites:** Perform Filecoin Commit2 (fil-c2) UBI tasks.

1.  Download the tool for Filecoin Commit2 task parameters:

    ```bash
    wget https://286cb2c989.acl.multichain.storage/ipfs/QmTTD7kcAR9i3SJ8CXoncinkYNerP9t3DUvib8H8MfAaph?filename=shed.zip -O shed.zip
    unzip shed.zip
    ```
2.  Download parameters (specify path with FIL\_PROOFS\_PARAMETER\_CACHE variable):

    ```bash
    export FIL_PROOFS_PARAMETER_CACHE=/var/tmp/filecoin-proof-parameters
    lotus-shed fetch-params --proving-params 32GiB # 512MiB, 32GiB represent sector sizes
    ```
3.  Configure environment variables in `fil-c2.env` under CP repo ($CP\_PATH):

    ```bash
    FIL_PROOFS_PARAMETER_CACHE="/var/tmp/filecoin-proof-parameters"
    RUST_GPU_TOOLS_CUSTOM_GPU="GeForce RTX 3080:8704" 
    ```

Adjust the value of `RUST_GPU_TOOLS_CUSTOM_GPU` based on the GPU used by the CP's Kubernetes cluster for fil-c2 tasks.

### **Step2: Modify CP config to enable UBI tasks in `fil-c2.env`:**

```ini
[API]
UbiTask = true
```

### **Step3: Initialization:**

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

**Step4: Account Management:**

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
