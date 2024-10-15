# Computing Provider Income

Swan Chain is a decentralized network that connects computing providers with users requiring computational resources. To foster early network growth and incentivize CPs to join and contribute resources, a dual compensation mechanism has been designed:

1. **Universal Basic Income (UBI)**: Provides CPs with a predictable token income when their resources are underutilized.
2. **Paid Jobs**: Offers market-priced compensation for computational tasks requested by users.

This mechanism ensures a fair and gradual distribution of tokens to providers, supporting the network's expansion until it reaches a critical mass of user-paid tasks. Importantly, the UBI distribution rate is influenced by the resource usage rate, and CPs earn market-based compensation when engaged in paid jobs.

### **Total Income**

The total daily income for a CP is:

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

### Individual CP's Income

To calculate the UBI for a single CP, we consider both the resource usage and completion rates of tasks. UBI allocation is conditional on sufficient resource contribution and performance metrics:

**(1) UBI Workload Calculation**

* Calculate the daily completion rate of a single ECP zk-task: $$P_{\text{ECP}}$$
* Calculate the completion rate of a single FCP sampling task: $$P_{\text{FCP}}$$
* Number of GPUs $$N_{\text{CP}}(GPU_k)$$ and GPU types.
* Calculate the total UBI workload:

$$
UBI_{\text{total}} = UBI_{\text{ECP}} + UBI_{\text{FCP}}
$$

$$
UBI_{\text{ECP}} = \sum\limits_i \left( \sum\limits_k N_{\text{ECP},i}(GPU_k) \times f_k \right) \times P_{\text{ECP},i}
$$

$$
UBI_{\text{FCP}} = \sum\limits_j \left( \sum\limits_k N_{\text{FCP},j}(GPU_k) \times f_k \right) \times P_{\text{FCP},j}
$$

**(2) Calculating the UBI for a single CP**:

As an ECP:

$$
UBI_{\text{ECP},i}(x) = \frac{\sum\limits_k N_{\text{ECP},i}(GPU_k) \times f_k \times P_{\text{ECP},i}}{UBI_{\text{ECP}} + UBI_{\text{FCP}}} \times I(x)
$$

As an FCP:

$$
UBI_{\text{FCP},i}(x) = \frac{\sum\limits_k N_{\text{FCP},i}(GPU_k) \times f_k \times P_{\text{FCP},i}}{UBI_{\text{ECP}} + UBI_{\text{FCP}}} \times I(x)
$$



***

### Conditions for CP to Receive UBI

A CP must meet certain conditions to qualify for UBI:

1. **Sufficient Collateral**:

$$
Collateral_{\text{CP}} = \sum\limits_k N_{\text{CP}}(GPU_k) \times C_{\text{base}} \times f_k
$$

Where:

* $$N_{\text{CP}}(GPU_k)$$ is the number of GPUs held by CP.
* $$C_{\text{base}}$$ is the base collateral.

2. **Completion of Basic Test Tasks:**

* FCP: Sampling task
* ECP: ZK task

3. GPU count and type are also factored into the UBI eligibility.

