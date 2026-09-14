Sales & Revenue Performance Dashboard
Power BI · Star Schema · 12 DAX Measures · 4 Pages

Overview
A four-page executive sales dashboard built in Power BI Desktop, covering revenue performance, product profitability, customer segmentation, and sales rep productivity.
The underlying model is a classic star schema — one fact table joined to four dimension tables — designed to handle realistic business scenarios: multi-year comparisons, segment drill-downs, and margin analysis by product sub-category.
This project is part of my freelance BI portfolio. A companion build guide and the source CSV files are included in this repository so the dashboard can be rebuilt from scratch.
---
What the dashboard covers
Page	Purpose	Key visuals
Executive Overview	Top-line revenue, profit, and growth snapshot	KPI cards, revenue trend, YoY comparison, regional and category breakdown
Product Performance	Margin and revenue by sub-category	Ranked bar charts, conditional formatting on margin %, profit distribution
Customer & Segment	Enterprise vs Mid-Market vs SMB analysis	Segment cards, region breakdown, top 10 customers table
Sales Rep Performance	Individual rep leaderboard and detail	Ranked bar, full rep table with margin conditional formatting
---
Data model
```
DimDate ─────┐
DimProduct ──┤
              ├── FactSales
DimCustomer ─┤
DimSalesRep ─┘
```
All relationships are many-to-one from `FactSales` to each dimension. `DimDate` is marked as a date table, enabling time intelligence functions across the model.
Source data is five clean CSV files (~5,000 sales transactions). No transforms required in Power Query — column types only.
---
DAX measures
12 measures, all stored in a dedicated `\_Measures` table:
Measure	Purpose
`Total Revenue`	`SUM(FactSales\[Revenue])`
`Total Profit`	`SUM(FactSales\[Profit])`
`Total COGS`	`SUM(FactSales\[COGS])`
`Profit Margin %`	`DIVIDE(\[Total Profit], \[Total Revenue], 0)`
`Total Orders`	`DISTINCTCOUNT(FactSales\[OrderID])`
`Avg Order Value`	`DIVIDE(\[Total Revenue], \[Total Orders], 0)`
`Total Quantity`	`SUM(FactSales\[Quantity])`
`Revenue PY`	`CALCULATE` + `SAMEPERIODLASTYEAR` — prior-year revenue
`Revenue YoY %`	Year-over-year growth rate
`Revenue YTD`	`TOTALYTD` accumulation
`Profit YTD`	`TOTALYTD` accumulation
`Avg Discount`	`AVERAGE(FactSales\[Discount])`
Time intelligence measures (`PY`, `YoY %`, `YTD`) rely on the marked `DimDate` table and respect any active date filter from slicers.
---
Pages in detail
Page 1 — Executive Overview
Four KPI cards across the top row: Total Revenue, Gross Profit, Total Orders, and YoY Growth %. Each card shows the callout value with an accent bar in the primary colour (#0D9488 teal).
Below the cards: a monthly revenue and profit trend (line + column combo chart) on the left, a year-over-year revenue comparison bar chart on the right. Bottom row: regional revenue breakdown (horizontal bar) and a donut chart splitting revenue by product category.
Canvas background is #F8FAFC — off-white, not harsh white, which gives visuals room to breathe.
Page 2 — Product Performance
Horizontal bar chart ranking sub-categories by revenue and profit side by side. Below it: profit margin % by sub-category with conditional formatting — coral (#EF6461) at the low end, teal (#0D9488) at the high end, so margin risk is immediately visible.
Page 3 — Customer & Segment
Segment-level KPI summary (Enterprise / Mid-Market / SMB), regional revenue breakdown with both revenue and profit, and a full-width top 10 customers table. The table uses alternating rows, a Top N filter, and data bars on the revenue column.
Page 4 — Sales Rep Performance
Horizontal bar chart of the top 10 reps by revenue (sortable). Below it, a detailed rep table with six metrics: Revenue, Orders, Profit, Avg Order Value, and Margin %. Profit Margin % is conditionally formatted green-to-red so underperformers are visible at a glance.
---
Slicers and interactivity
Three slicers on Page 1, synced across all pages via the Sync Slicers pane:
Year — formatted as buttons for fast switching
Region — dropdown
Category — dropdown
A page navigator is included on each page (Insert → Buttons → Page navigator) so users can move between pages without using the tab row at the bottom.
---
Colour palette
Role	Hex	Used for
Primary / Teal	`#0D9488`	Revenue bars, KPI accents, positive indicators
Navy	`#1B3A5C`	Prior-year series, headers
Amber	`#F59E0B`	Profit lines, highlights
Coral	`#EF6461`	Low-margin indicators, negative variance
Canvas	`#F8FAFC`	Page background
Card background	`#FFFFFF`	Visual backgrounds
Border	`#E2E8F0`	Visual borders (8px radius)
Text primary	`#0F172A`	Titles and labels
Text muted	`#94A3B8`	Axis labels, subtitles
---
Files in this repository
```
├── data/
│   ├── FactSales.csv
│   ├── DimDate.csv
│   ├── DimProduct.csv
│   ├── DimCustomer.csv
│   └── DimSalesRep.csv
├── Sales\_Revenue\_Dashboard.pbix
└── README.md  ← this file
```
---
How to use
Clone or download this repository
Open `Sales\_Revenue\_Dashboard.pbix` in Power BI Desktop (free download from Microsoft)
If prompted to refresh data, click Refresh — the CSVs are in the `data/` folder
No Power BI Pro licence or cloud workspace is needed to view and interact with the file locally.
---
About
Built by Cristian Muresan — Data & Business Analyst with 5+ years of experience across B2B SaaS analytics, supply chain reporting, and ERP transformation (SAP ECC, Salesforce, Power BI, Tableau).
This dashboard is part of a freelance analytics portfolio demonstrating end-to-end BI delivery: data modelling, DAX, visual design, and executive-level storytelling.
Available for freelance engagements on Upwork — Power BI, Excel, and data analysis projects.
---
Reach out via Upwork or GitHub Issues if you have questions about the model or want to adapt this for your own data.
