---
description: >-
  Universal Basic Income (UBI) Token Allocation for Computing Providers in Swan
  Chain
---

# Universal Basic Income (UBI)

## **Abstract**

This document presents the design and implementation of a Universal Basic Income (UBI) token allocation mechanism for computing providers in the Swan Chain network. The allocation model is intended to incentivize network growth over a span of ten years and beyond, particularly during the initial phases before widespread user adoption of paid tasks. We detail the mathematical formula, the underlying algorithm, include specific parameter values, provide a visual representation of the allocation curve, and discuss the impact of this design on the network's sustainability and scalability.

***

### **Introduction**

Swan Chain is a decentralized network that connects computing providers with users requiring computational resources. To foster early network growth and incentivize computing providers to join and contribute resources, a UBI token allocation mechanism has been designed. This mechanism ensures a fair and gradual distribution of tokens to providers, supporting the network's expansion until it reaches a critical mass of user-paid tasks.

***

### **Background**

In decentralized networks, bootstrapping initial participation is crucial. Computing providers are less likely to join a network without immediate incentives, especially when user demand is low. The UBI token allocation serves as an initial reward system, encouraging providers to contribute resources by offering them a predictable token income. This strategy aims to balance supply and demand, stabilizing the network as it grows.

***

### **Token Allocation Formula and Algorithm**

#### **Mathematical Model**

The token allocation to computing providers is modeled using a gamma-like function:

$$
y(x) = A \cdot x^{B} \cdot e^{-C x}
$$

Where:

* $$y(x)$$ is the number of tokens allocated on day ( $$x$$ ).
* $$A$$ is the scaling factor determining the initial token allocation.
* $$B$$ is the growth rate exponent.
* $$C$$ is the decay rate constant.
* $$x$$ represents the day number, starting from 1.

#### **Specific Parameters**

For the Swan Chain UBI mechanism, the following parameters have been selected based on network modeling and forecasting:

* **Scaling Factor (**$$A$$**):42,000**
* **Growth Rate Exponent (**$$B$$**): 0.1860**
* **Decay Rate Constant (**$$C$$**): 0.0017**

These parameters shape the token distribution curve over time, ensuring an initial increase in token allocation to incentivize early participation, followed by a gradual decrease to maintain long-term sustainability.

#### **Algorithm Implementation**

The algorithm for token allocation proceeds as follows:

1. **Initialization**: Set day $$x = 1$$.
2. **Compute Daily Allocation**: $$y(x) = 42,000 \times x^{0.1860} \times e^{-0.0017 x}$$
3. **Distribute Tokens**: Allocate ( $$y(x)$$) tokens to computing providers proportionally based on their contributed resources.
4. **Increment Day**: Increase $$x$$ by 1.
5. **Repeat**: Continue the process for each subsequent day.

This algorithm ensures a smooth token distribution that initially increases to incentivize early participation and gradually decreases to maintain long-term sustainability.

***

### **Visualization of the Allocation Curve**

#### **Plot Description**

The allocation curve represents the number of tokens allocated to computing providers each day over an extended period. The curve is generated using the formula:

$$y(x) = 42,000 \times x^{0.1860} \times e^{-0.0017 x}$$

<figure><img src="../../../../.gitbook/assets/image (190).png" alt=""><figcaption></figcaption></figure>

**Key Features of the Curve:**

* **Initial Increase**: The curve rises from day 1, reflecting the increasing token allocation to encourage early participation.
* **Peak Allocation**: The maximum token allocation occurs around a specific day, after which the allocation begins to decrease.
* **Gradual Decay**: Following the peak, the curve demonstrates a gradual decline in daily token allocation, aligning with the anticipated growth in user-paid tasks.

#### **Data Points Illustration**

Below is a table of computed token allocations for selected days:

| **Day (**$$x$$**)** | **Daily Token Allocation (**$$y(x)$$**)** |
| ------------------- | ----------------------------------------- |
| 1                   | 41,928                                    |
| 60                  | 81,224                                    |
| 120                 | 83,441                                    |
| 180                 | 81,252                                    |
| 240                 | 77,406                                    |
| 300                 | 72,862                                    |
| 360                 | 68,066                                    |
| 420                 | 63,253                                    |
| 480                 | 58,556                                    |
| 540                 | 54,049                                    |
| 600                 | 49,774                                    |
| 660                 | 45,751                                    |
| 720                 | 41,989                                    |
| 780                 | 38,486                                    |
| 840                 | 35,236                                    |
| 900                 | 32,230                                    |
| 960                 | 29,456                                    |
| 1,020               | 26,901                                    |
| 1,080               | 24,552                                    |
| 1,140               | 22,396                                    |

