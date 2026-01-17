# Stochastic-Production-Planning


## 1. Problem Statement

This project addresses the complex challenge of aggregate production planning for a multi-product electronics portfolio over a 12-week horizon. Traditional planning often fails because it assumes demand is a fixed number. This model was developed to study production stability and financial resilience under demand uncertainty for three core products:

Product A: High-volume Monitor

Product B: Professional Grade TV

Product C: Luxury Refrigerator

The primary objective is to optimize Stage 1 decisions (Workforce and Production quantities) such that the expected costs of Stage 2 (Inventory, Backorders, and Overflow) are minimized across all possible future scenarios.


## 2. Executive Summary

The model successfully balanced labor stability with inventory agility. By optimizing for the "Expected Value" of demand rather than a single forecast, the system maintained high profit margins even with significant storage penalties.

### Financial Performance Highlights

- Cumulative Profit: $2,476,316.00
- Operating Margin: ~81.4%
- Cost Efficiency: Labor remains the primary cost driver (79.25%), followed by material production (11.58%). Strategic inventory placement kept backorder penalties to a record low of 1.63%.

### Operational Metrics

Product,Total Demand,Units Produced,Turnover Ratio,Avg Unit Cost,Profit Margin
A (Monitor),2829,3045,8.56,$51.09,65.9%
B (TV),2552,2702,7.26,$61.72,87.7%
C (Fridge),2295,2464,6.70,$98.27,84.9%


## 3. Business & Managerial Insights

Strategic Note: The following insights were derived from the model's behavior across 12 weeks of fluctuating demand.


## 4. Model Details

The system utilizes a Two-Stage Stochastic Program:

1. **Stage 1 (Here-and-Now):** Decisions made before demand is realized. This includes the weekly production quantity ($X$), workforce size ($W$), and hiring/firing ($H/F$) actions.

2. **Stage 2 (Recourse):** Decisions made after demand uncertainty is resolved. This includes warehouse allocation (Regular vs. Overflow), inventory carry-over ($I$), and backorder management ($B$).


### Uncertainty Modelling

Demand is treated as a random variable following a Poisson Distribution. Three scenarios are considered to represent the volatility of the electronics market:

- **Low Demand (25% Prob):** Represents a market contraction.
- **Medium Demand (50% Prob):** The baseline expected market performance.
- **High Demand (25% Prob):** Represents a surge or "viral" sales event.


## 5. Problem Formulation

### Objective Function

The goal is to minimize the total cost across the Stage 1 decisions and the expected costs of Stage 2 recourse actions:

$$\min Z = \sum_{p \in P} \sum_{t \in T} \left( C_{p,t}^{prod} X_{p,t} + C_{p,t}^{reg} R_{p,t} + C_{p,t}^{ot} O_{p,t} + C_{p,t}^{hire} H_{p,t} + C_{p,t}^{fire} F_{p,t} \right) + \sum_{s \in S} \pi_s \cdot Q(x, s)$$

Where $Q(x, s)$ represents the Stage 2 costs for scenario $s$:

$$Q(x, s) = \sum_{p \in P} \sum_{t \in T} \left( C_{p,t}^{hold} I_{p,t,s}^{reg} + 3 \cdot C_{p,t}^{hold} I_{p,t,s}^{ovr} + C_{p,t}^{back} B_{p,t,s} \right)$$

### Key Constraints

#### 1. Workforce Balance

The workforce in any period is the result of the previous period plus hiring and minus firing:

$$W_{p,t} = W_{p,t-1} + H_{p,t} - F_{p,t} \quad \forall p, t$$

#### 2. Production Capacity

Production is limited by the labor hours available (Regular + Overtime):

$$\tau_p X_{p,t} \le R_{p,t} + O_{p,t} \quad \forall p, t$$

$$O_{p,t} \le \alpha \cdot R_{p,t} \quad (\text{Overtime Limit})$$

#### 3. Inventory Flow (Mass Balance)

Inventory stays consistent across time, accounting for the realized demand of each scenario:

$$I_{p,t,s}^{total} = I_{p,t-1,s}^{total} + X_{p,t} - D_{p,t,s} + B_{p,t,s} - B_{p,t-1,s} \quad \forall p, t, s$$

#### 4. Global Warehouse ConstraintsPhysical space is limited for the regular warehouse; excess must go to overflow:

$$\sum_{p \in P} \sigma_p I_{p,t,s}^{reg} \le \text{Capacity}_{max} \quad \forall t, s$$

## 6. Technical Stack

- **Language:** Python 3.x
- **Optimization Engine:** Pyomo (Modeling Objects)
- **Solver:** HiGHS (Linear/Integer Optimizer)
- **Visualization:** Plotly (Multi-axis Interactive Financial Dashboards)
- **Data Handling:** Pandas / NumPy









