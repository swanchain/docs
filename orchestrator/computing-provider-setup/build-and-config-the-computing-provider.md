# Build and config the Computing Provider

Step1: Firstly, clone the code to your local:

```bash
git clone https://github.com/lagrangedao/go-computing-provider.git
cd go-computing-provider
git checkout v0.3.0
```

Step2: Then build the Computing provider follow the below steps:

```bash
make clean && make
make install
```

Step3: Update Configuration The computing provider's configuration sample locate in `./go-computing-provider/config.toml.sample`

```
cp config.toml.sample config.toml
```

{% hint style="info" %}
**Important Update:**&#x20;

1. **Connection Update:** The Computing Provider has transitioned from connecting to the **LAG Server** to connecting to the **Swan Hub**.
2. **Collateral Requirement:** CPs are now mandated to deposit a minimum of **50 SWAN** tokens as collateral into the contract. Failure to meet this requirement will result in the inability to receive task assignments.
3. **Real-Time Monitoring:** Computing Providers now support real-time monitoring of collateral balances. If the balance falls below the configured threshold, a warning will be logged.
{% endhint %}

Step4: Update your configuration file (config.toml) as follows, paying special attention to the newly added `HUB`, `RPC`, and `CONTRACT`sections:

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

**Note: You can check your token balance and collateral using the following command:**

```
# Check CP wallet Swan token balance and the number of tokens that can be as collateral
computing-provider collateral info  

# collateral 
computing-provider collateral [Wallet Address] [amount]

Example: Deposit 1000 Swan tokens as collateral
computing-provider collateral 0xFbc1d3...2373 1000
```
