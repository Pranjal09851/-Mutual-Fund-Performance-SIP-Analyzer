# -Mutual-Fund-Performance-SIP-Analyzer

Interactive Power BI dashboard on Indian mutual fund NAV data, backed by PostgreSQL. Compares funds on return **and** risk and simulates monthly SIPs.

![Dashboard]<img width="1476" height="737" alt="image" src="https://github.com/user-attachments/assets/c4157413-3da5-4235-936d-7a079a9b8b4b" />


## What it answers
- Which funds gave the best return per unit of risk?
- How deep did each fund fall in the worst period?
- What would a monthly SIP of ₹X have grown to?

## Tech
PostgreSQL · SQL (window functions, CTEs, materialized views) · Python · Power BI (DirectQuery) · DAX
Data: AMFI NAV via [mfapi.in](https://www.mfapi.in)

## How it was built
| Layer | What I did |
|---|---|
| Ingestion | Python script downloads NAV history for 13 Direct Growth funds to CSV |
| Storage | Star schema: `dim_fund`, `fact_nav`, `dim_date`, with indexes |
| Validation | Row counts, date ranges, checks for daily NAV jumps above 15% |
| Analytics | Daily return, running peak and drawdown, CAGR, volatility, Sharpe (6.5% assumed risk-free rate), category rank |
| SIP | Monthly instalments stored as units per ₹1 so any amount can be simulated |
| Serving | Materialized views wrapped in BI views for Power BI |
| Report | DirectQuery model, DAX measures, What-If slider for SIP amount, one-page dashboard |

## Dashboard features
KPI cards · risk vs return scatter · growth of ₹10,000 · drawdown chart · fund leaderboard · SIP growth chart · slicers for category, date range and SIP amount

## Run it
1. Run the Python script to create `funds.csv` and `nav.csv` (or use `data/`)
2. Create `mf_analyzer_db` and run `sql/` scripts in order
3. Refresh materialized views: `mv_nav_enriched`, then `mv_fund_metrics`, then `mv_sip_monthly`
4. Open the `.pbix` and update the PostgreSQL connection

The fund data uses DirectQuery. The SIP amount slider is a small table created inside Power BI, so the model runs in mixed mode.

## Disclaimer
For learning and portfolio use only. Not investment advice.
