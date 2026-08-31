# App Developer

{% hint style="info" %}
**Calling AI models?** You do not need a wallet or any blockchain setup. Start at [For Developers](../../core-concepts/swan-2.0-inference-cloud/how-to-use.md) and the [API Reference](swan-inference-api.md). The rest of this page covers the Swan 1.0 deployment tooling (Swan SDK, Lagrange/LDL, IPFS storage) and is kept for reference.
{% endhint %}

### Getting Started with Swan Chain (L2)

For the on-chain tooling below you will need to:

* [Set up your wallet](../../network-reference/readme/set-up-your-wallet.md)
* [Bridge tokens from Ethereum to Swan Mainnet](../../network-reference/readme/bridge-token.md)

### What You Can Do with Swan Chain

#### **AI Inference with Swan Inference API**

* Access frontier and open-source models (LLM, multimodal, image, audio, embedding) via an **OpenAI-compatible API** — see the [live catalog](https://inference.swanchain.io/models)
* Drop-in replacement — use any existing OpenAI SDK by changing the base URL to `https://api.swanchain.io/v1`
* Supports streaming, embeddings, image generation, and audio transcription

Get started with Swan Inference [here](swan-inference-api.md).

#### **Deploying with Swan SDK**

* The Swan SDK simplifies interactions with the Swan Chain Network Resource.
* Learn to create and manage computational tasks, retrieve hardware information, process payments, and monitor task statuses.

Explore Swan SDK deployment [here](deploying-with-swan-sdk.md).

#### **Store and Retrieve Files with Swan Storage**

* Utilize Multi-Chain Storage (MCS), Swan Chain's decentralized storage solution.
* Learn to install and configure the MCS SDK, manage storage buckets, and handle file operations.

Discover Swan Storage capabilities [here](store-and-retrieve-a-file-with-swan-storage/).

**Building and Pushing Docker Images**

* Learn how to create Docker images for your applications.
* Explore the process of pushing your Docker images to repositories.&#x20;

Get started with Docker [here](building-and-pushing-docker-images.md).

#### Creating Deployment Files with LDL

* The basics of LDL and its relationship to YAML
* How to create `deploy.yaml` files for your Swan Chain projects
* Key components of an LDL file: version, services, profiles, and deployment

Dive into LDL and deployment files [here](building-docker-images-and-deployment-file-with-ldl/creating-deployment-files-with-ldl.md).

***

These guides will empower you to harness the full potential of Swan Chain's ecosystem for your applications, from efficient computation management to secure, decentralized file storage.
