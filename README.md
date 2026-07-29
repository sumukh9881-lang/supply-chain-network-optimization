
# Supply Chain Network Optimization — Automotive Component Manufacturer

A linear programming model that determines the optimal warehouse network and shipment routing strategy for a hypothetical automotive component manufacturer, built using Python and PuLP.
## How to Run

Open `Supply_Chain_Network_Optimization.ipynb` in Google Colab and select **Runtime → Run all**. PuLP is installed automatically in the first code cell — no other setup required.

## Business Problem

A premium automotive component manufacturer supplies parts to major OEM (vehicle manufacturer) clusters across India. The company operates:

- **2 manufacturing plants** — Pune and Chennai (combined capacity: 1,000 units/day)
- **4 candidate distribution centers (DCs)** — Delhi, Mumbai, Bengaluru, Kolkata — each with a different fixed monthly cost and throughput capacity
- **5 customer clusters** — Gurgaon, Chennai, Pune, Ahmedabad, Jamshedpur — each with fixed daily demand

**Goal:** determine which DCs to open, and how many units to ship along each route, to meet all demand at the lowest possible total daily cost.

## Approach

This was modeled as a two-echelon capacitated facility location and transportation problem, solved using linear programming (Python's PuLP library, CBC solver). The model includes:

- Binary decision variables for DC open/close decisions
- Continuous decision variables for shipment quantities across all routes
- An objective function minimizing total cost (fixed DC costs + shipping costs)
- Constraints for plant capacity, customer demand, DC capacity, and flow balance

## Data Sources & Assumptions

Cost data was constructed using real published benchmarks combined with documented assumptions:

- **Freight cost:** based on India's average road freight rate of ₹3.78/tonne-km (2025 logistics industry data)
- **Warehouse rent:** based on 2025 industrial real estate rates across Delhi-NCR, Mumbai, Bengaluru, and Kolkata (Vestian)
- **Assumptions:** average component weight (15 kg/unit), warehouse size scaling (40 sq ft per unit/day capacity), and additional fixed operating costs — all documented in the notebook

## Key Findings

- **Optimal network:** Open Mumbai, Bengaluru, and Kolkata; keep Delhi closed — total daily cost of ₹13,07,210
- **Fixed costs dominate facility decisions:** Delhi→Gurgaon is the cheapest single route in the network (₹2/unit), yet Delhi remains closed, since its fixed cost outweighs the savings from that one route
- **Network resilience:** a simulated 15% freight cost increase raised total cost proportionally but did not change the optimal facility footprint
- **Facility right-sizing:** the base model showed Kolkata operating at only 67% of its capacity. Testing a smaller facility option confirmed the smaller size was optimal, saving approximately ₹27.6 lakh/year
- **Known limitation:** a direct plant-to-customer shipping extension caused the model to bypass all DCs, revealing that the cost model doesn't yet account for consolidation economies of scale — documented as a limitation for future work

## Results Summary

| Scenario | Total Daily Cost (₹) | Open DCs |
|---|---|---|
| Base model | 13,07,210 | Mumbai, Bengaluru, Kolkata |
| High freight (+15%) | 13,18,791.5 | Mumbai, Bengaluru, Kolkata (unchanged) |
| Kolkata right-sized | 12,15,210 | Mumbai, Bengaluru, Kolkata (Small) |
| Direct shipping allowed* | 46,990 | None (see limitation below) |

*See "Known Limitation" above — this result reflects an oversimplified cost model, not a realistic recommendation.

## Tools Used

Python, PuLP (linear programming), pandas

## Notebook

See `Supply_Chain_Network_Optimization.ipynb` for the full model, code, and results.
