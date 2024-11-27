# Getting Started

### Prerequisites <a href="#ef4e" id="ef4e"></a>

* A crypto wallet (MetaMask recommended)
* SWANU tokens
* A  pool account (e.g., F2Pool for Aleo Blockchain GPU task in this guide)

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
* Required for deploying Blockchain GPU tasks

**Important**: The system automatically deducts fees every 24 hours and transfers them directly to the Computing Provider’s beneficiary address

**Locked Balance**

* A portion of your Escrow Balance that ensures the application can run for at least 12 hours
* Will be released after the application is closed or terminated

**Note**: Before deploying any Blockchain GPU task, your Escrow Balance must have sufficient funds to cover the initial 12-hour locked period

### Critical Account Operations

#### Daily Settlement Process

* Settlement occurs at UTC 00:00 daily
* System automatically deducts fees from Escrow Balance
* Fees are transferred to corresponding Computing Provider's beneficiary address

**Important**: Insufficient balance will trigger automatic task termination

#### Low Balance Protection

* System actively monitors Escrow Balance levels
* When the balance insufficient for current deployment costs:
  * All running tasks will be automatically terminated
  * System prevents new task deployments

**Critical**: Maintain sufficient Escrow Balance to avoid service interruption

### Balance Management Steps <a href="#ba4f" id="ba4f"></a>

To use the platform, you must transfer SWANU from your external wallet to your Available Balance.

1. **Deposit Process**

* Navigate to “Account” in the left panel
* Click “Recharge” to transfer SWANU from your wallet to Available Balance
* Locate the arrow icon between the balance accounts and click to transfer from “Available Balance” to “Escrow Balance”

<figure><img src="https://miro.medium.com/v2/resize:fit:700/0*oVeOPBtONbpS5vbP" alt="" height="189" width="700"><figcaption></figcaption></figure>

**2. Withdrawal Process**

**CRITICAL NOTE:** Withdrawals follow a strict two-step process with mandatory waiting period and specific restrictions.

<figure><img src="https://miro.medium.com/v2/resize:fit:700/0*5yVg-76xPLHGexhe" alt="" height="330" width="700"><figcaption></figcaption></figure>

1. **Escrow to Available Balance**

* Step 1: Request Withdrawal
  * Click the arrow icon (right to left) to transfer from Escrow to Available Balance
  * Status will show as "requested" in the "Transfer" section
* Step 2: Confirm Withdrawal (after 7 days)
  * After 7-day waiting period, confirm the transfer in "Transfer" section
  * Once successful, funds will appear in Available Balance

<figure><img src="https://miro.medium.com/v2/resize:fit:700/1*n4tkbtMlu4XTaxBpV6Q8cA.png" alt=""><figcaption></figcaption></figure>

2. **Available Balance to Wallet**

* Simply click the "Withdraw" button in Available Balance card to withdraw to wallet
* Monitor transaction status in withdrawal history

<figure><img src="https://miro.medium.com/v2/resize:fit:700/1*NUx8UWjJ2RKvaGz3BHaEBA.png" alt="" height="198" width="700"><figcaption></figcaption></figure>

{% hint style="info" %}
**Important Withdrawal Rules:**

* Only one active withdrawal request is permitted per account
* New withdrawal requests automatically invalidate previous ones
* Recommended: Stop all tasks 24 hours before requesting withdrawal
* Ensure sufficient balance for final settlements
{% endhint %}
