# Computing Provider(CP) Account

### Overview

`CPAccount` is a smart contract account based on Swan Chain, designed for managing and registering computing providers (CP). Each computing provider that connects to Swan Chain must create a unique account. When an account is created, the `CPAccount` automatically registers with the CP Account Register Contract.

### Features

The contract account provides a range of functions, including ownership management, worker and beneficiary information management, task type updates (supporting extension for additional task types), and multi-address updates. The contract also includes an event mechanism to notify external systems about changes in ownership, worker, multi-address, and task types.

#### Key Functions

* **Ownership Management**: Allows the owner to change the contract's owner address.
* **Worker Management**: Allows the owner to change the worker address.
* **Beneficiary Management**: Allows the owner to change the beneficiary address.
* **Multi-address Management**: Allows the owner to update the multi-address array.
* **Task Type Management**: Allows the owner to update the task type array, supporting extension for additional task types.

#### Account Roles

To ensure the security of the CP Account, the roles are categorised into:

* **Owner**: Manages the CP Account and has permission to change account information such as multi-addresses, worker address, and beneficiary address. The private key of the owner's address does not need to be present on the server for security reasons.
* **Worker**: This address is used for submitting task results and needs to be funded with a certain amount of sETH to pay for gas fees.
* **Beneficiary**: This address receives all earnings from the CP Account and is solely used for receiving funds. For security purposes, the private key of the beneficiary's address should not be stored on the server to maintain isolation.

#### Event Mechanism

* **OwnershipTransferred**: Ownership transfer event, notifying external systems of changes in the owner address.
* **WorkerChanged**: Worker change event, notifying external systems of changes in the worker address.
* **MultiaddrsChanged**: Multi-address change event, notifying external systems of changes in multi-addresses.
* **BeneficiaryChanged**: Beneficiary change event, notifying external systems of changes in the beneficiary address.
* **TaskTypesChanged**: Task type change event, notifying external systems of changes in task types.
* **CPAccountDeployed**: CPAccount deployment event, notifying external systems of the creation and registration of the CPAccount.

### Source Code

Detailed source code for the `CPAccount` contract can be found [here](https://github.com/swanchain/market-providers/tree/main/computing-provider/account).

### Summary

The `CPAccount` contract provides a robust framework for managing and registering computing providers on Swan Chain. It facilitates secure management of ownership, worker, and beneficiary roles, with flexible support for extending task types. Through its comprehensive functionality and event-driven architecture, it ensures transparency and security in CP Account operations.
