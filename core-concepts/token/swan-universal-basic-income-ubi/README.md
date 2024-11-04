---
description: >-
  Universal Basic Income (UBI) and Paid Job Compensation for Computing Providers
  (CP) in Swan Chain
---

# UBI Allocation Curve

### **Table of Contents**

* [Introduction](https://docs.swanchain.io/core-concepts/token/swan-universal-basic-income-ubi#introduction)
* [Compensation Model](https://docs.swanchain.io/core-concepts/token/swan-universal-basic-income-ubi#compensation-model)
* [Algorithm Implementation](https://docs.swanchain.io/core-concepts/token/swan-universal-basic-income-ubi#algorithm-implementation)
* [Visualization of Income Over Time](https://docs.swanchain.io/core-concepts/token/swan-universal-basic-income-ubi#visualization-of-income-over-time)
  * [Interpretation of the Plots](https://docs.swanchain.io/core-concepts/token/swan-universal-basic-income-ubi#interpretation-of-the-plots)
  * [Data Points Illustration](https://docs.swanchain.io/core-concepts/token/swan-universal-basic-income-ubi#data-points-illustration)
  * [Impact of the Design](https://docs.swanchain.io/core-concepts/token/swan-universal-basic-income-ubi#impact-of-the-design)
  * [Conclusion](https://docs.swanchain.io/core-concepts/token/swan-universal-basic-income-ubi#conclusion)
  * [Future Work](https://docs.swanchain.io/core-concepts/token/swan-universal-basic-income-ubi#future-work)
* [Appendix](https://docs.swanchain.io/core-concepts/token/swan-universal-basic-income-ubi#appendix)
  * [Sample Calculations](https://docs.swanchain.io/core-concepts/token/swan-universal-basic-income-ubi#sample-calculations)

### **Introduction**

Swan Chain is a decentralized network that connects computing providers with users requiring computational resources. To foster early network growth and incentivize CPs to join and contribute resources, a dual compensation mechanism has been designed:

1. **Universal Basic Income (UBI)**: Provides CPs with a predictable token income when their resources are underutilized.
2. **Paid Jobs**: Offers market-priced compensation for computational tasks requested by users.

This mechanism ensures a fair and gradual distribution of tokens to providers, supporting the network's expansion until it reaches a critical mass of user-paid tasks. Importantly, the UBI distribution rate is influenced by the resource usage rate, and CPs earn market-based compensation when engaged in paid jobs.

***

## **Compensation Model**

$$
I(x) = A \cdot x^{B} \cdot e^{-C x} \cdot (1 - u(x)) + P_{\text{market}}(x) \cdot u(x)
$$

The total daily income $$I(x)$$for a computing provider on day $$( x )$$ comprises two components:

1. **UBI Income** $$y_{\text{UBI}}(x)$$
2. **Paid Job Income** $$y_{\text{Paid}}(x)$$

The **UBI income** is modeled using a gamma-like function adjusted by the resource usage rate $$u(x)$$:

$$
y_{\text{UBI}}(x) = A \cdot x^{B} \cdot e^{-C x} \cdot (1 - u(x))
$$

Where:

* $$A = 20,000$$ (Scaling factor)
* $$B = 0.3100$$ (Growth rate exponent)
* $$C = 0.0017$$ (Decay rate constant)
* $$x$$ is the day number, starting from 1
* $$u(x)$$ is the **resource usage rate** on day$$( x )$$ (ranging from 0 to 1)

The **paid job income** depends on the market demand and the resource utilization:

$$
y_{\text{Paid}}(x) = P_{\text{market}}(x) \cdot u(x)
$$

Where:

* $$P_{\text{market}}(x)$$ is the total market value for computational resources on day $$( x )$$
* $$u(x)$$ represents the proportion of a CP's resources utilized by paid job

#### Defining $$u(x)$$

**(1) Calculate the total duration of real GPU orders across the network**

$$
T_{day}= \sum\limits_{i}Task_{ECP,i}(GPU_k)  \times  f_k + \sum\limits_{j}Task_{FCP,j}(GPU_k)  \times  f_k  \times W_{FCP}
$$

Where:

* $$Task_{\text{FCP},i}(GPU_k)$$ represents the time that the $$i$$-th FCP task uses $$GPU_k$$.
* $$Task_{\text{ECP},j}(GPU_k)$$ represents the time that the $$j$$-th ECP task uses $$GPU_k$$.
* &#x20;$$f_k$$ represents the earnings growth factor
* $$W_{FCP}$$ represents the FCP resource bonus ratio, currently set at a constant value of 1.2

{% hint style="info" %}
**NOTE:** The value of $$W_{FCP}$$, 1.2, means that if the same configuration of servers is deployed for FCP, it will generate 20% more earnings than ECP.
{% endhint %}

**（2）Calculate the total available usage time for all GPUs in the network**

$$
T_{total} =  \sum\limits_k N_{ECP}(GPU_k) \times 24   \times f_k +  \sum\limits_k N_{FCP}(GPU_k) \times 24   \times f_k * W_{FCP}
$$

Where:

* $$N_{\text{FCP}}(\text{GPU}_k)$$represents the number of $$\text{GPU}_k$$ _in FCP_
* &#x20;$$N_{\text{ECP}}(\text{GPU}_k)$$ represents the number of $$\text{GPU}_k$$ in ECP.&#x20;

**(3) Calculate** $$u(x)$$

$$
u(x) = \frac{T_{\text{day}}}{T_{\text{total}}}
$$

#### Defining Total Market Value $$P_{\text{market}}(x)$$

$$P_{\text{market}}(x)$$ represents the cost in Swan Tokens when all GPUs in the CP are fully utilized:

$$
P_{market}{(x)} =  \sum\limits_k N_{ECP}(GPU_k)   \times Price({GPU_k})  \times 24 +  \sum\limits_k N_{FCP}(GPU_k)  \times W_{FCP} \times Price({GPU_k})  \times 24
$$

Where:

* $$Price(GPU_k)$$ is the price of $$GPU_k$$.
* $$W_{FCP}$$ represents the FCP resource bonus ratio, currently set at a constant value of 1.2
* $$N_{\text{FCP}}(\text{GPU}_k)$$represents the number of $$\text{GPU}_k$$ _in FCP_
* &#x20;$$N_{\text{ECP}}(\text{GPU}_k)$$ represents the number of $$\text{GPU}_k$$ in ECP.&#x20;

## **Algorithm Implementation**

The compensation mechanism proceeds as follows:

1. **Initialization**: Set day $$(x = 1)$$.
2. **Determine Resource Usage Rate**: Calculate $$u(x)$$ based on the CP's resource utilization by paid jobs.
3. **Compute UBI Income**:

$$
y_{\text{UBI}}(x) = 20,000 \times x^{0.3100} \times e^{-0.0017 x} \times (1 - u(x))
$$

4. **Compute Paid Job Income**:

$$
y_{\text{Paid}}(x) = P_{\text{market}}(x) \times u(x)
$$

5. **Calculate Total Income**:

$$
I(x) = y_{\text{UBI}}(x) + y_{\text{Paid}}(x)
$$

6. **Distribute Income**: Allocate $$I(x)$$ to CPs based on their resource contributions and utilization.
7. **Increment Day**: Increase $$x$$ by 1.
8. **Repeat**: Continue the process for each subsequent day.

This algorithm ensures that CPs are incentivized to contribute resources to the network, receiving UBI when their resources are underutilized and earning market-based compensation when engaged in paid jobs.

***

## **Visualization of Income Over Time**

#### **Scenarios**

We consider three scenarios to illustrate how CPs' income evolves over time:

1. **No Paid Jobs** $$u(x) = 0$$ : CPs receive income solely from UBI.
2. **Low Paid Job Demand** $$u(x) = 0.1$$: CPs primarily earn UBI income with a small contribution from paid jobs.
3. **Increasing Paid Job Demand**: Resource usage rate $$u(x)$$ increases over time, shifting CPs' income from UBI to paid jobs.

<figure><img src="../../../.gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

### **Interpretation of the Plots**

**1. Total Income Over 720 Days**

* **Scenario 1:** $$u(x) = 0$$
  * CPs receive income solely from UBI.
  * The total income decreases gradually over time due to the decay in the UBI function.
* **Scenario 2:** $$u(x) = 0.1$$
  * CPs receive slightly less UBI income than in Scenario 1 due to the 10% resource usage.
  * Paid job income contributes minimally, resulting in a slightly lower total income.
* **Scenario 3:** $$Increasing u(x)$$
  * Initially, total income is similar to Scenario 1.
  * As $$u(x)$$ increases, paid job income increases while UBI income decreases.
  * Total income remains relatively stable or increases slightly, demonstrating that paid job income offsets the reduction in UBI.

**2. Income Components for Scenario 3**

* **UBI Income**:
  * Decreases over time as resource usage rate $$u(x)$$ increases.
  * Reflects the transition from reliance on UBI to paid jobs.
* **Paid Job Income**:
  * Increases over time with the increase in $$u(x)$$ .
  * Compensates for the decrease in UBI income.
* **Total Income Stability**:
  * The sum of UBI and paid job income maintains income stability for CPs.

**3. Resource Usage Rate** $$u(x)$$ **for Scenario 3**

* Shows a smooth increase from 0 to 0.8 over 720 days.
* Reflects the gradual adoption of paid tasks in the network.

### **Data Points Illustration**

Below is a table of computed token allocations for selected days:

| Day | Daily UBI (y) | Cumulative UBI |
| --- | ------------- | -------------- |
| 1   | 19,966.03     | 19,966.03      |
| 30  | 54,549.22     | 1,261,976.56   |
| 60  | 64,262.68     | 3,062,143.25   |
| 90  | 69,246.55     | 5,072,341.49   |
| 120 | 71,941.60     | 7,194,431.61   |
| 150 | 73,261.06     | 9,375,212.61   |
| 180 | 73,666.56     | 11,581,013.65  |
| 210 | 73,430.22     | 13,788,817.87  |
| 240 | 72,728.28     | 15,982,188.47  |
| 270 | 71,682.24     | 18,149,084.82  |
| 300 | 70,379.70     | 20,280,565.34  |
| 330 | 68,885.86     | 22,369,958.88  |
| 360 | 67,250.50     | 24,412,305.58  |
| 390 | 65,512.29     | 26,403,963.32  |
| 420 | 63,701.70     | 28,342,321.28  |
| 450 | 61,843.01     | 30,225,585.83  |
| 480 | 59,955.70     | 32,052,616.78  |
| 510 | 58,055.51     | 33,822,799.99  |
| 540 | 56,155.17     | 35,535,946.61  |
| 570 | 54,265.01     | 37,192,212.48  |
| 600 | 52,393.39     | 38,792,032.93  |
| 630 | 50,547.09     | 40,336,069.55  |
| 660 | 48,731.55     | 41,825,166.37  |
| 690 | 46,951.10     | 43,260,313.71  |
| 720 | 45,209.18     | 44,642,617.97  |

Note: The "Cumulative UBI" column represents the definite integral of y(x) from day 1 to the specified day.

{% hint style="warning" %}
This table shows simulated data for UBI distribution calculated under the condition of U(x) = 0. The actual UBI release will dynamically change based on the network CP resource utilization rate.
{% endhint %}

***

### **Impact of the Design**

#### **Incentivizing Optimal Resource Utilization**

* **Adaptive Compensation**: CPs are motivated to engage in paid jobs as they become available, earning higher income through market rates.
* **Resource Availability**: UBI ensures that CPs keep their resources available to the network, even during periods of low demand.

#### **Sustainable Long-Term Distribution**

* **Transition to Market-Based Economy**: As the network matures and paid job demand increases, CPs naturally shift from UBI reliance to market compensation.
* **Controlled Token Issuance**: The decreasing UBI allocation over time prevents token oversupply, maintaining economic stability.

#### **Economic Implications**

* **Income Stability**: CPs benefit from a combination of UBI and paid job income, smoothing income fluctuations.
* **Market Alignment**: Compensation reflects real-time network demand, promoting efficient resource allocation.

***

### **Conclusion**

The combined UBI and paid job compensation model for Swan Chain computing providers effectively balances incentives, supporting early network growth while promoting efficient resource utilization. By dynamically adjusting CPs' income based on resource usage rates and market demand, the model ensures sustainable network development and economic stability as the network transitions to a mature, user-driven ecosystem.

***

### **Future Work**

* **Dynamic Market Pricing**: Implement real-time market pricing mechanisms for paid jobs to reflect supply and demand accurately.
* **Adaptive UBI Parameters**: Explore methods to adjust UBI parameters ( A ), ( B ), and ( C ) based on network growth metrics.
* **Enhanced Monitoring Tools**: Develop systems to track resource usage and job completion accurately, ensuring fair compensation.

***

## **Appendix**

### Sample Calculations

**Scenario 1: No Paid Jobs**

**Day 1:**

* **UBI Income:**\
  $$y_{\text{UBI}}(1) = 20000 \times 1^{0.31} \times e^{-0.0017 \times 1} \approx 19,965.90 \, \text{tokens}$$
* **Paid Job Income:**\
  $$0 \, \text{tokens}$$ (No paid job demand)
* **Total Income:**\
  $$I(1) = 19,965.90 + 0 = 19,965.90 \, \text{tokens}$$

**Scenario 2: Low Paid Job Demand**

**Day 1:**

* **UBI Income:**\
  $$y_{\text{UBI}}(1) = 20000 \times 1^{0.31} \times e^{-0.0017 \times 1} \approx 19,965.90 \, \text{tokens}$$
* **Paid Job Income:**\
  $$y_{\text{Paid}}(1) = 50000 \times 0.1 = 5,000 \, \text{tokens}$$
* **Total Income:**\
  $$I(1) = 19,965.90 + 5,000 = 24,965.90 \, \text{tokens}$$

**Scenario 3: Increasing Paid Job Demand**

**Day 360:**

* **UBI Income:**\
  $$y_{\text{UBI}}(360) = 20000 \times 360^{0.31} \times e^{-0.0017 \times 360} \approx 13,898.99 \, \text{tokens}$$
* **Paid Job Income:**\
  $$y_{\text{Paid}}(360) = 50000 \times 0.4 = 20,000 \, \text{tokens}$$
* **Total Income:**\
  $$I(360) = 13,898.99 + 20,000 = 33,898.99 \, \text{tokens}$$

**Day 720:**

* **UBI Income:**\
  $$y_{\text{UBI}}(720) = 20000 \times 720^{0.31} \times e^{-0.0017 \times 720} \times (1 - 0.8) \approx 2,477.66 \, \text{tokens}$$
* **Paid Job Income:**\
  $$y_{\text{Paid}}(720) = 50000 \times 0.8 = 40,000 \, \text{tokens}$$
* **Total Income:**\
  $$I(720) = 2,477.66 + 40,000 = 42,477.66 \, \text{tokens}$$

#### Observations

* **Income Stability:** Despite the decrease in UBI income over time, total income remains stable or increases due to higher compensation from paid jobs.
* **Incentive Alignment:** Community participants (CPs) are incentivized to participate in paid jobs without experiencing significant income loss during transitions from UBI reliance to paid employment.

***
