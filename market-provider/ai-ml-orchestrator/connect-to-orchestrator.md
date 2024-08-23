# Connect to Orchestrator

**Prerequisites :**

* **ETH**
  * Ensure that you have sufficient ETH for gas fee.&#x20;
    * You can bridge ETH from Etherum to Swan Chain by visiting [https://bridge.swanchain.io](https://bridge.swanchain.io/).
  * Ensure that you have sufficient Swan Credit Token (SWANC) for collateral.
    * You can obtain SWANC by following this [guide](../../swan-chain/swan-chain-mainnet/swan-credit-token.md).
* **Obtain Filecoin V28 Parameters (optional)**
  * To complete UBI tasks, you need to have the Filecoin V28 parameters.&#x20;
  * Make sure you have the parameters corresponding to 512M and 32G sectors, as these cater to different task requirements.

Ensure that you have completed all these preparations before connecting to the Orchestrator.&#x20;

**Step 1: Setup Your Computing Provider**

Follow this [guide](../../computing-provider/fog-computing-provider-fcp/computing-provider-setup.md) to deploy your Computing Provider and make sure you have the latest version installed.

{% embed url="https://github.com/swanchain/go-computing-provider" %}

**Step 2: Deposit SWANC Tokens as Collateral**

```
 computing-provider collateral add --fcp --from <YOUR_WALLET_ADDRESS>  <amount>
```

**Note:** Currently one AI task requires 5 `SWANC`. Please deposit enough collaterals for the tasks



**Step 3: Real-Time Monitoring**

Stay informed about your collateral balance by checking it in real-time. If the balance falls below the configured threshold, a warning `No sufficient collateral Balance, the current collateral balance is ...`will be logged.

**Option 1:** Check your collateral information by using the following command:

```bash
# Check CP wallet Swan token balance and collateral information
computing-provider collateral info
```

**Option 2:** Log in to Dashboard > Profile > CP Collateral Check

<figure><img src="../../.gitbook/assets/image (153).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (154).png" alt=""><figcaption></figcaption></figure>

**Congratulations!**&#x20;

Your nodes are now ready to receive task assignments. You can check the status of your nodes [here](https://orchestrator.swanchain.io/provider-status).
