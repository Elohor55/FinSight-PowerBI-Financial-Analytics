# FinSight Financial Analytics: Power BI P&L and Cost Driver Analysis

A Power BI project that turns 4,500 ledger transactions into a monthly Profit & Loss statement and explains **why revenue grew 52.6% in 2025 while profit got worse**.

> **Company (case study):** FinSight Distribution & Consumer Products Ltd., a Nigerian FMCG distributor with 8 branches.
> **Period:** January 2024 to December 2025 | **Currency:** NGN

---

## Business problem

Management reported strong sales growth but no matching profit growth. The goal is to build a reusable P&L model and dashboard that answers:

1. Why did revenue growth not become profit growth?
2. Which cost lines drove the margin squeeze?
3. How did each branch perform?
4. What should leadership do about it?

## Key findings (NGN millions)

| | 2024 | 2025 | Change |
|---|---|---|---|
| Revenue | 1,410.1 | 2,152.2 | +52.6% |
| Cost of Sales | 1,174.1 | 1,901.1 | +61.9% |
| Gross Profit | 236.0 | 251.1 | +6.4% |
| Gross Margin | 16.7% | 11.7% | -5.0 pts |
| Operating Expenses | 888.7 | 1,581.9 | +78.0% |
| Net Profit Before Tax | -691.1 | -1,375.4 | -684.3 |

- Costs rose by about 1,433M against about 749M of extra revenue and other income.
- **Biggest cost drivers:** diesel and generator fuel (+183%, about +324M), import and customs charges (+135%), inbound and outbound freight (+185% to +190%), and FX losses (+99%).
- **Not the problem:** salaries (+37%), rent (+33%) and electricity (+14%) grew slower than revenue.
- **Every branch loses money** in both years. Only Abuja Central improved (-100.3M to -76.2M). Lagos Mainland (Ikeja) has the largest loss (about -378M in 2025).
- The business was already in loss in 2024, with operating expenses at 63% of revenue.



## Tools

- Power BI Desktop (Power Query, DAX, data modelling, report design)
- Microsoft Excel (source data and validation pivots)

## Data model

Star schema with one fact table and five dimensions.

```
                DimDate
                   |
DimAccount --- FactFinancialTransactions --- DimBranch
                   |            |
            DimDepartment   DimCostCentre
```

| Table | Rows | Purpose |
|---|---|---|
| FactFinancialTransactions | 4,500 | Every ledger entry (amounts are always positive) |
| DimAccount | 38 | Chart of accounts, P&L mapping and `NaturalSignMultiplier` (+1 income, -1 cost) |
| DimBranch | 8 | Branch, city, state, region |
| DimDepartment | 10 | Department and manager |
| DimCostCentre | 12 | Functional cost pools |
| DimDate | generated | Calendar table built in DAX |

All relationships are one-to-many, single direction, from dimension to fact.

## Approach

1. **Profile and clean** the data in Power Query (types, blank keys, text casing, rounding).
2. **Model** a star schema and mark a DAX calendar as the date table.
3. **Sign the amounts** with `NaturalSignMultiplier` so revenue is positive and costs negative.
4. **Write measures** for the P&L, margins, year-over-year, month-over-month and year-to-date.
5. **Build the report:** executive summary, P&L matrix, cost driver analysis, branch performance.
6. **Validate** the totals against an Excel pivot before publishing.
7. **Recommend** actions based on the findings.

## Data quality issues handled

| Issue | Count | Treatment |
|---|---|---|
| Blank DepartmentID | 96 rows | Set to `UNASSIGNED`, rows kept |
| Blank CostCentreID | 58 rows | Set to `UNASSIGNED`, rows kept |
| Lowercase descriptions | 55 rows | Capitalised |
| Repeated TransactionReference | 8 references | Flagged, not deleted |
| Float noise in amounts | several | Rounded to 2 decimals |
| Duplicate chart of accounts sheet | 1 sheet | Not loaded |
| Repeated PLLineSort values across groups | n/a | Sort within group, never on this column alone |


## Dashboard pages

| Page | Content |
|---|---|
| Executive Summary | KPI cards, monthly revenue vs cost trend, year slicer |
| P&L Statement | Matrix by section and group, with year columns and variances |
| Cost Drivers | Top cost increases by account, expense share by group |
| Branch Performance | Revenue, cost and net profit by branch, region slicer |

Add screenshots to `docs/images/` and link them here:

```markdown
![Executive Summary](docs/images/executive_summary.png)
```

## Recommendations

1. Cut fuel cost with solar or hybrid power and bulk diesel contracts.
2. Reduce freight cost with fuller loads, fewer trips and renegotiated vendor rates.
3. Hedge or forward-buy FX for import payments and review local sourcing.
4. Reprice to pass through higher costs, starting with wholesale.
5. Replicate Abuja Central's cost practices in other branches.
6. Track gross margin and operating cost as a share of revenue every month.



## Limitations

- Department and cost-centre tagging looks inconsistent for some accounts, so departmental views should be treated with caution.
- Branch results depend on how costs were tagged to branches in the source data.
- The case brief describes profit as flat or declining, but the data shows a loss in both years. This project reports what the data shows.

## Author

**ELOHOR EGWEREHE ** | [[LinkedIn](https://www.linkedin.com/](https://www.linkedin.com/in/elohor-egwerehe-8739b9244/?lipi=urn%3Ali%3Apage%3Ad_flagship3_profile_view_base_contact_details%3BrPgnTiLMSv%2BvfdgwWuwKmA%3D%3D)) | [Email](egwerehe@gmail.com)

## License

MIT License. See [LICENSE](LICENSE).
# FinSight-PowerBI-Financial-Analytics
Financial Analysis that talks growth of business over the last 2years 
