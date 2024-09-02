# Edge Computing Provider (ECP)

## Edge Computing Provider (ECP)

**ECP (Edge Computing Provider)** specializes in processing data at the source of data generation, using minimal latency setups ideal for real-time applications. This provider handles specific, localized tasks directly on devices at the network’s edge, such as IoT devices.

At the current stage, ECP supports the generation of **ZK-Snark proof of Filecoin network**, and more ZK proof types will be gradually supported, such as Aleo, Scroll, starkNet, etc

\
**ECP hardware requirements:**

* Possess a public IP
* Have at least one GPU
* At least 4 vCPUs
* Minimum 300GB HDD storage
* Minimum 32GB memory
* Minimum 20MB bandwidth

#### ECP (Edge Computing Provider) Status:

The ECP (Edge Computing Provider) status indicates the current operational state of the provider:

* **Inactive**: Previously had an ECP taskType, but no longer does.
* **Online**: Has an ECP taskType, query is successful, sufficient collateral, and not rejecting tasks (Normal operation).
* **Offline**: Has an ECP taskType, but query is unsuccessful.
* **NSC (Not Sufficient Collateral)**: Has an ECP taskType, but insufficient collateral.
* **Declined**: Has an ECP taskType, but rejecting tasks (due to insufficient resources or sequencer).
* **Inconsistent**: Local information does not match on-chain information (e.g., CP account, multiaddress, nodeID).
