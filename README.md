# Welcome

Swan Chain is a decentralized **AI inference cloud**. Developers call one OpenAI-compatible API and get frontier models and open-source models served by a network of independent GPU providers; providers earn per token for the requests they serve. The marketplace lives at [**inference.swanchain.io**](https://inference.swanchain.io).

{% hint style="info" %}
**Try it in a minute.** The [playground](https://inference.swanchain.io/playground) runs a live model with no account. When you are ready, [sign up](https://inference.swanchain.io/signup), copy your API key, and point any OpenAI SDK at `https://inference.swanchain.io/v1`. The [live catalog](https://inference.swanchain.io/models) shows every model with its current price, context window and how many providers are serving it right now.
{% endhint %}

## Start here

<table data-view="cards"><thead><tr><th></th><th></th><th data-hidden data-card-target data-type="content-ref"></th></tr></thead><tbody><tr><td><strong>Use AI models</strong></td><td>Playground, account, API key, credits, first request. Everything a developer needs in one page.</td><td><a href="core-concepts/swan-2.0-inference-cloud/how-to-use.md">how-to-use.md</a></td></tr><tr><td><strong>Earn with your GPU</strong></td><td>Run a model server, connect the open-source <code>computing-provider</code>, pass verification, deposit collateral, get paid per token.</td><td><a href="core-concepts/swan-2.0-inference-cloud/become-a-provider.md">become-a-provider.md</a></td></tr><tr><td><strong>API reference</strong></td><td>Every endpoint, header, limit and error code, with copy-paste examples in curl, Python, Node and Go.</td><td><a href="bulders/app-developer/swan-inference-api.md">swan-inference-api.md</a></td></tr><tr><td><strong>How the marketplace works</strong></td><td>Routing, the two-price model, settlement, verification and the Token Plan — for anyone who wants to know what happens behind a request.</td><td><a href="core-concepts/market-provider/inference-marketplace.md">inference-marketplace.md</a></td></tr></tbody></table>

## Live network

| | |
|---|---|
| Marketplace | [inference.swanchain.io](https://inference.swanchain.io) |
| Model catalog and prices | [inference.swanchain.io/models](https://inference.swanchain.io/models) |
| Providers, models and traffic in real time | [inference.swanchain.io/network](https://inference.swanchain.io/network) |
| Pricing and Token Plan | [inference.swanchain.io/pricing](https://inference.swanchain.io/pricing) |
| Provider software (open source) | [github.com/swanchain/computing-provider](https://github.com/swanchain/computing-provider) |
| Governance proposals (SIPs) | [github.com/swanchain/governance](https://github.com/swanchain/governance) |

## Build on Swan Chain

Swan Chain is also an Ethereum Layer 2 (chain ID **254**). If you are deploying contracts or running a node rather than calling models, start with the [network reference](network-reference/readme/README.md) and the [DApp developer guide](bulders/dapp-developer/README.md).

<table data-view="cards"><thead><tr><th></th><th data-hidden data-card-target data-type="content-ref"></th></tr></thead><tbody><tr><td><strong>Network info</strong> — chain IDs, RPC endpoints, explorers, token addresses</td><td><a href="network-reference/readme/README.md">README.md</a></td></tr><tr><td><strong>Contract addresses</strong> — L2 infrastructure and the inference contracts</td><td><a href="network-reference/contract-addresses.md">contract-addresses.md</a></td></tr><tr><td><strong>Deploy a contract</strong> — first smart contract with Remix</td><td><a href="bulders/dapp-developer/deploying-your-first-smart-contract-with-remix.md">deploying-your-first-smart-contract-with-remix.md</a></td></tr></tbody></table>

{% hint style="warning" %}
**Looking for ECP/FCP, ZK tasks, UBI, Lagrange or a past testnet?** That material describes Swan 1.0 and is kept under [Legacy: Swan 1.0](swan-chain-campaign/README.md) for existing operators. It is no longer maintained.
{% endhint %}
