# Computing Provider (CP)

A Computing Provider (CP) is a third-party individual or organization that offers scalable computing resources, which businesses can access on demand over Swan network. These resources include cloud-based compute, storage, platform, and application services.

As a resource provider, you can run a **ECP** (Edge Computing Provider) and **FCP** (Fog Computing Provider) to contribute yourcomputing resource.

* **ECP (Edge Computing Provider)** specializes in processing data at the source of data generation, using minimal latency setups ideal for real-time applications. This provider handles specific, localized tasks directly on devices at the network’s edge, such as IoT devices. At the current stage, ECP supports the generation of **ZK-Snark proof of Filecoin network**, and more ZK proof types will be gradually supported, such as Aleo, Scroll, starkNet, etc. [Install Guideline](https://github.com/swanchain/go-computing-provider/blob/v0.4.6/ubi/README.md)
* **FCP (Fog Computing Provider)** Offers a layered network that extends cloud capabilities to the edge of the network, providing services such as AI model training and deployment. This provider utilizes infrastructure like Kubernetes (K8S) to support scalable, distributed computing tasks. **FCP** will execute tasks assigned by Market Provider, like [Orchestrator](https://orchestrator.swanchain.io/) on the [Swan chain](https://swanchain.io/).

For the current status of Swan Provider Network, please check here [https://orchestrator.swanchain.io](https://orchestrator.swanchain.io)
