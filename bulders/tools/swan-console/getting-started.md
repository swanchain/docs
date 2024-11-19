# Getting Started

### Prerequisites <a href="#ef4e" id="ef4e"></a>

* A crypto wallet (MetaMask recommended)
* SWANU tokens
* A mining pool account (e.g., F2Pool for Aleo mining in this guide)

### 1. Obtaining SWANU Tokens <a href="#id-5d1c" id="id-5d1c"></a>

#### 1.1 Purchase via DEX <a href="#id-2347" id="id-2347"></a>

You can acquire SWANU tokens through the DEX pool at[ ](https://www.geckoterminal.com/swanchain/pools/0x4fd0c2c5360d980d650fe02a19b1460b1802134b)IcecreamSwap: [https://www.geckoterminal.com/swanchain/pools/0x4fd0c2c5360d980d650fe02a19b1460b1802134b](https://www.geckoterminal.com/swanchain/pools/0x4fd0c2c5360d980d650fe02a19b1460b1802134b)

#### 1.2 Earn Through Computing Resources <a href="#id-7788" id="id-7788"></a>

Alternatively, earn SWANU by contributing your computing resources:

* Deploy ECP (Edge Computing Provider)
* Deploy FCP (Fog Computing Provider)

Learn more about CP deployment here: [https://docs.swanchain.io/bulders/computing-provider](https://docs.swanchain.io/bulders/computing-provider)

### 2. Managing Your SWANU Balance <a href="#id-6a71" id="id-6a71"></a>

Swan Chain Console features two distinct balance types:

<figure><img src="https://miro.medium.com/v2/resize:fit:700/0*DTVIX1gxYtcRZsEY" alt="" height="194" width="700"><figcaption></figcaption></figure>

**Available Balance**

* Primary account for storing credits
* Allows unrestricted transfers
* Functions as your main SWANU wallet within the platform

**Escrow Balance**

* Used for all operational costs
* Subject to transfer restrictions
* Required for deploying mining tasks
* **Important**: The system automatically deducts fees every 24 hours and transfers them directly to the Computing Provider’s beneficiary address

**Locked Balance**

* A portion of your Escrow Balance that ensures the application can run for at least 12 hours
* Will be released after the application is closed or terminated
* **Note**: Before deploying any mining task, your Escrow Balance must have sufficient funds to cover the initial 12-hour locked period

### Balance Management Steps <a href="#ba4f" id="ba4f"></a>

To use the platform, you must transfer SWANU from your external wallet to your Available Balance.

1. **Deposit Process**

* Navigate to “Account” in the left panel
* Click “Recharge” to transfer SWANU from your wallet to Available Balance
* Locate the arrow icon between the balance accounts and click to transfer from “Available Balance” to “Escrow Balance”

<figure><img src="https://miro.medium.com/v2/resize:fit:700/0*oVeOPBtONbpS5vbP" alt="" height="189" width="700"><figcaption></figcaption></figure>

**2. Withdrawal Process**

**CRITICAL NOTE:** The withdrawal process involves two steps and includes a mandatory waiting period.

<figure><img src="https://miro.medium.com/v2/resize:fit:700/0*5yVg-76xPLHGexhe" alt="" height="330" width="700"><figcaption></figcaption></figure>

**Step 1: Escrow to Available Balance**

* Click the arrow icon (right to left) between balance accounts
* **IMPORTANT:** A 7-day processing period required
* **CRUCIAL:** You must confirm the first withdrawal transaction within the “ Transfer” section after processing

<figure><img src="https://miro.medium.com/v2/resize:fit:700/1*n4tkbtMlu4XTaxBpV6Q8cA.png" alt="" height="362" width="700"><figcaption></figcaption></figure>

**Step 2: Available Balance to Wallet**

* Only proceed after completing Step 1
* Click the “Withdraw” button in the Available Balance card
* Monitor transaction status in withdrawal history

<figure><img src="https://miro.medium.com/v2/resize:fit:700/1*NUx8UWjJ2RKvaGz3BHaEBA.png" alt="" height="198" width="700"><figcaption></figcaption></figure>
