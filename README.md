# Driver-Based Operational & Financial Simulator — Greggs PLC

A dynamic, driver-based forecasting model that links operational drivers (units sold, pricing, variable cost per unit, fixed costs) to a full IFRS-aligned P&L, enabling instant "what-if" scenario analysis on EBITDA, EBIT, Profit Before Tax, and Net Profit.

Built and documented by **Ayoub Feddal, Financial Analyst**.

---

## 1. Project Overview

Greggs plc's FY2024 Annual Report discloses a single, static set of financial results — turnover of £2,014.4m and profit before tax of £189.8m. This project rebuilds those results from the ground up as a **driver-based model**, decomposing revenue and costs into operational levers (footfall/units, transaction value, variable cost per unit, fixed cost per shop) across a synthetic 2,600-shop estate calibrated to the real reported figures.

The result is a reusable simulation engine that answers questions like:

- What happens to EBITDA if ingredient costs rise 10%?
- How much does a 3% menu price increase offset a cost shock?
- What is the combined effect of volume growth, cost inflation, and a pricing response?

## 2. Repository Contents

| File | Description |
|---|---|
| `colab_simulator.py` | Full Python script (Colab-ready) — Phases 1–3: data setup, P&L logic, scenario engine, and chart generation |
| `Greggs_Driver_Based_Simulator_Case_Study.pdf` | Executive case study — Executive Summary → Methodology → Key Insights |
| `input_dashboard.png` | Chart: distribution of driver inputs across the modeled shop estate |
| `scenario_sensitivity_chart.png` | Chart: EBITDA & PBT sensitivity across 8 scenarios |
| `README.md` | This file |

## 3. Methodology

### Phase 1 — Data Setup
Company-level FY2024 actuals (revenue, cost of sales, operating expenses, shop count) sourced from Greggs plc's published Annual Report and Accounts are used to derive an "average shop" driver profile. A synthetic dataset of 2,600 shop-level records is generated with realistic statistical variation around these averages — this keeps the model grounded in real, disclosed financial data rather than fully arbitrary assumptions. The only assumption not directly sourced from the accounts is the average transaction value (£3.20), which sits within the publicly reported range for UK food-on-the-go retail and is used as the bridge between revenue and unit volume.

### Phase 2 — Logic Build (IFRS-Aligned P&L)
```
Revenue              = Units Sold × Price per Unit
Cost of Sales        = Units Sold × Variable Cost per Unit
Gross Profit          = Revenue − Cost of Sales
Operating Expenses    = Fixed Costs (includes embedded D&A, per reported presentation)
EBITDA / EBIT         = Gross Profit − Operating Expenses
Net Finance Costs      = Revenue × finance cost ratio (proxy for IFRS 16 lease interest)
Profit Before Tax (PBT)= EBIT − Net Finance Costs
Tax                    = PBT × 25% (UK statutory corporation tax rate, FY2024)
Net Profit             = PBT − Tax
```
Depreciation & amortisation is disclosed as a memo line (not subtracted again) because it is already embedded within reported operating expenses — this avoids double-counting. At an aggregate (portfolio) level, the model reconciles to within ~11% of the reported FY2024 PBT; the residual variance reflects items not separately disclosed at shop level (e.g. central/head-office overheads, one-off items).

### Phase 3 — Scenario Testing
A `run_scenario()` function accepts percentage changes to any of the four core drivers (units sold, price per unit, variable cost per unit, fixed costs) and returns the resulting portfolio-level P&L, including EBITDA margin and a Net Cash Flow proxy. Eight scenarios are pre-built, covering individual driver shocks and a combined "growth + inflation + pricing response" case.

## 5. Key Outputs

- **Base case (FY2024 baseline):** EBITDA ≈ £174m, PBT ≈ £169m, EBITDA margin ≈ 8.6%
- **Input cost +10%:** EBITDA falls to ≈ £95m (-45%)
- **Fixed costs +8%:** EBITDA falls to ≈ £91m (-48%)
- **Price +3%:** EBITDA rises to ≈ £235m (+35%)
- **Combined growth + inflation + pricing scenario:** EBITDA ≈ £215m, PBT ≈ £210m — both above base case, showing a coordinated response can more than offset a significant cost shock

Full detail, charts, and business interpretation are provided in the accompanying PDF case study.

## 6. Compliance & Data Notes

- All baseline company figures (revenue, cost of sales, operating expenses, profit before tax, shop count) are sourced from Greggs plc's publicly disclosed FY2024 Annual Report and Accounts.
- The P&L structure follows IAS 1 presentation conventions for the income statement.
- UK corporation tax is applied at the FY2024 statutory main rate of 25% (HMRC).
- IFRS 16 lease costs are reflected via a finance-cost proxy ratio, as per-shop lease disclosures are not separately published.
- Shop-level data is **synthetic** (statistically generated to match disclosed portfolio totals) — it is not actual internal Greggs operational data, which is not publicly available. This project is an independent illustrative analysis and is not affiliated with or endorsed by Greggs plc.

## 7. Tech Stack

`Python 3` · `pandas` · `numpy` · `matplotlib` · `reportlab` (PDF generation) — designed to run entirely within Google Colab's free tier.

## 8. Author

**Ayoub Feddal** — Financial Analyst
Project: *Driver-Based Operational & Financial Simulator*
