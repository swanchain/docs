# Sequencer

The Sequencer plays a crucial role in:

* **Receiving Proofs:** Securely receives proofs from CPs.
* **Validating Proofs:** Checks the integrity, authenticity, and timeliness of proofs, including CP signatures.
* **Batching Proofs:** Aggregates proofs into blobs for efficient storage and processing.
* **Submitting Data:** Submits aggregated task data to the Swan Chain, creating AggregateTask contracts with blob CIDs.
* **Gas Fee Management:** Manages gas fees for CPs, maintaining separate accounts for gas costs and charging a small fee per task.

**Dynamic Pricing Strategy**

Due to significant increases in ETH mainnet gas prices, the Sequencer has implemented a dynamic pricing strategy to maintain stable L3 operations. The price per proof is calculated using the following formula:

$$
Gas = \frac{ \frac{Gas_{base}}{GasPrice_{base}}*GasPrice_{real}}{Count_{proof}}
$$

$$
Gas_{L3} =  \min(Gas, Gas_{max})
$$

Where:

* $$Gas_{base}$$ = 0.01 ETH
* $$GasPrice_{base}$$ = 20 Gwei
* $$GasPrice_{real}$$ = Current ETH mainnet gas price at the time of L3 chain interaction
* $$Gas_{max}$$ = 0.00001 ETH

For example, based on recent network conditions (as of November 12):

* Daily network-wide proof submissions ($$Count_{proof}$$) = 8,500
* Average gas price ($$GasPrice_{real}$$) = 25 Gwei

$$
Gas_{L3} = \min( \frac{ \frac{0.01}{20}*25 }{8500}, 0.00001)=0.000001470588235 ETH
$$

This results in an adjusted gas fee that ensures efficient operation while maintaining cost-effectiveness for all participants.

