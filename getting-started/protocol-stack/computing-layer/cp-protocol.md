---
description: Computing Provider Protocol Introduction
---

# CP Protocol

The CP Protocol (Computing Provider Protocol) is specifically designed for computing providers (CPs) in the Swan network, aiming to standardize and protocolize the management and invocation of computing resources. A CP can be any third-party individual or organization that provides scalable cloud computing, storage, platform, and application services for enterprises to access on demand.

### Main Features

The CP Protocol is divided into two main categories: **Standard Protocol** and **Extension Protocol**. Each category is designed to meet specific business and technical needs and covers only the protocol layer. For specific code design, please refer to the corresponding SIP (Swan Improvement Proposal) document.

#### Standard Protocol

The core focus of the standard protocol is on standardizing the invocation and management of computing resources to ensure effective management. It includes the following key aspects:

* **Create job**: Specifies the standardized process for deploying tasks on CP resources.
* **Query job details**: Defines the protocol for accessing the execution status and relevant parameters of a task.
* **Renew job**: Provides the standard procedure for task renewal.
* **Terminate job early**: Sets the protocol standard for early termination of tasks.
* **Query CP resources**: Describes how to query and select available computing resources.

#### Extension Protocol

The extension protocol focuses on enhancing the security, customizability, and flexibility of resources. Specific functions include:

* **Query CP resource pricing**: Defines the standard method for querying CP resource pricing information.
* **Query CP whitelist**: Specifies how to manage and query a list of specific users.
* **Query CP blacklist**: Sets out how to maintain and query a blacklist to prevent malicious users from accessing resources.
* **Multi-chain payment verification mechanism**: Describes the verification protocol for using multi-chain payment methods to invoke CP resources.
* **Universal collateral/withdrawal mechanism**: Defines a standard protocol for a general collateral and withdrawal process to facilitate the access and management of market providers.

### Compatibility and Application Extension

According to the design of the CP Protocol, any type of market provider (MP, Market Provider) can develop and design a computing engine that meets their needs based on this protocol. This flexibility ensures that the CP Protocol is not only applicable to current computing providers but also provides foundational support for various new types of computing service models that may emerge in the future.

By introducing the CP Protocol, the Swan network not only enhances the efficiency and security of resource management but also provides flexible configuration options to accommodate the needs of various business scenarios.
