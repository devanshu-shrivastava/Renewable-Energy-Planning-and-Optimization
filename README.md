# Optimal Energy Mix Planning for India
### Using Excel Solver to minimise the total cost of Energy Technologies to meet total demand under climate commitments and capacity constraints

**Course:** Planning and Operations Engineering — Rutgers University, New Brunswick  
**Instructor:** Dr. David W. Coit  
**Author:** Devanshu Shrivastava, MEng Energy Systems  
**Date:** May 1, 2026

---

## Overview

India is the third-largest producer and consumer of electricity globally, with energy demand projected to grow 2–2.5× by 2047. Historically built on coal, India's power sector now faces the dual challenge of meeting rapidly rising demand while honouring ambitious climate commitments — including 500 GW of non-fossil capacity by 2030.

This project formulates India's energy mix planning challenge as a **Linear Programming (LP) resource-allocation problem** and solves it using **Excel Solver (Simplex LP)**, guaranteeing a globally optimal solution.

> **Core Question:** What is the least-cost combination of Solar, Wind, Hydro, Coal, and Gas capacity across India's major states that meets peak demand while satisfying climate policy constraints?

---

## Problem Formulation

### Scope
- **10 States:** Uttar Pradesh, Maharashtra, Tamil Nadu, Gujarat, Rajasthan, Madhya Pradesh, Karnataka, Andhra Pradesh, Himachal Pradesh, Assam
- **5 Technologies:** Solar PV, Wind, Hydropower, Coal, Natural Gas
- **50 Decision Variables:** One per state-technology combination (x_ij = MW installed)
- **100 Constraint Inequalities** across 9 constraint categories

### Objective Function
Minimise total capital cost of installed capacity across all states and technologies:

```
Minimise Z = Σᵢ Σⱼ (cⱼ · xᵢⱼ)
```

| Technology | Capital Cost (USD/MW) |
|------------|----------------------|
| Solar PV | 55,000 |
| Wind | 65,000 |
| Hydro | 80,000 |
| Coal | 45,000 |
| Gas | 30,000 |

### Constraints

| # | Constraint | Description |
|---|-----------|-------------|
| 1 | Demand Satisfaction | Each state must meet its peak electricity demand (MW) |
| 2 | Capacity Factors | Technology output scaled by state-specific capacity factors (NASA POWER / IEA data) |
| 3 | Coal Ceiling Cap | Coal ≤ 75% of current capacity per state (NDC phase-down trajectory) |
| 4 | Coal Floor (Just Transition) | Coal ≥ minimum threshold per state (protects coal-dependent communities) |
| 5 | Grid Stability | Dispatchable capacity (Coal + Gas + β·Hydro) ≥ 25% of peak demand |
| 6 | Min Solar | State-level solar floor from MNRE renewable targets |
| 7 | Min Wind | State-level wind floor from MNRE renewable targets |
| 8 | Min Hydro | State-level hydro floor from MNRE renewable targets |
| 9 | Land Resource Cap | Max Solar and Wind capacity per state based on available land |

### Peak Demand by State

| State | Peak Demand (MW) |
|-------|-----------------|
| Uttar Pradesh | 28,000 |
| Maharashtra | 32,000 |
| Tamil Nadu | 20,000 |
| Gujarat | 22,000 |
| Rajasthan | 19,000 |
| Madhya Pradesh | 15,000 |
| Karnataka | 14,500 |
| Andhra Pradesh | 13,000 |
| Himachal Pradesh | 2,000 |
| Assam | 2,500 |

---

## Results

### Optimal Solution

**Minimum Total Capital Cost: $17.68 Billion**  
*(362,835 MW of total new capacity across 10 states)*

