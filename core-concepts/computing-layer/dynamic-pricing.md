# Dynamic Pricing

## 1. Introduction

In the fast-paced world of cloud computing and AI, the demand for GPU resources fluctuates rapidly. Our new dynamic pricing strategy aims to optimize resource allocation by implementing a quadratic demand model. This approach ensures that prices respond more aggressively to high demand, promoting efficient resource utilization and improved accessibility for all users.

### Abstract Workflow

![dynamic\_pricing\_overview.png](../protocol-stack/computing-layer/imgs/dynamic\_pricing\_overview.png)

## 2. Why a Quadratic Demand Model?

### Advantages of Quadratic Pricing:

* More responsive to high-demand situations
* Encourages efficient resource usage during peak times
* Provides better price signals to users about resource scarcity
* Allows for more nuanced pricing across different demand levels

## 3. Quadratic Dynamic Pricing Model

Our new model uses a quadratic equation to calculate the demand factor, which is then used to adjust the base price of resources in real-time.

### Core Formula:

```
New Price = Demand Factor * Base Price
```

Where:

* Base Price: The standard price set for the resource
* Demand Factor: A multiplier based on current demand (range: 1.0 to 5.0)

### Demand Factor Calculation:

The demand factor is now calculated using a quadratic function:

```
Demand Factor = 1 + 4 * (w1 * H + w2 * C)^2
```

Where:

* H: Historical usage factor (0 to 1)
* C: Current resource availability factor (0 to 1)
* w1 = 0.35 (weight for historical usage)
* w2 = 0.65 (weight for current availability)

This quadratic formula ensures that the demand factor grows more rapidly as demand increases, with a maximum value of 5.0.

## 4. Base Price Determination

Before applying our dynamic pricing model, we first establish a base price for each type of computing resource. This base price is derived from the prices set by our various Computing Providers (CPs).

### 4.1 Computing Provider Pricing

Each CP sets their own prices for different hardware resources, including:

* CPU (price per core per hour)
* GPU (price per GPU per hour)
* Memory (price per GB per hour)
* Storage (price per GB per hour)

### 4.2 Weighted Arithmetic Mean Calculation

For each type of resource, we calculate a Weighted Arithmetic Mean (WAM) across the available resource from all CPs. This WAM becomes our base price for that resource.

The formula for WAM is:

```
WAM = (Σ (Price_i * Weight_i)) / (Σ Weight_i)
```

Where:

* Price\_i is a unique price point set by one or more CPs
* Weight\_i is the number of CPs that have set their price to Price\_i

This approach ensures that our base price is more heavily influenced by the most common price points among our CPs.

#### Let's look at how different CP prices affect the base price for memory:

```
CP1: $0.010 per GB per hour
CP2: $0.010 per GB per hour
CP3: $0.012 per GB per hour
CP4: $0.010 per GB per hour
CP5: $0.015 per GB per hour
CP6: $0.012 per GB per hour
```

Grouping by unique prices:

```
$0.010: Weight = 3 (set by CP1, CP2, CP4)
$0.012: Weight = 2 (set by CP3, CP6)
$0.015: Weight = 1 (set by CP5)
```

WAM Calculation:

```
WAM = (0.010*3 + 0.012*2 + 0.015*1) / (3 + 2 + 1)
    = 0.069 / 6
    = $0.0115 per GB per hour
```

This WAM of $0.0115 would then be used as the base price for memory in our dynamic pricing calculations.

### 4.3 Composite Base Price

For each hardware configuration we offer, we calculate a composite base price by summing the WAM prices of its components:

```
Base Price = (CPU_WAM * #cores) + (GPU_WAM * #GPUs) + (Memory_WAM * #GB) + (Storage_WAM * #GB)
```

## 5. Components of the Demand Factor

### 5.1 Historical Resource Usage (H)

We analyze the past 30 days of usage data, focusing on patterns in daily and weekly usage.

Calculation:

```
H = (Current Hour Avg Usage - Overall Avg Usage) / (Max Usage - Overall Avg Usage)
```

### 5.2 Current Resource Availability (C)

This factor considers the current occupation rate of resources, but only starts contributing when more than 40% of resources are occupied.

Calculation:

```
If (Occupied Resources / Total Resources) <= 0.4:
    C = 0
Else:
    C = ((Occupied Resources / Total Resources) - 0.4) / 0.6
```

## 6. Price Model

![dynamic\_pricing\_chart.png](../protocol-stack/computing-layer/imgs/dynamic\_pricing\_chart.png)

## 7. Example Scenarios

### Scenario 1: Low Demand

* Historical Usage: Low (H = 0.2)
* Current Availability: 45% resources occupied (C = 0.083)

```
Weighted Sum = 0.35 * 0.2 + 0.65 * 0.083 = 0.124
Demand Factor = 1 + 4 * (0.124)^2 = 1.061
For a base price of $10/hour: New Price = 1.061 * $10 = $10.61/hour
```

### Scenario 2: Moderate Demand

* Historical Usage: Average (H = 0.5)
* Current Availability: 70% resources occupied (C = 0.5)

```
Weighted Sum = 0.35 * 0.5 + 0.65 * 0.5 = 0.5
Demand Factor = 1 + 4 * (0.5)^2 = 2.0
For a base price of $10/hour: New Price = 2.0 * $10 = $20/hour
```

### Scenario 3: High Demand

* Historical Usage: High (H = 0.8)
* Current Availability: 90% resources occupied (C = 0.833)

```
Weighted Sum = 0.35 * 0.8 + 0.65 * 0.833 = 0.821
Demand Factor = 1 + 4 * (0.821)^2 = 3.697
For a base price of $10/hour: New Price = 3.697 * $10 = $36.97/hour
```

### Scenario 4: Peak Demand

* Historical Usage: Very High (H = 1.0)
* Current Availability: 100% resources occupied (C = 1.0)

```
Weighted Sum = 0.35 * 1.0 + 0.65 * 1.0 = 1.0
Demand Factor = 1 + 4 * (1.0)^2 = 5.0
For a base price of $10/hour: New Price = 5.0 * $10 = $50/hour
```

## 8. Conclusion

This comprehensive dynamic pricing strategy ensures that our prices reflect both the most common price points among our Computing Providers and the current market demand. By using a frequency-weighted average of CP prices, we establish a fair baseline that represents the consensus pricing in our provider ecosystem. The subsequent application of our quadratic dynamic pricing model then allows us to respond effectively to changes in demand, ensuring efficient resource allocation and maximizing value for all stakeholders in our computing ecosystem. This approach not only captures market trends in resource pricing but also adapts quickly to shifts in user demand, creating a responsive and efficient pricing mechanism.