#### **Interpretation**

* **Early Days**: High token allocations incentivize providers to join the network early.
* **Mid-Term**: Token allocations gradually decrease but remain substantial, supporting continued participation.
* **Long-Term**: Allocations continue to decrease, encouraging reliance on user-paid tasks as the primary incentive.

***

### **Impact of the Design**

#### **Incentivizing Early Participation**

* **Growth Phase**: The initial increase in token allocation encourages computing providers to join early, fostering rapid network growth.
* **Peak Allocation**: The model reaches a peak allocation before user-paid tasks become prevalent, ensuring providers are adequately rewarded during the network's formative stages.

#### **Sustainable Long-Term Distribution**

* **Gradual Decay**: The exponential decay component ensures that token allocation decreases over time, aligning with the anticipated increase in user-paid tasks.
* **Preventing Oversaturation**: By reducing token allocation over time, the model prevents token oversupply, maintaining the token's value and economic balance within the network.

#### **Mathematical Justification**

*   **Convergence of Total Allocation**: The integral of  $$y(x)$$from day 1 to infinity converges to a finite value, ensuring that the total token supply remains controlled.

    $$\int_{1}^{\infty} y(x) , dx \approx 74,317,411 \text{ tokens}$$
* **Flexibility in Parameters**: Adjusting $$A$$, $$B$$, and $$C$$allows the network to fine-tune the allocation model in response to network growth rates and market conditions.

#### **Economic Implications**

* **Token Value Stability**: Controlled token issuance helps maintain token scarcity, supporting its value in the marketplace.
* **Encouraging Resource Contribution**: Providers receive predictable rewards, encouraging sustained participation and resource availability.

***

### **Conclusion**

The UBI token allocation model for Swan Chain computing providers is a strategic approach to stimulate early network growth and ensure long-term sustainability. By employing a gamma-like function with specific parameters, the model provides a fair and controlled distribution of tokens that adapts over time. This design balances the immediate need to incentivize providers with the long-term goal of a self-sustaining network driven by user-paid tasks.

***

### **Future Work**

* **Monitoring and Adjustment**: Continual monitoring of network performance and token economics will inform potential adjustments to the allocation parameters.
* **User Adoption Strategies**: Complementing the UBI model with initiatives to increase user demand for computing tasks will further strengthen the network.
* **Integration with Market Dynamics**: Exploring mechanisms to integrate token allocation with market signals can enhance responsiveness to changing conditions.

***

### **Appendix**

#### **Sample Calculations**

*   **Day 1 Allocation**:

    $$y(1) = 42,000 \times 1^{0.1860} \times e^{-0.0017 \times 1} \approx 41,928 \text{ tokens}$$
*   **Day 365 Allocation**:

    $$y(365) = 42,000 \times 365^{0.1860} \times e^{-0.0017 \times 365} \approx 67,663 \text{ tokens}$$
*   **Day 730 Allocation**:

    $$y(730) = 42,000 \times 730^{0.1860} \times e^{-0.0017 \times 730} \approx 41,387 \text{ tokens}$$

#### **Total Allocation Over Time**

The total token allocation over an extended period can be calculated using the definite integral:

$$\text{Total Allocation} = \int_{1}^{\infty} y(x) , dx \approx 74,317,411 \text{ tokens}$$

This ensures the total token supply remains within planned limits, preventing inflation and maintaining economic stability.

***

### **References**

* **Gamma Function and Applications**: Details on the mathematical properties of the gamma function used in the allocation formula.
* **Decentralized Network Incentive Models**: Studies on incentive mechanisms in blockchain and decentralized networks.
* **Token Economics**: Literature on designing token economies for sustainability and growth.

***

### **Disclaimer**

This document is intended for informational purposes to outline the UBI token allocation mechanism within the Swan Chain network. The parameters and models presented are subject to change based on ongoing network analysis and market conditions.

***

## **Appendix B: Mathematical Derivations**

### **Definite Integral to Infinity**

To ensure the total token allocation remains finite, we compute the definite integral of $$y(x)$$from day 1 to infinity:

$$
\int_{1}^{\infty} y(x) , dx = \int_{1}^{\infty} 42,000 \cdot x^{0.1860} \cdot e^{-0.0017 x} , dx
$$

Using the properties of the gamma function, we find:

$$\int_{1}^{\infty} y(x) , dx = \frac{A}{C^{B + 1}} \cdot \Gamma(B + 1, C)$$

Where:

* $$\Gamma(B + 1, C)$$is the upper incomplete gamma function.

Computing this integral numerically yields approximately 74,317,411 tokens, confirming the convergence of the total allocation.

***