| State | Solar (MW) | Wind (MW) | Hydro (MW) | Coal (MW) | Gas (MW) |
|-------|-----------|-----------|------------|-----------|---------|
| Uttar Pradesh | 22,000 | 2,000 | 500 | 5,400 | 23,024 |
| Maharashtra | 15,000 | 5,000 | 1,000 | 4,800 | 28,165 |
| Tamil Nadu | 10,000 | 12,000 | 500 | 2,400 | 14,906 |
| Gujarat | 30,000 | 15,000 | 200 | 2,700 | 10,581 |
| Rajasthan | 40,000 | 8,000 | 200 | 2,100 | 7,435 |
| Madhya Pradesh | 10,000 | 3,000 | 500 | 2,550 | 12,182 |
| Karnataka | 12,000 | 6,000 | 2,000 | 1,200 | 10,494 |
| Andhra Pradesh | 12,000 | 8,000 | 500 | 2,250 | 7,612 |
| Himachal Pradesh | 800 | 400 | 4,000 | 0 | 9 |
| Assam | 500 | 200 | 1,500 | 150 | 2,076 |

### Key Patterns

**Solar dominates desert states** — Rajasthan leads with 40,000 MW (highest solar irradiance in India, CF = 0.25). Gujarat follows with 30,000 MW.

**Hydro dominates Himalayan states** — Himachal Pradesh allocates 4,000 MW of hydro (CF = 0.45, highest of any state) with near-zero coal.

**Wind leads coastal/southern states** — Tamil Nadu receives 12,000 MW of wind, reflecting India's best onshore wind corridor (CF = 0.30).

**Gas fills the gap in high-demand, low-renewable states** — Uttar Pradesh receives 23,023 MW of gas, the highest of any state, as the cheapest technology ($30,000/MW) to fill demand after renewables minimums are met.

**Andhra Pradesh and Karnataka receive zero gas** — where coal is not at its ceiling and renewable CFs are adequate, coal provides cheaper baseload than gas.

---

## Constraint Sensitivity Analysis

| Constraint Removed | New Minimum Cost | Cost Difference |
|-------------------|-----------------|----------------|
| Demand not required to be met | $14.68 Billion | −$3.00 billion |
| Without Coal Ceiling Cap | $17.68 Billion | $0 (Non-binding) |
| Without Coal Floor (Just Transition) | $17.20 Billion | −$480 million |
| Without Min Conventional Sources (Grid) | $17.68 Billion | $0 (Non-binding) |
| **Without Minimum Renewables Constraint** | **$6.40 Billion** | **−$11.28 billion** |
| Without Land Resource Cap | $17.68 Billion | $0 (Non-binding) |

### Key Policy Insight

> **India's climate commitments add $11.28 billion to the system cost** — a 64% premium above the pure least-cost solution of $6.40 billion. Without the renewables mandate, the LP would default almost entirely to gas and coal. This quantifies the economic cost of India's NDC and 500 GW clean energy targets: the energy transition is feasible but not free.

---

## Tools Used

- **Microsoft Excel** — Model formulation, decision variables, constraint setup
- **Excel Solver (Simplex LP)** — Globally optimal LP solution
- **Linear Programming** — Resource allocation formulation
- **NASA POWER & IEA Data** — State-specific capacity factors

---

## Assumptions & Limitations

- Static single-period model (no multi-year dynamics or intermittency)
- Capital costs only — excludes fuel costs, O&M, transmission, and social costs
- Continuous LP (no integer constraints on plant unit sizes)
- Uniform capital costs across states for the same technology

### Future Extensions
- Multi-period MILP with stochastic renewable generation
- LCOE-based cost structure instead of pure capital cost
- Battery storage and pumped hydro as additional decision variables
- Alignment with CEA's Optimal Generation Mix 2030 framework

---

## Repository Contents

| File | Description |
|------|-------------|
| `Energy_Mix_India.xlsx` | Main Excel model with decision variables, constraints, and Solver setup |
| `README.md` | This file |

> **Note:** Update the filename above to match your actual Excel file before uploading.

---

## Author

**Devanshu Shrivastava**  
MEng Energy Systems, Rutgers University  
[linkedin.com/in/shrivastavadevanshu](https://linkedin.com/in/shrivastavadevanshu)  
devanshushrivastava28@gmail.com
