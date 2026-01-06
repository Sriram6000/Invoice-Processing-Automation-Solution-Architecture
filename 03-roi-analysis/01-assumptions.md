# Assumptions

Defines the operational and financial drivers used across the ROI analysis for **Pattern 1: High Volume (Azure Blob + Azure Functions)** over a 3‑year horizon.

## Scope & Method
- **Scope:** Accounts Payable operations across **20 countries** with phased rollout.
- **Currency:**: USD for all calculations.
- **Rounding:** Dollar amounts rounded to the nearest whole; **FTEs used as calculated** (not rounded) for cost
- **Horizon:** 3 years (extendable to 5 years on request).
- **Scope Boundary:** Direct operating costs; excludes taxes and intercompany allocations unless explicitly noted.
- **Fair Comparison (Read This):**
  - **Baseline (Manual)** shown as **Direct Labor Only** (conservative lower bound).
  - **Automated (Pattern 1)** includes **Residual Labor + License + Infra + Support + one‑time Capex**.
- Development effort is excluded from ROI; analysis focuses on operational run-rate savings post-implementation.


## Rollout & Timing (Year 1)

Benefits are **not** instantaneous across 20 countries. Applying pro‑rata weights:

- **Q1:** 20% of annual volume  
- **Q2:** 60%  
- **Q3:** 80%  
- **Q4:** 100%

**Year 2+** assumed at full run‑rate. If country rollouts are staggered, adjust quarterly weights accordingly.

## Operational Assumptions
- Annual Volume (Global): **4.8M**  
- Growth: **+25% YoY**
- Manual handling time: **6.0 min/invoice**
- Productive hours per FTE per year: **1,680** (140 hrs/month × 12)
- Manual error rate: **2.5%** of invoices
- Delayed payments (SLA breaches): **20%** of invoices (late at invoice level)

## Financial Assumptions
- Fully loaded Accounts Payable FTE (blended): **$40,000/year**
- The below indirect cost are pending from the team 
  - QC/Specialist FTE: **$XX,XXX/year**
  - Overhead on labor: **+XX%**
  - SLA penalties: **$X per late invoice** or **$Y per breach event**

## Automation (Pattern 1: High Volume – Azure Blob & Functions)
- **Manual reduction:** **85%** (residual **0.9 min/invoice**)
- **Error rate post‑automation:** **0.5%**
- **SLA late post‑automation:** **≤5%** of invoices
- **Per‑invoice license:** **$0.03** *(Power Platform/AI Builder scale)*
- **Infra (annual):** **$150,000** *(Azure Functions, Blob, monitoring, log storage)*
- **Support team:** **6 FTE × $40,000 = $240,000/year**
- **One‑time implementation (Year 1):** **$350,000** *(build, config, country pilot, change mgmt)*

 ---
 |Back to : [ROI Analysis](README.md) | Next : [Baseline Manual](02-baseline-manual.md)|
|-|-|
