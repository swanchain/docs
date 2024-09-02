---
description: Now all CPs can migrate to the mainnet to participate in Swan Mainnet Campaign
---

# Migrating FCP to Swan Mainnet

## How to Upgrade and Migrate to Swan Mainnet

### First-time Deployment

If you are deploying for the first time, you can follow the instructions in the latest version: [Swan Chain Computing Provider v0.6.1](https://github.com/swanchain/go-computing-provider/blob/v0.6.1/README.md)

### Migration from Proxima to Mainnet

1. **Update `resource-exporter` to the latest version `v11.2.8`**: Follow the instructions in this issue: [Update Resource Exporter to v11.2.8](https://github.com/swanchain/go-computing-provider/issues/14).
2.  **Specify a new CP\_PATH**:

    ```bash
    export CP_PATH="/YOUR/CP/PATH"
    ```
3.  **Download the mainnet version of the computing-provider**:

    ```bash
    wget https://github.com/swanchain/go-computing-provider/releases/download/v0.6.1/computing-provider
    ```
4.  **Verify CP version**:

    ```bash
    computing-provider -v
    ```

    Ensure it shows `version 0.6.1+mainnet`.
5.  **Initialize CP repo and update configuration**: Refer to [Initialize CP Repo and Update Configuration](https://github.com/swanchain/go-computing-provider/tree/v0.6.1?tab=readme-ov-file#initialize-cp-repo-and-update-configuration)

    Note:

    * No need to modify parts of the configuration file with default values.
    * The "contract address" is now built into the program, no separate configuration is needed.
    * The default configuration file template can be found [here](https://github.com/swanchain/go-computing-provider/blob/v0.6.1/config.toml.sample).
6. **Initialize a Wallet and Deposit SwanETH**: Refer to [Initialize a Wallet and Deposit SwanETH](https://github.com/swanchain/go-computing-provider/tree/v0.6.1?tab=readme-ov-file#initialize-a-wallet-and-deposit-swaneth).
7. **Initialization CP Account**: Refer to [Initialization CP Account](https://github.com/swanchain/go-computing-provider/tree/v0.6.1?tab=readme-ov-file#initialization-cp-account).
8. **Collateral SWANC for FCP**: Refer to [Collateral SWANC for FCP](https://github.com/swanchain/go-computing-provider/tree/v0.6.1?tab=readme-ov-file#collateral-swanc-for-fcp).
9. **Withdraw SWANC from FCP**: Refer to [Withdraw SWANC from FCP](https://github.com/swanchain/go-computing-provider/tree/v0.6.1?tab=readme-ov-file#withdraw-swanc-from-fcp).
10. **Start the Computing Provider**: Refer to [Start the Computing Provider](https://github.com/swanchain/go-computing-provider/tree/v0.6.1?tab=readme-ov-file#start-the-computing-provider).

### Mainnet Changes

1. **Different Compilation Method**:
   * Mainnet version: `make mainnet`
   * Testnet version: `make testnet`
2. **Different Collateral**:
   * In the mainnet, FCP collateral is SwanC. Each task requires 5 SwanC collateral (claim from [https://faucet.swanchain.io](https://faucet.swanchain.io)).
3. **Different Task Distribution Platform**:
   * Mainnet task distribution platform: [https://lagrange.computer](https://lagrange.computer), same as the testnet lagrangeDAO platform.
4. **Funds Operations**:
   * Refer to [Funds Operations Guide](https://docs.swanchain.io/swan-provider/computing-provider-cp/fog-computing-provider-fcp/fcp-token-operations-guide).
5. **ECP (Edge Computing Provider)**:
   * The new version will support ECP running independently or with FCP, allowing FCP to earn both rewards simultaneously.
   * ECP tasks are more frequent but offer lower rewards.

For any questions, refer to the detailed documentation on the [Swan Mainnet Campaign](https://docs.swanchain.io/swan-chain/swan-chain-mainnet/swan-provider-campaign).
