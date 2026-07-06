# Customer Segmentation using RFM Analysis

*Syntecxhub Data Analysis Internship — Task 3*

An RFM (Recency, Frequency, Monetary) analysis and customer segmentation model built on the AdventureWorks sales dataset, delivered as a working Excel dashboard plus Power BI/Tableau-ready data files.

---

## 📌 Project Overview

This project segments customers into behavioral groups (Champions, At Risk, Loyal, Cannot Lose Them, etc.) based on their purchase recency, frequency, and monetary value, then translates each segment into a targeted marketing recommendation.

**Objectives:**
- Clean and prepare transactional sales data
- Calculate RFM metrics for every customer
- Score and segment customers using a standard 11-segment RFM framework
- Analyze the behavior and revenue contribution of each segment
- Provide targeted marketing recommendations per segment
- Visualize the segmentation in an interactive dashboard

---

## 🗂️ Dataset

**Source:** AdventureWorksDW (Microsoft sample data warehouse)

| Table | Description |
|---|---|
| `FactInternetSales` | 60,398 order lines, Jul 2005 – Jul 2008 |
| `DimCustomer` | 18,484 customers with demographics |
| `DimGeography` | Customer location data |
| `RFM_Table.xlsx` | Reference lookup: 128 RFM score combinations mapped to 11 segments, with segment descriptions and marketing actions |

---

## ⚙️ Methodology

1. **Data Cleaning** — merged `FactInternetSales` with `DimCustomer` and `DimGeography`; removed nulls and ensured correct data types.
2. **RFM Calculation** (reference date: 2008-08-01, one day after the last order):
   - **Recency** = days since the customer's last order
   - **Frequency** = count of distinct sales orders per customer
   - **Monetary** = total sales amount per customer
3. **Scoring** — each metric scored 1–5 by quintile, combined into a 3-digit RFM code (e.g. `543`).
4. **Segmentation** — each RFM code mapped to one of 11 named segments using the provided `RFM_Table.xlsx` lookup (all 128 combinations matched with zero gaps).
5. **Analysis** — computed customer count, average RFM values, and revenue share per segment.
6. **Recommendations** — paired each segment with a marketing action from the reference table.
7. **Visualization** — built an interactive dashboard (Excel, with a Power BI/Tableau build guide).

---

## 📊 Key Findings

| Segment | % of Customers | % of Revenue |
|---|---|---|
| Champions | 10.3% | **29.9%** |
| At Risk | 15.1% | **28.9%** |
| Loyal | 8.5% | 19.6% |
| Cannot Lose Them | 5.1% | 8.8% |
| Potential Loyalist | 15.5% | 2.4% |
| Hibernating | 13.2% | 1.5% |

**Insight:** Revenue is split almost evenly between two opposite groups — **Champions** (actively buying, high value) and **At Risk** (high historical value, gone quiet). Nearly 30% of total revenue sits with customers who haven't ordered recently, making win-back campaigns as high-priority as loyalty rewards.

---

## 📁 Repository Contents

```
├── AW_RFM_Dashboard.xlsx          # Interactive dashboard (charts + live formulas, zero errors)
├── AW_RFM_Customer_Segments.csv   # 18,484 customers with RFM scores, segments, demographics
├── AW_Transactions.csv            # Cleaned order-line transaction data
├── AW_Segment_Summary_Full.csv    # Segment-level summary, descriptions, marketing actions
├── AW_Power_BI_Dashboard_Guide.md # Step-by-step guide to rebuild the dashboard in Power BI/Tableau
└── README.md
```

---

## 🛠️ Tools Used

- **Python** (pandas) — data cleaning, RFM calculation, segmentation logic
- **Excel** (openpyxl) — dashboard build, charts, live formulas
- **Power BI / Tableau** — recommended for the final interactive dashboard (see build guide)

---

## 🚀 How to Use

1. Open `AW_RFM_Dashboard.xlsx` → **Dashboard** tab for the visual summary.
2. Explore `AW_RFM_Customer_Segments.csv` for the full customer-level RFM dataset.
3. Follow `AW_Power_BI_Dashboard_Guide.md` to import the CSVs into Power BI or Tableau and rebuild the dashboard natively.

---

---

## 👤 Author

Sattwik Sahu

Submitted as part of the **Syntecxhub Internship Program** — Data Analysis Track.

**Connect:** [Syntecxhub](https://www.syntecxhub.com) | `@Syntecxhub`

---
