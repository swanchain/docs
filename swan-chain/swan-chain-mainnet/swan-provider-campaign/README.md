# Swan Provider Campaign

## Introduction

The Swan Provider Campaign is designed to incentivize participants to contribute computing resources and workloads to the Swan Chain network. By participating, individuals and organizations can compete to earn their share of the 800,000 SWAN tokens prize pool.

### Duration

July 15, 00:00 (EST) - September 15, 23:59 (EST)

### Prize Pool Distribution

* **Total Prize Pool:** 800,000 SWAN tokens
  * **Edge Computing Provider (ECP):** 40% of the total prize pool, starting from July 29
  * **Fog Computing Provider (FCP):** 50% of the total prize pool, starting from July 15
  * **Market Provider (MP):** 10% of the total prize pool, starting from July 15

## Role of the Challenge

Participants will contribute computing resources to the Swan Chain network in the following capacities:

### Fog Computing Provider (FCP)

Fog Computing Providers (FCPs) extend cloud capabilities to the edge of the network, providing scalable and distributed computing services. They play a crucial role in handling AI model training and deployment, leveraging infrastructure like Kubernetes (K8S).

{% hint style="info" %}
**Hardware Requirements:**

* Public IP
* Domain name (\*.example.com)
* SSL certificate
* At least one GPU
* Minimum 8 vCPUs
* Minimum 100GB SSD storage
* Minimum 64GB memory
* Minimum 50MB bandwidth
{% endhint %}

**Links:**

* [FCP Deployment Guide](../../../swan-provider/cp-computing-provider/fcp-fog-computing-provider/computing-provider-setup.md)
* [FCP Event Details](../../../swan-provider/cp-computing-provider/fcp-fog-computing-provider/)

### Edge Computing Provider (ECP)

Edge Computing Providers (ECPs) are integral to the Swan Chain ecosystem, specializing in processing data at the source of data generation with minimal latency. They are essential for real-time applications, handling localized tasks directly on devices at the network’s edge, such as IoT devices.

{% hint style="info" %}
**Hardware Requirements:**

* Public IP
* At least one GPU
* Minimum 4 vCPUs
* Minimum 300GB HDD storage
* Minimum 32GB memory
* Minimum 20MB bandwidth
{% endhint %}

**Links:**

* [ECP Deployment Guide](../../../swan-provider/cp-computing-provider/ecp-edge-computing-provider/ecp-setup.md)
* [FCP Token Operations Guide](../../../swan-provider/cp-computing-provider/fcp-fog-computing-provider/fcp-token-operations-guide.md)
* [ECP Event Details](edge-computing-provider-ecp.md)

### Market Provider (MP)

Market Providers (MPs) are vital for the distribution and allocation of computing tasks. They facilitate an efficient marketplace where job requesters and computing providers can connect. MPs use an auction engine for job distribution and a payment engine for financial transactions.

**Links:**

* [MP Event Details](../../../swan-provider/mp-market-provider/)
