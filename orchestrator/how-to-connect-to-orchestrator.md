# How to connect to Orchestrator

This is a step-by-step tutorial on how to connect to the Orchestrator, including the process of depositing SWAN tokens as collateral.

**Step 1: Update Your Computing Provider**

Ensure you have the latest version of the Computing Provider to accommodate the transition from the **LAG Server** to the **Orchestrator.**

[https://github.com/swanchain/go-computing-provider](https://github.com/swanchain/go-computing-provider)

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

Stay informed about your collateral balance by checking it in real-time. If the balance falls below the configured threshold, a warning `No sufficient collateral Balance, the current, collateralbalance is ...`will be logged.

**Option 1:** Check your collateral information by using the following command:

```bash
# Check CP wallet Swan token balance and collateral information
computing-provider collateral info
```

**Option 2:** Log in to Dashboard > Profile > CP Collateral Check

<figure><img src="../.gitbook/assets/image (153).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (154).png" alt=""><figcaption></figcaption></figure>

**Step 4: Connect to Swan Hub**

Once you've completed the collateral deposit, follow the appropriate steps based on your CP deployment status:

1. **If you haven't deployed a CP yet:**
   * Please refer to the provided [instructions](computing-provider-setup/) to set up your Computing Provider (CP) .
2. **If you have already deployed a CP:**
   * Initiate the connection to the Orchestrator by updating your CP configuration file (`config.toml`).

```toml
[API]
Port = 8085                                     # The port number that the web server listens on
MultiAddress = "/ip4/<public_ip>/tcp/<port>"    # The multiAddress for libp2p
Domain = ""                                     # The domain name

RedisUrl = "redis://127.0.0.1:6379"           # The redis server address
RedisPassword = ""                            # The redis server access password

[LOG]
CrtFile = "/YOUR_DOMAIN_NAME_CRT_PATH/server.crt"	# Your domain name SSL .crt file path
KeyFile = "/YOUR_DOMAIN_NAME_KEY_PATH/server.key"   	# Your domain name SSL .key file path

[HUB]
ServerUrl = "https://swanhub-cali.swanchain.io"     # The swan hub API address
AccessToken = ""                                    # Login to"https://testnet-provider.lagrangedao.org/provider-status"-> show API-KEY 
WalletAddress = ""                                  # The cp wallet address
BalanceThreshold= 50                                # The cp’s collateral balance threshold

[RPC]
GOERLI_URL = "https://goerli.infura.io/v3/"
SWAN_TESTNET ="https://rpc-testnet.swanchain.io"
SWAN_MAINNET= ""

[CONTRACT]
SWAN_CONTRACT="0x407a5856050053CF1DB54113bd9Ea9D2Eeee7C35"
SWAN_COLLATERAL_CONTRACT="0xaAea25dF7D20f2098B816486880E4b706DD57044"

[MCS]
ApiKey = ""                                   # Acquired from "https://www.multichain.storage" -> setting -> Create API Key
BucketName = ""                               # Acquired from "https://www.multichain.storage" -> bucket -> Add Bucket
Network = "polygon.mainnet"                   # polygon.mainnet for mainnet, polygon.mumbai for testnet
FileCachePath = "/tmp"                        # Cache directory of job data

[Registry]                                    
ServerAddress = ""                            # The docker container image registry address, if only a single node, you can ignore
UserName = ""                                 # The login username, if only a single node, you can ignore
Password = ""                                 # The login password, if only a single node, you can ignore
```

and then  run `computing-provider` using the following command

```bash
export CP_PATH=xxx
nohup computing-provider run >> cp.log 2>&1 & 
```

**Congratulations!**&#x20;

You have successfully connected to the Swan Hub, deposited the required collateral, and are now ready to receive task assignments.
