# Mining Task Example

In this section, we will use Swan Console to launch an example Mining deployment on the Swan Network. You can follow the same process for any other tasks.

### 1. Accessing the Marketplace <a href="#id-01f0" id="id-01f0"></a>

The Swan Chain marketplace provides access to various mining pools. To begin:

* Select “Marketplace” from the left panel
* Navigate to the Mining section
* View all supported mining tasks

<figure><img src="https://miro.medium.com/v2/resize:fit:700/1*dOQb-tlz_4bk9ALO8bvF1g.png" alt="" height="327" width="700"><figcaption></figcaption></figure>

### 2. Pool Registration <a href="#id-8b13" id="id-8b13"></a>

Using f2pool as an example:

* Visit[ F2pool’s official guide](https://f2pool.io/mining/guides/how-to-mine-aleo/#2-sign-up-for-an-f2pool-account)
* Complete account registration
* Save your account name — you’ll need it for the deployment process.

### 3. Deployment Configuration <a href="#id-5935" id="id-5935"></a>

### 3.1 Pre-deployment Preparation <a href="#id-57d6" id="id-57d6"></a>

Before proceeding with deployment, you’ll notice a section containing environment variables. These variables are crucial for your mining operation — they contain the connection details and authentication information needed to connect to the mining pool.

<figure><img src="https://miro.medium.com/v2/resize:fit:700/1*qY6qoGSI2oKXlSPnZYjIIw.png" alt="" height="478" width="700"><figcaption></figcaption></figure>

**Important**: Save these variables securely — you’ll need them for the configuration steps.

### 3.2 Settings Configuration <a href="#id-259c" id="id-259c"></a>

**3.2.1 Aleo Mainnet**

1. Create a unique instance name (no specific naming rules)
2. Click “Add Environment Variables” to create more input fields. Remember that all F2pool images require three essential variables:

* MINER\_URL: stratum+ssl://aleo-asia.f2pool.com:4420
* ACCOUNTNAME: Your registered F2pool account name
* WORKERNAME: A unique identifier for this mining worker

<figure><img src="https://miro.medium.com/v2/resize:fit:700/0*7GAJelNTO-Nd6BYn" alt="" height="188" width="700"><figcaption></figcaption></figure>

<figure><img src="https://miro.medium.com/v2/resize:fit:700/0*qKqzcRCvdOBdGtyA" alt="" height="225" width="700"><figcaption></figcaption></figure>

**Critical Note**: Even minor typing errors in these variables can prevent successful connection to the mining pool. For detailed information about Aleo mining on F2pool, consult the[ official documentation](https://f2pool.io/mining/guides/how-to-mine-aleo/).

**3.2.2 Iron Fish (IRON)**

1. Create a unique instance name (no specific naming rules)

2\. Click “Add Environment Variables” to create more input fields, Remember that all F2pool images require three essential variables:

* MINER\_URL: Choose the server closest to your location
* ACCOUNTNAME: Your F2pool account name
* WORKERNAME: Your chosen worker identifier

<figure><img src="https://miro.medium.com/v2/resize:fit:700/0*pGGSbv4bVLreXHm5" alt="" height="188" width="700"><figcaption></figcaption></figure>

Available MINER\_URL Options:

* North America: stratum+ssl://ironssl-na.f2pool.com:1510
* Europe: stratum+ssl://ironssl-euro.f2pool.com:1510
* Asia: stratum+ssl://ironssl-asia.f2pool.com:1510

For complete Iron mining instructions, visit[ F2pool’s Iron Fish guide](https://f2pool.io/mining/guides/how-to-mine-iron-fish/).

### 3.3 Provider Selection <a href="#a831" id="a831"></a>

After setting up your environment variables, you’ll need to choose the hardware specifications for your mining operation.

<figure><img src="https://miro.medium.com/v2/resize:fit:700/0*zc0_x7GTIc6XlZa6" alt="" height="398" width="700"><figcaption></figcaption></figure>

Swan Chain offers two distinct approaches for selecting a Computing Provider (CP):

1. **Automatic Matching**

* System automatically matches you with suitable Computing Providers
* Recommended for new users

**2. Manual Provider Selection**

* Filter providers based on Geographic location, Hardware pricing and Provider account ID
* **PRO TIP**: Check provider performance metrics via their account ID in the provider dashboard
* **NOTE**: This option gives you more control but requires understanding of provider metrics

<figure><img src="https://miro.medium.com/v2/resize:fit:700/0*_FJH5EGnuiluoYgc" alt="" height="314" width="700"><figcaption></figcaption></figure>

### 4. Final Deployment <a href="#ab5d" id="ab5d"></a>

* Review all configurations thoroughly
* Click “Deploy Now” to initiate the mining task

**Note:** If you see the “Not sufficient balance” notification, please follow the “Managing your SWANU accounts” steps to transfer SWANU.

<figure><img src="https://miro.medium.com/v2/resize:fit:700/0*DuZLFLjsRD5tXhUm" alt="" height="232" width="700"><figcaption></figcaption></figure>

Once you’ve deployed your mining task, give it 3–5 minutes, then head over to your F2pool dashboard. If you see hashrate and mining data showing up, congrats! Your mining task is up and running.

<figure><img src="https://miro.medium.com/v2/resize:fit:700/0*bcefwN3rs3-wMm3W" alt="" height="319" width="700"><figcaption></figcaption></figure>

### 5. Manage Instances  <a href="#id-71f9" id="id-71f9"></a>

Through the Swan Console, you can:

* Monitor deployment status in real time
* View detailed performance metrics
* Terminate mining tasks if needed

### 5.1 **Deployment Dashboard Overview** <a href="#id-71f9" id="id-71f9"></a>

To monitor your deployments on Swan Chain Console:

1. Navigate to “Instance” in the left panel
2. Click on "Instance" or “Mining Task” to view your deployment status
3. Here you can monitor:
   1. Task running status
   2. Resource usage
   3. Connection status
   4. Cost

<figure><img src="https://miro.medium.com/v2/resize:fit:700/0*GBD0Qgdu-sGz4CkS" alt="" height="284" width="700"><figcaption></figcaption></figure>

**Note:** Costs are calculated and deducted every 12 hours. Make sure to maintain sufficient balance in your Escrow account to cover these periodic charges

### 5.2 Terminate **Active Deployment** <a href="#a2ad" id="a2ad"></a>

To close an active deployment:

1. Navigate to the Instance pane
2. Select the target deployment
3. Click the "Terminate" button
4. Confirm the blockchain transaction

<figure><img src="../../../.gitbook/assets/image (192).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (193).png" alt=""><figcaption></figcaption></figure>

Once terminated:

* All associated costs will cease
* Locked SWANU will be released
* Resources will be deallocated
