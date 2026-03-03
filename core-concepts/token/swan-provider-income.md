# Computing Provider Income

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

## Swan 2.0: Market-Driven Income <a href="#swan-2.0-market-driven-income" id="swan-2.0-market-driven-income"></a>

{% hint style="info" %}
The UBI model described above served as the **bootstrap phase** (Swan 1.0) that built the initial provider network. Swan 2.0 introduces contribution-based rewards that complement and gradually replace UBI. See [SIP-002](https://github.com/swanchain/governance/discussions/16) for the full proposal.
{% endhint %}

With the launch of the [Inference Cloud](../swan-2.0-inference-cloud.md), Computing Provider income transitions from UBI-only to a dual-income model where providers earn based on actual work performed.

### Dual Income Streams

Under Swan 2.0, providers earn through two complementary channels:

1. **Inference Revenue (Stablecoins)** — Direct payment in USDC for serving inference requests through the [Inference Marketplace](../market-provider/inference-marketplace.md)
2. **Contribution Rewards (SWAN Tokens)** — Daily SWAN token rewards allocated proportionally based on contribution score

When paid inference requests generate protocol revenue, the split is:

| Recipient | Share |
|-----------|-------|
| Provider | 70% (paid in request currency) |
| Protocol Treasury | 20% |
| SWAN Buyback & Burn | 10% |

### Contribution Score

Each provider receives a daily Contribution Score that determines their share of the SWAN token reward pool:

$$
\text{Contribution\_Score} = W_{inf} \times \text{norm}(\text{inferences}) + W_{tok} \times \text{norm}(\text{tokens}) + W_{up} \times \text{uptime} + W_{qual} \times \text{quality} + W_{div} \times \text{diversity}
$$

Where the weights are:

| Component | Weight | Description |
|-----------|--------|-------------|
| $$W_{inf}$$ | 0.30 | Inference volume — number of requests processed |
| $$W_{tok}$$ | 0.25 | Token throughput — total input + output tokens |
| $$W_{up}$$ | 0.20 | Uptime — 30-day uptime percentage |
| $$W_{qual}$$ | 0.15 | Quality — success rate adjusted by latency |
| $$W_{div}$$ | 0.10 | Model diversity — number of distinct models served |

And the component scores are:

$$
\text{norm}(x) = \frac{x}{\max(x_{\text{all providers}})}
$$

$$
\text{uptime\_score} = \frac{\text{uptime}_{30d}}{100}
$$

$$
\text{quality\_score} = \text{success\_rate} \times (1 - \text{norm}(\text{avg\_latency}))
$$

$$
\text{diversity\_score} = \frac{\text{models\_served}}{\text{max\_models\_in\_catalog}}
$$

### Minimum Contribution Thresholds

To prevent reward fragmentation and gaming:

| Threshold | Requirement | Effect if Not Met |
|-----------|-------------|-------------------|
| Minimum Uptime | 80% over 7 days | Excluded from contribution pool |
| Minimum Inferences | 100/week | Reduced to 50% contribution weight |
| Minimum Success Rate | 90% | Reduced to 75% contribution weight |
| Minimum Online Hours | 120 hours/week | Pro-rated availability bonus |

### 3-Phase Transition

The transition from UBI to contribution-based rewards follows an accelerated 3-month timeline:

**Phase 1: Hybrid Mode (Month 1)**

$$
\text{Daily\_Reward} = \text{UBI\_Base} \times 0.75 \times (1 - u) + \frac{\text{Score}_i}{\sum \text{Scores}} \times \text{Contribution\_Pool}
$$

Where the Contribution Pool is 25% of the current daily UBI allocation.

**Phase 2: Contribution-Weighted UBI (Month 2)**

$$
\text{Daily\_Reward} = \text{UBI\_Base} \times 0.50 \times \text{availability} + \frac{\text{Score}_i}{\sum \text{Scores}} \times \text{Contribution\_Pool}
$$

Where the Contribution Pool increases to 50% and availability requires maintaining 95%+ uptime.

**Phase 3: Pure Contribution Mode (Month 3+)**

$$
\text{Daily\_Reward} = \frac{\text{Score}_i}{\sum \text{Scores}} \times \text{Reward\_Pool} + \text{Availability\_Bonus}
$$

Where the Availability Bonus (10% of the daily pool) incentivizes standby capacity, weighted by hardware tier:

| Hardware Tier | Multiplier |
|---------------|-----------|
| RTX 3090 / A4000 | 1.0x |
| RTX 4090 / A5000 / A6000 | 1.5x |
| A100 | 2.5x |
| H100 | 4.0x |

### Payout Structure

| Component | Allocation |
|-----------|------------|
| Liquid SWAN | 80% |
| Locked SWAN (3-month vesting) | 20% |

### Unified Computing Provider (CP) Role

Under Swan 2.0, the legacy ECP and FCP roles merge into a single **Computing Provider (CP)** classification. All providers are evaluated equally based on contribution metrics, regardless of their previous role.

**Migration path for existing providers:**

1. ECPs and FCPs running inference tasks are automatically converted to unified CP role
2. Providers not running inference have a 30-day grace period to onboard to Swan Inference or migrate staked SWAN to SwanFi
3. Hardware requirements: ≥ 24 GB VRAM recommended; ≥ 48 GB VRAM for priority task routing

