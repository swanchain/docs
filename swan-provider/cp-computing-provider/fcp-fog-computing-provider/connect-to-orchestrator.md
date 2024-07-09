# Connect to Orchestrator

**Prerequisites :**

* **SwanETH**
  * Ensure that you have sufficient SwanETH for collateral.&#x20;
  * You can obtain SwanETH following this [guide](../../../swan-chain/swan-saturn-testnet/before-you-get-started/claim-faucet-tokens.md) and [bridge](../../../swan-chain/swan-saturn-testnet/before-you-get-started/bridge-tokens.md) them from Sepolia to Swan Saturn Chain
* **Obtain Filecoin V28 Parameters (optional)**
  * To complete UBI tasks, you need to have the Filecoin V28 parameters.&#x20;
  * Make sure you have the parameters corresponding to 512M and 32G sectors, as these cater to different task requirements.

Ensure that you have completed all these preparations before connecting to the Orchestrator.&#x20;

**Step 1: Setup Your Computing Provider**

Follow this [guide](computing-provider-setup.md) to deploy your Computing Provider and make sure you have the latest version installed.

{% embed url="https://github.com/swanchain/go-computing-provider" %}

**Step 2: Deposit SwanETH Tokens as Collateral**

To connect to the Orchestrator, a minimum of **0.01 SwanETH** tokens must be deposited as collateral for each task. Use the following command to deposit tokens into the contract:

```bash
# Collateral deposit command
computing-provider collateral add [Wallet Address] [Amount]
```

_Example: Deposit 1 SwanETH token as collateral_

```bash
computing-provider collateral add 0xFbc1d3...2373 1
```

**Step 3: Real-Time Monitoring**

Stay informed about your collateral balance by checking it in real-time. If the balance falls below the configured threshold, a warning `No sufficient collateral Balance, the current collateral balance is ...`will be logged.

**Option 1:** Check your collateral information by using the following command:

```bash
# Check CP wallet Swan token balance and collateral information
computing-provider collateral info
```

**Option 2:** Log in to Dashboard > Profile > CP Collateral Check

<figure><img src="../../../.gitbook/assets/image (153).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (154).png" alt=""><figcaption></figcaption></figure>

**Congratulations!**&#x20;

Your nodes are now ready to receive task assignments. You can check the status of your nodes [here](https://cp-test.swanchain.io/provider-status).
