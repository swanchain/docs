# Legacy: Swan 1.0

{% hint style="warning" %}
Everything in this section describes **Swan 1.0** — the network before the Inference Cloud. It is kept for existing operators and for the historical record, and is **no longer maintained**: external links may be dead, programs have ended, and the economics described here (UBI, ECP/FCP roles, task auctions) have been replaced. For anything current, start at [Welcome](../README.md).
{% endhint %}

## What changed

| Swan 1.0 | Swan 2.0 (current) | Where to read about it |
|---|---|---|
| Two provider roles, **ECP** (ZK proofs) and **FCP** (Kubernetes tasks) | One **Computing Provider** that serves AI inference over a WebSocket connection — no public IP, domain or TLS needed | [Become a Provider](../core-concepts/swan-2.0-inference-cloud/become-a-provider.md) |
| Providers paid mainly from **UBI** — a daily SWAN allocation sampled across registered hardware | Providers paid **per token** at a published payout price for every request they serve | [How the Marketplace Works](../core-concepts/market-provider/inference-marketplace.md) |
| Consumers bought compute through **task auctions** (Orchestrator, Lagrange, Swan Console) | Consumers call an **OpenAI-compatible API** and pay per token from a credit balance or a Token Plan | [For Developers](../core-concepts/swan-2.0-inference-cloud/how-to-use.md) |
| Collateral sized by **Computing Units** (CU × 3,533 SWAN) | Collateral deposited per provider account, see the current rules | [Earnings and Collateral](../core-concepts/token/computing-provider-collateral/README.md) |

## Still running a legacy CP?

The ECP/FCP pages below remain the reference for **withdrawing collateral and exiting** the legacy network. The recommended path for the hardware itself is to install the current [`computing-provider`](https://github.com/swanchain/computing-provider) and join the inference network — the [provider guide](../core-concepts/swan-2.0-inference-cloud/become-a-provider.md) takes about fifteen minutes.

## In this section

* [UBI Allocation Curve](../core-concepts/token/swan-universal-basic-income-ubi/README.md) — the Swan 1.0 reward model
* [Swan CP UBI](swan-cp-ubi.md) — the UBI program as it applied to ECP/FCP operators
* [Legacy Computing Provider (ECP/FCP)](../bulders/computing-provider/README.md) — setup, funding and exit procedures
* [Market Provider](../bulders/market-provider/README.md), [Storage Provider](../bulders/storage-provider/README.md) — the auction-era marketplaces
* [App Deployment and Storage](../bulders/app-developer/README.md), [Developer Tools](../bulders/tools/README.md) — Lagrange/LDL deployments, Swan SDK, Swan Console, IPFS storage
* [Mission 3.0](../bulders/mission-3.0/README.md) and [Past Campaigns](past-campaigns.md) — completed community programs and testnets
