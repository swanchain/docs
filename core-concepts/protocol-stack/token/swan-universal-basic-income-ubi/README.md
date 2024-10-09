---
description: >-
  Universal Basic Income (UBI) and Paid Job Compensation for Computing Providers
  (CP) in Swan Chain
---

# UBIand Paid Job Compensation for CP

### **Abstract**

This document presents the design and implementation of a compensation mechanism for computing providers (CPs) in the Swan Chain network, combining Universal Basic Income (UBI) token allocation and market-priced paid jobs. The allocation model incentivizes network growth over a span of ten years and beyond, particularly during the initial phases before widespread user adoption of paid tasks. We detail the mathematical formula, the underlying algorithm—including how resource usage rates determine UBI distribution—and discuss the impact of this design on the network's sustainability and scalability. Notably, CPs receive income from both UBI and paid jobs, with UBI decreasing as paid job engagement increases, reflecting an adaptive compensation system aligned with network activity.

***

### **Introduction**

Swan Chain is a decentralized network that connects computing providers with users requiring computational resources. To foster early network growth and incentivize CPs to join and contribute resources, a dual compensation mechanism has been designed:

1. **Universal Basic Income (UBI)**: Provides CPs with a predictable token income when their resources are underutilized.
2. **Paid Jobs**: Offers market-priced compensation for computational tasks requested by users.

This mechanism ensures a fair and gradual distribution of tokens to providers, supporting the network's expansion until it reaches a critical mass of user-paid tasks. Importantly, the UBI distribution rate is influenced by the resource usage rate, and CPs earn market-based compensation when engaged in paid jobs.

***

### **Background**

In decentralized networks, bootstrapping initial participation is crucial. Computing providers may be hesitant to join a network without immediate incentives, especially when user demand is low. The combined UBI and paid job compensation system serves as a balanced reward mechanism:

* **UBI**: Encourages providers to contribute resources by offering a baseline income during periods of low demand.
* **Paid Jobs**: Motivates CPs to engage in user-requested tasks, earning income based on market rates.

This strategy aims to balance supply and demand, stabilizing the network as it grows and transitions towards a mature, user-driven ecosystem.

***

## **Compensation Model**

#### **Mathematical Model**

The total daily income $$I(x)$$for a computing provider on day $$( x )$$ comprises two components:

1. **UBI Income** $$y_{\text{UBI}}(x)$$
2. **Paid Job Income** $$( y_{\text{Paid}}(x) )$$

The UBI income is modeled using a gamma-like function adjusted by the resource usage rate $$u(x)$$:

$$
y_{\text{UBI}}(x) = A \cdot x^{B} \cdot e^{-C x} \cdot (1 - u(x))
$$

Where:

* $$A = 42,000$$ (Scaling factor)
* $$B = 0.1860$$ (Growth rate exponent)
* $$C = 0.0017$$ (Decay rate constant)
* $$x$$ is the day number, starting from 1
* $$u(x)$$ is the **resource usage rate** on day$$( x )$$ (ranging from 0 to 1)

The paid job income depends on the market demand and the resource utilization:

$$
y_{\text{Paid}}(x) = P_{\text{market}}(x) \cdot u(x)
$$

Where:

* $$P_{\text{market}}(x)$$ is the market price for computational resources on day $$( x )$$
* $$u(x)$$ represents the proportion of a CP's resources utilized by paid jobs

#### **Total Income**

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

***

## **Algorithm Implementation**

The compensation mechanism proceeds as follows:

1. **Initialization**: Set day $$(x = 1)$$.
2. **Determine Resource Usage Rate**: Calculate $$u(x)$$ based on the CP's resource utilization by paid jobs.
3. **Compute UBI Income**:

$$
y_{\text{UBI}}(x) = 42,000 \times x^{0.1860} \times e^{-0.0017 x} \times (1 - u(x))
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

<figure><img src="../../../../.gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>



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

### **Appendix**

#### **Sample Calculations**

**Scenario 1: No Paid Jobs** $$u(x) = 0$$

**Day 1**:

* UBI Income:

$$
y_{\text{UBI}}(1) = 42,000 \times 1^{0.1860} \times e^{-0.0017 \times 1} \times (1 - 0) \approx 41,928.93 \text{ tokens}
$$

* Paid Job Income:

$$
y_{\text{Paid}}(1) = 50,000 \times 0 = 0 \text{ tokens}
$$

* Total Income:

$$
I(1) = 41,928.93 + 0 = 41,928.93 \text{ tokens}
$$

**Scenario 2: Low Paid Job Demand** $$u(x) = 0.1$$

**Day 1**:

* UBI Income:

$$
y_{\text{UBI}}(1) = 42,000 \times 1^{0.1860} \times e^{-0.0017 \times 1} \times (1 - 0.1) \approx 37,736.04 \text{ tokens}
$$

* Paid Job Income:

$$
y_{\text{Paid}}(1) = 50,000 \times 0.1 = 5,000 \text{ tokens}
$$

* Total Income:

$$
I(1) = 37,736.04 + 5,000 = 42,736.04 \text{ tokens}
$$

**Scenario 3: Increasing Paid Job Demand**

**Day 360:** $$u(360) = 0.4$$

* UBI Income:

$$
y_{\text{UBI}}(360) = 42,000 \times 360^{0.1860} \times e^{-0.0017 \times 360} \times (1 - 0.4) \approx 13,898.99 \text{ tokens}
$$

* Paid Job Income:

$$
y_{\text{Paid}}(360) = 50,000 \times 0.4 = 20,000 \text{ tokens}
$$

* Total Income:

$$
I(360) = 13,898.99 + 20,000 = 33,898.99 \text{ tokens}
$$

**Day 720** $$u(720) = 0.8$$

* UBI Income:

$$
y_{\text{UBI}}(720) = 42,000 \times 720^{0.1860} \times e^{-0.0017 \times 720} \times (1 - 0.8) \approx 2,477.66 \text{ tokens}
$$

* Paid Job Income:

$$
y_{\text{Paid}}(720) = 50,000 \times 0.8 = 40,000 \text{ tokens}
$$

* Total Income:

$$
I(720) = 2,477.66 + 40,000 = 42,477.66 \text{ tokens}
$$

#### **Observations**

* **Income Stability**: Despite the decrease in UBI income, total income remains stable or increases due to higher paid job compensation.
* **Incentive Alignment**: CPs are incentivized to participate in paid jobs without experiencing significant income loss during transitions.

***

### **References**

* **Gamma Function and Applications**: Details on the mathematical properties of the gamma function used in the allocation formula.
* **Decentralized Network Incentive Models**: Studies on incentive mechanisms in blockchain and decentralized networks.
* **Token Economics**: Literature on designing token economies for sustainability and growth.

***

### **Disclaimer**

This document is intended for informational purposes to outline the UBI and paid job compensation mechanism within the Swan Chain network. The parameters and models presented are subject to change based on ongoing network analysis and market conditions.
