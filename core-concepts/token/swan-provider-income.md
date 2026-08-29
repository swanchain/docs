# Computing Provider Income

{% hint style="info" %}
**How providers earn today (Swan 2.0):** per token, at the payout price published for each model — see [Income under Swan 2.0](#swan-2.0-market-driven-income) below. The UBI formulas that follow describe **Swan 1.0** and are kept for reference; UBI has **ended** under [SIP-003](https://github.com/swanchain/governance/discussions/21).
{% endhint %}

## Swan 1.0: UBI income (historical)

Swan Chain is a decentralized network that connects computing providers with users requiring computational resources. To foster early network growth and incentivize CPs to join and contribute resources, a dual compensation mechanism has been designed:

1. **Universal Basic Income (UBI)**: Provides CPs with a predictable token income when their resources are underutilized.
2. **Paid Jobs**: Offers market-priced compensation for computational tasks requested by users.

This mechanism ensures a fair and gradual distribution of tokens to providers, supporting the network's expansion until it reaches a critical mass of user-paid tasks. Importantly, the UBI distribution rate is influenced by the resource usage rate, and CPs earn market-based compensation when engaged in paid jobs.

### **Total Income**

The total daily income $$I(x)$$for a computing provider on day $$( x )$$ comprises two components:&#x20;

* **UBI Income** $$y_{\text{UBI}}(x)$$
* **Paid Job Income** $$y_{\text{Paid}}(x)$$

$$
I(x) = y_{\text{UBI}}(x) + y_{\text{Paid}}(x)
$$

Substituting the expressions for $$y_{\text{UBI}}(x)$$ and $$y_{\text{Paid}}(x)$$

$$
I(x) = A \cdot x^{B} \cdot e^{-C x} \cdot (1 - u(x)) + P_{\text{market}}(x) \cdot u(x)
$$

#### **Resource Usage Rate Impact**

* **When** $$u(x) = 0$$:
  * CP receives full UBI allocation.
  * No income from paid jobs.
* **When** $$u(x) = 1$$:
  * All resources are utilized by paid jobs.
  * CP receives full income from paid jobs.
  * No UBI allocation.
* **Intermediate Values**:
  * CP's income is a combination of UBI and paid job compensation, proportional to resource utilization.

### Individual CP's UBI

To calculate the UBI for a single CP, we consider both the resource usage and completion rates of tasks. UBI allocation is conditional on sufficient resource contribution and performance metrics:

**(1) UBI Workload Calculation**

* Calculate the daily completion rate of a single ECP zk-task: $$P_{\text{ECP}}$$
* Calculate the completion rate of a single FCP sampling task: $$P_{\text{FCP}}$$
* Number of GPUs: $$N_{\text{ECP}}(GPU_k)$$ and GPU types.&#x20;
* Calculate the total UBI workload:

$$
UBI_{\text{total}} = UBI_{\text{ECP}} + UBI_{\text{FCP}}
$$

$$
UBI_{ECP}=\sum\limits_i (\sum\limits_k N_{ECP,i}(GPU_k) \times f_k)
$$

$$
UBI_{FCP}=\sum\limits_j (\sum\limits_k N_{FCP,j}(GPU_k) \times f_k) *W_{FCP}
$$

**(2) Calculating the UBI for a single CP**:

As an ECP:

$$
UBI_{\text{ECP},i}(x) = \frac{\sum\limits_k N_{\text{ECP},i}(GPU_k) \times f_k \times P_{\text{ECP},i}}{UBI_{\text{ECP}} + UBI_{\text{FCP}}} \times y_{\text{UBI}}(x)
$$

As an FCP:

$$
UBI_{FCP,i}(x)= \frac{\sum\limits_k N_{FCP,i}(GPU_k) \times f_k \times P_{FCP,i} \times W_{FCP} }{UBI_{ECP}+UBI_{FCP}} \times y_{\text{UBI}}(x)
$$



***

### Conditions for CP to Receive UBI

A CP must meet certain conditions to qualify for UBI:

1. **Sufficient Collateral**:

$$
Collateral_{ECP}= \sum\limits_k N_{ECP}(GPU_k) \times C_{base} \times f_k
$$

$$
Collateral_{FCP}= \sum\limits_k N_{FCP}(GPU_k) \times C_{base} \times f_k  \times W_{FCP}
$$

Where:

* $$N_{ECP}(GPU_k)$$ represents the number of ECP for $$GPU_k$$
* $$C_{\text{base}}$$ is the base collateral, with an initial value of 3533 (this value will be dynamically adjusted based on the daily computing units of the entire network; for specific adjustment rules, check [here](computing-provider-collateral/collateral-requirement-and-earning-multiplier.md))
* $$N_{\text{FCP}}(\text{GPU}_k)$$represents the number of $$\text{GPU}_k$$ _in FCP_
* &#x20;$$N_{\text{ECP}}(\text{GPU}_k)$$ represents the number of $$\text{GPU}_k$$ in ECP.&#x20;
* $$W_{FCP}$$ represents the FCP resource bonus ratio, currently set at a constant value of 1.2

{% hint style="info" %}
**NOTE:** The value of $$W_{FCP}$$, 1.2, means that if the same configuration of servers is deployed for FCP, it will generate 20% more earnings than ECP.
{% endhint %}

2. **Completion of Basic Test Tasks:**

* FCP: Sampling task
* ECP: ZK task

3. GPU count and type are also factored into the UBI eligibility.

#### Exit Mechanism:

* CP Exit Mechanism If a CP wishes to exit, they must set `taskType` = 100.
  * The CP will no longer receive any tasks and will not incur any collateral deductions.
  * The CP will no longer appear on [the current dashboard list.](https://provider.swanchain.io/overview)
* CPs can request to withdraw their collateral, but this requires a 7-day confirmation period to ensure settlement before the withdrawal is finalized (first `requestWithdraw`, followed by `confirmRequest` after 7 days).

***

## Income under Swan 2.0 <a href="#swan-2.0-market-driven-income" id="swan-2.0-market-driven-income"></a>

Under the [Inference Cloud](../swan-2.0-inference-cloud/README.md) a provider's income is the sum of what it is paid for the requests it actually serves. There is no allocation for registered-but-idle hardware.

### Per-token payout

Every catalog model publishes a **payout price** per 1M input and output tokens (per image for image models, per minute for audio), set by the platform alongside the consumer price. For each request:

$$
\text{earning} = \frac{\text{input\_tokens} \times \text{payout\_input\_price} + \text{output\_tokens} \times \text{payout\_output\_price}}{10^6}
$$

The platform margin is the spread between the consumer price and the payout price; there is no percentage commission. Payout prices are visible in the provider dashboard and in the public catalog (`payout_input_price`, `payout_output_price` in `GET /api/v1/models`). At the time of writing the payout is 90% of the consumer price for almost every model. Providers do not set prices; they choose which models to serve. `computing-provider inference recommend-models` ranks models by current demand against your hardware.

### Token Plan traffic

Requests covered by a consumer's Token Plan credit the provider at the same payout price, but the month's total plan payouts are capped at the plan revenue pool (subscribers × $6). If plan usage costs more than the pool, payouts are pro-rated across providers by their share of plan tokens served. These earnings are held as *settlement pending* until month end; pay-as-you-go earnings are not.

### What moves your income

| Factor | Effect |
|--------|--------|
| **Being online for models people use** | Traffic is only routed to online providers; demand is visible per model on the [network page](https://inference.swanchain.io/network) |
| **Health** | Success rate, uptime and latency weight how much of a model's traffic you receive; degraded providers get only probe traffic |
| **Verification trust** | Passing fingerprint/logprob/context checks raises routing weight; failures lower it and can trip a circuit breaker |
| **Honest context window** | Declaring the window you really serve makes you eligible for the long-context requests you can handle and avoids caps |
| **Reference-node status** | A node the platform designates as a model's reference baseline earns nothing for that model |

### Settlement and payouts

* Earnings accrue per request and appear in the dashboard (daily/weekly/monthly, per model, CSV export).
* Settlement aggregates usage into daily batches per provider and collateral chain.
* Payout requests go to the beneficiary wallet: **minimum $10**, **flat $1 fee**, one request per chain per hour, one pending payout at a time. Earnings can also be converted into inference credit on the same account.

### UBI has ended

The Swan 1.0 UBI allocation was tapered to zero under [SIP-003](https://github.com/swanchain/governance/discussions/21) and is off. There is no daily SWAN allocation for registered hardware; the per-token payout above is a provider's income.

### Unified Computing Provider role

The legacy ECP and FCP roles are merged into a single Computing Provider. Operators of legacy providers can install the current [`computing-provider`](https://github.com/swanchain/computing-provider) and follow [Become a Provider](../swan-2.0-inference-cloud/become-a-provider.md); the legacy pages under [Legacy: Swan 1.0](../../swan-chain-campaign/README.md) remain for withdrawing Swan 1.0 collateral.
