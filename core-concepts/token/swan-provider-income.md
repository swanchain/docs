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
UBI_{ECP}=\sum\limits_i (\sum\limits_k N_{ECP,i}(GPU_k) \times f_k) \times P_{ECP,i}
$$

$$
UBI_{FCP}=\sum\limits_j (\sum\limits_k N_{FCP,j}(GPU_k) \times f_k) \times P_{FCP,j} *W_{FCP}
$$

**(2) Calculating the UBI for a single CP**:

As an ECP:

$$
UBI_{\text{ECP},i}(x) = \frac{\sum\limits_k N_{\text{ECP},i}(GPU_k) \times f_k \times P_{\text{ECP},i}}{UBI_{\text{ECP}} + UBI_{\text{FCP}}} \times y_{\text{UBI}}(x)
$$

As an FCP:

$$
UBI_{FCP,i}(x)= \frac{\sum\limits_k N_{FCP,i}(GPU_k) \times f_k) \times P_{FCP,i} \times W_{FCP} }{UBI_{ECP}+UBI_{FCP}} \times y_{\text{UBI}}(x)
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
* $$C_{\text{base}}$$ is the base collateral, with an initial value of 3500 (this value will be dynamically adjusted based on the daily computing units of the entire network; for specific adjustment rules, check [here](computing-provider-collateral/collateral-requirement-and-earning-multiplier.md))
* $$N_{\text{FCP}}(\text{GPU}_k)$$represents the number of $$\text{GPU}_k$$ _in FCP_
* &#x20;$$N_{\text{ECP}}(\text{GPU}_k)$$ represents the number of $$\text{GPU}_k$$ in ECP.&#x20;

2. **Completion of Basic Test Tasks:**

* FCP: Sampling task
* ECP: ZK task

3. GPU count and type are also factored into the UBI eligibility.

