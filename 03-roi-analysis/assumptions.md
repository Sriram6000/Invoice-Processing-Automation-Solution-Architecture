# Assumptions

## Scope & Currency
- Scope: Accounts Payable operations across **20 countries**.
- Currency: **USD** for all calculations.

## Operational Assumptions
- Annual Volume (Global): **4.8M**  
- Growth: **+25% YoY**
- Manual handling time: **6.0 min/invoice**
- Productive hours per FTE per year: **1,680** (140 hrs/month × 12)
- Manual error rate: **2.5%** of invoices
- Delayed payments (SLA breaches): **20%** of invoices (late at invoice level)

## Financial Assumptions
- Fully loaded Accounts Payable FTE (blended): **$40,000/year**
- (No data as of now) QC/Specialist FTE: **$XX,000/year**
- (No data as of now) Overhead on labor: **+25%**
- (No data as of now) SLA penalties: **$X per late invoice** or **$Y per breach event**

## Automation (Pattern 1: High Volume – Azure Blob & Functions)
- **Manual reduction:** **85%** (residual **0.9 min/invoice**)
- **Error rate post‑automation:** **0.5%**
- **SLA late post‑automation:** **≤5%** of invoices
- **Per‑invoice license:** **$0.03** *(Power Platform/AI Builder scale)*
- **Infra (annual):** **$150,000** *(Azure Functions, Blob, monitoring, log storage)*
- **Support team:** **6 FTE × $40,000 = $240,000/year**
- **One‑time implementation (Year 1):** **$350,000** *(build, config, country pilot, change mgmt)*


> **Note:** Update these values as you gather more accurate data from Finance, Procurement, and platform licensing.


