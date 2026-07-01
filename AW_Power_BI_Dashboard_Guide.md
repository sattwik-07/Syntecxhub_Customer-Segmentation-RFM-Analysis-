# Project 1: Customer Segmentation using RFM Analysis
## Power BI Dashboard Build Guide (AdventureWorks dataset)

Built from the actual files you provided — `AdventureWorksDW.XLSX` (FactInternetSales + DimCustomer + DimGeography) and `RFM_Table.xlsx` (the official 128-combination RFM Score → Segment lookup, plus segment descriptions and marketing actions). All 128 R/F/M score combinations in the data matched the lookup table with zero gaps.

I can't generate a native `.pbix` file directly in this environment, but the CSVs below drop straight into Power BI with minimal setup, and `AW_RFM_Dashboard.xlsx` is a fully working version you can open right now.

---

## 1. Data files

| File | Rows | Use |
|---|---|---|
| `AW_RFM_Customer_Segments.csv` | 18,484 customers | Main table — Recency, Frequency, Monetary, R/F/M scores, Segment, plus customer demographics (country, income, occupation, gender) |
| `AW_Transactions.csv` | 60,398 order lines | Cleaned FactInternetSales — use for time-series/trend visuals |
| `AW_Segment_Summary_Full.csv` | 11 rows | One row per segment: customer count, avg RFM, revenue share, description, and marketing action (straight from `RFM_Table.xlsx`) |

**Source:** AdventureWorksDW, FactInternetSales, 60,398 Internet sales order lines, Jul 2005–Jul 2008, 18,484 customers. Reference date for Recency = 2008-08-01 (one day after the last order).

## 2. Power BI setup

1. **Get Data → Text/CSV** → import all three files.
2. Set `CustomerKey` as **Whole Number** but mark it "Do not summarize" in the field's default summarization.
3. **Model view**: relate `AW_Transactions[CustomerKey]` → `AW_RFM_Customer_Segments[CustomerKey]` (many-to-one), and `AW_RFM_Customer_Segments[Segment]` → `AW_Segment_Summary_Full[Segment]` (many-to-one).
4. Sort `Segment` by `SegmentOrder` (already in the customer table) so charts display in a consistent Champions → Lost order.

## 3. Recommended visuals

- **Cards:** Total Customers (18,484), Total Revenue, Avg Order Value, Avg Recency
- **Bar chart:** Customers by Segment (sorted by SegmentOrder)
- **Pie/Donut:** % of Revenue by Segment — headline insight: Champions ≈ 10% of customers, ≈ 30% of revenue; At Risk ≈ 15% of customers, ≈ 29% of revenue
- **Bar chart:** Avg Recency (days) by Segment, ascending
- **Scatter plot:** Frequency (x) vs Monetary (y), colored by Segment, sized by Recency
- **Table:** Segment × Customers × Avg RFM × Description × Marketing Action (pulled straight from `AW_Segment_Summary_Full`)
- **Map:** Customers/Revenue by Country (from the Country column)
- **Line chart** (from `AW_Transactions`): Monthly revenue trend
- **Slicers:** Segment, Country, Occupation

## 4. DAX measures

```dax
Total Revenue = SUM(AW_RFM_Customer_Segments[Monetary])
Total Customers = DISTINCTCOUNT(AW_RFM_Customer_Segments[CustomerKey])
Avg Order Value = DIVIDE([Total Revenue], SUM(AW_RFM_Customer_Segments[Frequency]))
% Revenue by Segment = DIVIDE(SUM(AW_RFM_Customer_Segments[Monetary]), CALCULATE(SUM(AW_RFM_Customer_Segments[Monetary]), ALL(AW_RFM_Customer_Segments[Segment])))
```

## 5. In Tableau instead?

Same two-file import (`AW_RFM_Customer_Segments.csv` + `AW_Transactions.csv`), joined on `CustomerKey`. Build the same worksheets, then combine into a dashboard with a Segment filter action.

---

## Segmentation logic used

- **Recency** = days since last order (ref date 2008-08-01)
- **Frequency** = distinct SalesOrderNumbers per customer
- **Monetary** = total SalesAmount
- R/F/M each scored 1–5 by quintile → combined into a 3-digit code → mapped to one of 11 segments using the exact lookup table from `RFM_Table.xlsx`

## Key findings (visible now in `AW_RFM_Dashboard.xlsx`)

| Segment | % of Customers | % of Revenue |
|---|---|---|
| Champions | 10.3% | **29.9%** |
| At Risk | 15.1% | **28.9%** |
| Loyal | 8.5% | 19.6% |
| Cannot Lose Them | 5.1% | 8.8% |
| Potential Loyalist | 15.5% | 2.4% |
| Hibernating customers | 13.2% | 1.5% |

**Takeaway:** Unlike a typical RFM distribution, revenue here is split almost evenly between two very different groups — **Champions** (actively buying, high value) and **At Risk** (high historical value, gone quiet). That makes win-back campaigns for At Risk just as high-priority as loyalty rewards for Champions; nearly 30 cents of every revenue dollar is sitting with customers who haven't ordered recently.
