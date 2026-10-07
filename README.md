# Construction Preconstruction Analytics Portfolio

Three Excel-based projects that apply forecasting, statistical modeling, and dashboard design to preconstruction decisions: how much work is coming, whether the team has capacity for it, and which projects are worth bidding.

| Project | Question it answers | Tools |
|---|---|---|
| [Demand Forecasting & Capacity Optimization](demand-forecasting-capacity-model/) | Do we have the labor capacity for expected demand, and should we bid more or be selective? | Excel (FORECAST.ETS, XLOOKUP, scenario analysis) |
| [Construction Risk & Field Performance Dashboard](construction-risk-dashboard/) | Which project types and contract types carry the most cost and schedule risk? | Excel (COUNTIFS/AVERAGEIF, pivot tables, charts) |
| [Feasibility Risk Model](feasibility-risk-model/) | How risky is a specific project relative to its peers, and what is driving it? | R (logistic regression), Excel (XLOOKUP, dynamic dashboard) |

---

## 1. Demand Forecasting & Capacity Optimization

**File:** [`Demand_Forecasting_Capacity_Optimization.xlsx`](demand-forecasting-capacity-model/Demand_Forecasting_Capacity_Optimization.xlsx)

**Data:** 403 months of U.S. advance retail sales (FRED series RSXFS, Jan 1992 to Jul 2025), used as a proxy demand series.

**What it does**
- Forecasts monthly demand from Aug 2025 through Dec 2033 using Excel's exponential smoothing (`FORECAST.ETS`) with a 95% confidence band
- Converts demand into projects and required labor hours using adjustable assumptions (10,000 demand units per project, 500 labor hours per project, 20,000 available hours)
- Compares utilization across Low (0.9x), Base (1.0x), and High (1.1x) demand scenarios against an 85% target
- Returns a bidding recommendation automatically: "Selective Bidding" above target, "Pursue Additional Projects" below it

**Results**

| Scenario | Utilization | vs. 85% target |
|---|---|---|
| Low | 75.5% | Room to pursue more work |
| Base | 83.8% | Near target |
| High | 92.2% | Over target, bid selectively |

Under the High scenario the team needs about 18,450 of its 20,000 available hours. Since skilled labor can't realistically be staffed to 100%, the model recommends prioritizing high-margin, scope-fit projects over buying work to fill capacity.

---

## 2. Construction Risk & Field Performance Dashboard

**File:** [`Construction_Risk_Field_Performance_Dashboard.xlsx`](construction-risk-dashboard/Construction_Risk_Field_Performance_Dashboard.xlsx)

**Data:** 150 sample construction projects across six project types and four contract types (Indianapolis-area regions), plus 10,000 field sensor readings. Sample data, not actual company records.

**What it does**
- Summarizes project risk (High / Medium / Low), average cost overrun, schedule overrun, and RFI count
- Breaks down risk tier by project type and overruns by contract type
- Analyzes 10,000 sensor readings for delay risk by recommended field action (adjust schedule, reallocate workers, etc.)
- All metrics are formula-driven (`COUNTIFS`, `AVERAGEIF`), so the dashboard updates when the data changes

**Results**
- 104 low, 43 medium, and 3 high risk projects; average cost overrun 9.2% and schedule overrun 16.7%
- **Infrastructure is the riskiest project type:** 10 of 19 projects (53%) rated medium or high risk, vs. 2 of 25 for multifamily
- **Contract type matters:** Cost-Plus had the lowest cost overrun (5.5%) while Design-Build had the highest (11.7%); GMP contracts ran the longest schedule overruns (18.1%)
- About 49% of sensor readings flagged delay risk, and the rate was nearly flat (48% to 51%) across every field action, so no single action stood out as more effective

---

## 3. Feasibility Risk Model

**File:** [`Feasibility_Risk_Model.xlsx`](feasibility-risk-model/Feasibility_Risk_Model.xlsx)

**Data:** 3,245 projects across five types (Road, Power Plant, Building, Bridge, Water Infrastructure), each with cost, risk score, environmental impact, timeline, complexity, resource allocation, cost deviation history, and stakeholder priority. Sample dataset.

**What it does**
- Trains a logistic regression in R on eight project variables to predict the probability a project is feasible (Feasible = 1; Not Feasible and Borderline = 0)
- Splits projects into risk tiers by predicted probability using tertile cutoffs, so each tier holds about a third of projects. Tiers are relative to peer projects, not absolute
- Excel dashboard: enter a Project ID to see its feasibility probability, risk tier, bid recommendation, and cost vs. its tier average
- Flags risk drivers automatically when a project is at or above the 67th percentile on cost, risk score, environmental impact, timeline, or complexity
- Filters the tier distribution by project type

**Results**
- For Power Plant projects, high-risk projects averaged $3.24M vs. $4.50M for low-risk ones, so the model does not simply equate expensive with risky
- The model's discriminative power is limited (AUC = 0.53). The tiers are useful for ranking projects against each other, but next steps include feature engineering and testing tree-based models to improve separation

---

## Relevance to Preconstruction

| Preconstruction task | Where it shows up |
|---|---|
| Forecasting future workload | Project 1 |
| Capacity planning and go/no-go bidding | Project 1 |
| Identifying high-risk project and contract types | Project 2 |
| Scoring individual projects before bidding | Project 3 |

## Repository Structure

```
excel-analytics-portfolio/
├── README.md
├── demand-forecasting-capacity-model/
│   └── Demand_Forecasting_Capacity_Optimization.xlsx
├── construction-risk-dashboard/
│   └── Construction_Risk_Field_Performance_Dashboard.xlsx
└── feasibility-risk-model/
    └── Feasibility_Risk_Model.xlsx
```

**Author:** Jack Andersen, B.S. Applied Statistics, Purdue University (May 2027)
