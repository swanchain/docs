# Fees

## How do network fees on Swan Chain work?[​](https://docs.base.org/docs/fees#how-do-network-fees-on-base-work) <a href="#how-do-network-fees-on-base-work" id="how-do-network-fees-on-base-work"></a>

Every transaction on Swan Chain consists of two costs: an L2 (execution) fee and an L1 (security) fee. The L2 fee is the cost to execute your transaction on the L2, and the L1 fee is the estimated cost to publish the transaction on the L1. Typically the L1 security fee is higher than the L2 execution fee.

The L1 fee will vary depending on the amount of transactions on the L1. If the timing of your transaction is flexible, you can save costs by submitting transactions during periods of lower gas on the L1 (for example, over the weekend)

Similarly, the L2 fee can increase and decrease depending on how many transactions are being submitted to the L2. This adjustment mechanism has the same implementation as the L1.

For additional details about fee calculation on Swan Chain, please refer to the [op-stack developer documentation](https://community.optimism.io/docs/developers/build/transaction-fees/).

## How to Reduce Gas Fees on Swan Chain with MetaMask

When interacting with the Swan Chain mainnet, you might notice that gas fees can sometimes be higher than expected. This guide will walk you through the process of optimizing your MetaMask settings to significantly reduce these fees

### Step-by-Step Guide

#### 1. Initiate a Transaction

Begin by starting a transaction on the Swan Chain network. When the MetaMask window pops up, don't confirm it immediately.

#### 2. Edit the Estimated Fee

Look for an "Edit" button or link next to the estimated fee. Click on this to open the fee editing window.

<figure><img src="../.gitbook/assets/DETAILS HEX.png" alt="" width="375"><figcaption></figcaption></figure>

#### 3. Access Advanced Settings

In the fee editing window, find and click on the "Advanced" option. This will reveal more detailed gas fee settings.

<figure><img src="../.gitbook/assets/Edit gas fee.png" alt="" width="375"><figcaption></figcaption></figure>

#### 4. Adjust Gas Fee Parameters

Now, you'll see two important fields:

* **Max base fee (GWEI)**
* **Priority fee (GWEI)**

Set both of these values to **0.0015.**

<figure><img src="../.gitbook/assets/= 0.00000018 SETH.png" alt="" width="375"><figcaption></figcaption></figure>

#### 5. Save as Default

This is a crucial step to save time in future transactions. Look for a checkbox that says something like "Save these values as my default for the Swan Chain network". Make sure to check this box.

#### 6. Confirm and Save

Click on the "Save" button to confirm your new settings.

#### 7. Complete the Transaction

Now, you can proceed with confirming your transaction. You should notice a significant reduction in the gas fee. You can also visit [swanscan.io](https://swanscan.io) to check your transaction fee. &#x20;

<figure><img src="../.gitbook/assets/Max fee 0.00000018 SETH.png" alt="" width="375"><figcaption></figcaption></figure>

### Results

After applying these settings, you should see a dramatic decrease in gas fees. For example:

* Before: Approximately $0.86&#x20;
  *

      <figure><img src="../.gitbook/assets/O Transaction Fee.png" alt=""><figcaption></figcaption></figure>
* After: Approximately $0.25&#x20;
  *

      <figure><img src="../.gitbook/assets/O Transaction Fee (1).png" alt=""><figcaption></figcaption></figure>

This represents a reduction of nearly two-thirds in gas costs!
