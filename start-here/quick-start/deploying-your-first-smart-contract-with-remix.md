# Deploying Your First Smart Contract with Remix

Smart Contracts are written in Solidity, a statically-typed programming language designed for Ethereum and other EVM-compatible chains. This tutorial uses [Remix](https://remix.ethereum.org/#lang=en\&optimize=false\&runs=200\&evmVersion=null), a browser-based IDE that requires minimal setup.

**1.Open Remix IDE:** Navigate to [Remix](https://remix.ethereum.org/).

**2.Create a New Contract:** In the workspace, create a new file with the ".sol" extension, e.g., "MyFirstContract.sol".

**3.Code a Basic Contract:**

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract MessageContract {
    string private message;

    function writeMessage(string calldata newMessage) public {
        message = newMessage;
    }

    function readMessage() public view returns (string memory) {
        return message;
    }
}
```

Remix automatically compiles the contract when saved. Ensure there are no compilation errors.

**4.Compile Manually (Optional):** Click the "Advanced Configurations" tab and select the Solidity version `london`.

<figure><img src="../../.gitbook/assets/image (2) (1) (1).png" alt=""><figcaption></figcaption></figure>

**5.Deploy the Contract:** Click the "Deploy" icon logo (it looks like an Ethereum logo with an arrow pointing to the right).



<figure><img src="https://lh7-us.googleusercontent.com/ipWX5Hf3N3dLz03VSskdtYs5Y_8wUjy-XAriWpYbZpsX8jucpNBqOMu5sNomIfa8Q3oBCzvqtVsIAlBAsbFVvDC7cERlspm80AbtZEI-oJtWq3mN87yxCagZr1LiXnlK8rbt3b8AqgkAmmkncjpDesw" alt=""><figcaption></figcaption></figure>

**6.Configure Deployment:**

* Select the basic contract.
* Under "Environment," choose "Injected Provider - MetaMask."
* Connect Remix to MetaMask.

<figure><img src="https://lh7-us.googleusercontent.com/IBSDB3_h_dspP8TmnYITbxN6evUr3qi8fkSmi9esFp8lkqNWNCFRh4DoNON58N4Lip38QR9SarNebFAXiIa4VpdRMFgfOPCFbghccUMBi_C6bt0NvVNQlT7XD3bSqTJy5O0Z_Fl7TaO7WDieYtX1gXA" alt=""><figcaption></figcaption></figure>

**7.Initiate Deployment:** Click "Deploy" and confirm the MetaMask transaction.

<figure><img src="../../.gitbook/assets/image (187).png" alt=""><figcaption></figcaption></figure>

**8.Interact With Your Contract:**

Step1: After deployment, find your contract under "Deployed Contracts."

<figure><img src="https://lh7-us.googleusercontent.com/ll7xe7898NMnbD0klTFPS1lh31XCBPU4tALhEju4Fbz1wot78quV-bhKIXeW-LybFehbPUHHcTEkQ-E_eiGbpZA8t9mQaygJOT9BNDhekkvX09qzvevv3QPe5w6KaAOo6OVL93a4Sbzt1UMnPBnS5Eo" alt=""><figcaption></figcaption></figure>

Step2: Expand the contract and call `writeMessage` by entering a message and clicking the button.

<figure><img src="https://lh7-us.googleusercontent.com/4VaxPuh7P7C0ovrielVb-VRSH89SUmfJngYVpGdDSbA_cK2xu8wm_WFpAsWjXP6nvtnQqvhw3CDWogzlFcf7R549RxJ0IjNM9FqMBgu34pwGXIA1B-vaSlHLljXUVn9Rd-LBP6YjvLIaoVoqhtVKbFk" alt=""><figcaption></figcaption></figure>

<figure><img src="https://lh7-us.googleusercontent.com/pxJUuzfByMz7j1KVU9MDAIxG1S1-2cWTtwUTJgZHreFpVq6KHuh6rnOM-xs_fTrT5KkSYwbo3CMcS5YksjMWmA51TDzKBBLSKv1WzC8TGdP0WgKytCZdv-5O_ENyRZwqU7phYIQI6TKRW7sMPETgLXs" alt=""><figcaption></figcaption></figure>

Step3: Confirm the MetaMask transaction.

**Step4: Read the Message:**

Click the blue "readMessage" button to read the on-chain message.

<figure><img src="https://lh7-us.googleusercontent.com/Og4j16JQNEGhVqxe0p_t9B4prneNgTh_SxOr0g3hOU8q_uZ_7eUxCYIf6nj1LSGxQ4_Is3p_lE3IVGV7qLarKK9c9xZo93LBdep6PmKfsy_MHzJrB5o8XBfnw5e1A__F55nqaKJ-_VCsRyZmO-IGqG0" alt=""><figcaption></figcaption></figure>

Now you've successfully deployed and interacted with your first smart contract using Remix and MetaMask.
