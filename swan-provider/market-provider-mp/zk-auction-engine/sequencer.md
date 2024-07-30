# Sequencer

The Sequencer plays a crucial role in:

* **Receiving Proofs:** Securely receives proofs from CPs.
* **Validating Proofs:** Checks the integrity, authenticity, and timeliness of proofs, including CP signatures.
* **Batching Proofs:** Aggregates proofs into blobs for efficient storage and processing.
* **Submitting Data:** Submits aggregated task data to the Swan Chain, creating AggregateTask contracts with blob CIDs.
* **Gas Fee Management:** Manages gas fees for CPs, maintaining separate accounts for gas costs and charging a small fee per task.
