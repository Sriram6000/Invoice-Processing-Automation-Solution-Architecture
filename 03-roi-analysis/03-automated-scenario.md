# Automated Scenario — Pattern 1 (High Volume)

Model the **target-state run‑rate** when Pattern 1 is implemented, including residual labor, licenses, infra, support, and one‑time capex.

## Improvement Levers

- Manual minutes ↓ **85%** → residual **0.9 min/invoice**
- Error rate ↓ **2.5% → 0.5%**
- SLA late ↓ **20% → ≤5%**
- New costs: **License**, **Infra**, **Support FTEs**, **Capex (Year 1)**

## Key Formulas

> Residual Minutes/Invoice = 6 × (1 - 0.85) = 0.9

> FTE_auto = (Invoices × 0.9 / 60) / 1,680

> License Cost = Invoices × $0.03

> Support Cost = 6 × $40,000 = $240,000

> Total Auto Cost (Yr 1) = Residual Labor + License + Infra + Support + Capex

> Total Auto Cost (Yr 2+) = Residual Labor + License + Infra + Support

## Year-by-Year Snapshot

| Period           | Annual Invoices | Residual FTE (calc) | Errors (# & %)    | Delayed Payments (# & %) | Residual Labor | License  | Infra  | Support | Capex (Y1) | **Total Auto Cost** |
|------------------|----------------:|--------------------:|------------------:|--------------------------:|---------------:|---------:|-------:|--------:|-----------:|--------------------:|
| This Year        | 4,800,000       | 42.9               | 24,000 (0.5%)     | 240,000 (5%)              | $1,714,286     | $144,000 | $150k | $240k  | **$350k**  | **$2,598,286**      |
| Next Year        | 6,000,000       | 53.6               | 30,000 (0.5%)     | 300,000 (5%)              | $2,142,857     | $180,000 | $150k | $240k  | $0         | **$2,712,857**      |
| Year After Next  | 7,500,000       | 67.0               | 37,500 (0.5%)     | 375,000 (5%)              | $2,678,571     | $225,000 | $150k | $240k  | $0         | **$3,293,571**      |

> Assumes infra and support are fairly stable across years; adjust by country rollout if needed.
