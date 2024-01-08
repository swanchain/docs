# Connect to Orchestrator

**Step 1: Setup Your Computing Provider**

Follow this [guide](computing-provider-setup/) to deploy your Computing Provider and make sure you have the latest version installed.

{% embed url="https://github.com/swanchain/go-computing-provider" %}

**Step 2: Deposit SWAN Tokens as Collateral**

To connect to the Orchestrator, a minimum of **50 SWAN** tokens must be deposited as collateral. Use the following command to deposit tokens into the contract:

```bash
# Collateral deposit command
computing-provider collateral [Wallet Address] [Amount]
```

_Example: Deposit 1000 Swan tokens as collateral_

```bash
computing-provider collateral 0xFbc1d3...2373 1000
```

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

Your nodes are now ready to receive task assignments. You can check the status of your nodes [here](https://cp-test.swanchain.io/provider-status).
