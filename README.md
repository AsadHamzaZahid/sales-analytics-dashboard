# Sales Analytics Dashboard - Power BI

An interactive sales analytics dashboard built in Power BI Desktop as a self-directed
learning project, covering data modeling, DAX, and interactive report design.

## Overview

The dashboard analyzes 300+ sales transactions across regions, product categories,
and customer types, surfacing revenue, margin, and order trends through a
multi-page, cross-filterable report.

**Note:** the underlying dataset is synthetic practice data, not real business
data - generated to simulate a realistic sales scenario for building and
practicing Power BI skills end-to-end (data modeling, DAX, dashboarding).

## Dashboard Preview

**Report Pages**

![Page 1 - Overview](<PAGE 1.png>)
![Page 2 - Drill-Through](<PAGE 2.png>)
![Page 3 - Revenue vs Margin Toggle](<PAGE 3.png>)

**Data Model & DAX**

![Model Relationship View](<Model relationship view.png>)
![DAX Formula](<Dax Formula.png>)

**Interactivity**

![Drill-Through in Action](<Drill through.png>)
![Slicer Filtering](<Slicer.png>)

**Additional Visuals**

![Data Visual 1](<Data visuals 1.png>)
![Data Visual 2](<Data visuals 2.png>)
![Data Visual 3](<Data visuals 3.png>)

## Data Model

Built as a star schema with one fact table and three dimension tables:

- **Sales** (fact) - OrderID, OrderDate, ProductID, CustomerID, Region,
  Salesperson, UnitsSold, UnitPrice, Discount
- **Products** (dimension) - ProductID, ProductName, Category, UnitCost
- **Customers** (dimension) - CustomerID, CustomerName, CustomerType, City, Country
- **DateTable** (dimension) - generated calendar table for time intelligence

Relationships: Sales connects to each dimension table on a one-to-many basis
(Sales as the "many" side), with DateTable marked as the official Date Table.

## Data Preparation (Power Query)

- Removed duplicate rows from Sales
- Cleaned inconsistent casing/spacing in Region and CustomerType
- Replaced blank Discount values with 0
- Added an Order Size column (Small / Medium / Large based on units sold)
- Enforced correct data types across all key columns

## Key DAX Measures

- `Total Revenue`, `Total Orders`, `Avg Order Value`
- `Margin` - revenue less cost, at the transaction level
- `Wholesale Revenue` - revenue filtered to Wholesale customers
- `YTD Revenue` - year-to-date running total
- `Revenue % of Total` - each category's share of company-wide revenue,
  correctly recalculated per drill-through context
- `Prior Month Revenue` / `MoM % Change` - month-over-month time intelligence
- `Top 5 Customer Revenue` / `Top 5 Customer % of Total` - customer concentration

## Report Pages

**Page 1 - Overview**
KPI cards (Total Revenue, Average Order Value), revenue by month, revenue by
category, revenue by customer type, revenue by country, and region/customer-type
slicers.

**Page 2 - Category Drill-Through**
Right-click any category on Page 1's chart to drill through into a page showing
that category's top customers, top products, and its % of total company revenue.

**Page 3 - Revenue / Margin Toggle**
Bookmark-driven buttons ("View Revenue" / "View Margin") that switch the same
visual between revenue and margin views, plus a back-navigation button.

## Excel Analysis

Alongside the Power BI build, the same underlying sales data was analyzed in
Excel to practice formula-based and pivot-based analysis:

- Lookup functions (VLOOKUP, INDEX-MATCH) to pull product and category details
- Conditional aggregation (SUMIFS, COUNTIFS, AVERAGEIF) for region-, category-,
  and salesperson-level breakdowns
- Nested IF / AND logic for order classification (Order Size, High Value flags)
- Pivot tables and pivot charts for Region x Category revenue breakdowns
- Conditional formatting (highlight rules, color scales, icon sets) to surface
  outliers directly in the raw data
- Data validation (dropdown lists) and a duplicate-check audit for data integrity

## Data Quality Note

While validating the Margin measure, I noticed company-wide margin was coming
out negative. Investigating further, I traced it to the practice dataset itself -
product unit cost had been set almost identical to the average selling price,
so any discount pushed margin below zero. It wasn't a DAX or modeling error; it
was a data generation issue in the source file. I regenerated the Products table
with realistic cost-to-price ratios (~35–55% margin) and refreshed the model to
confirm the fix. Flagging it here since it reflects the kind of validation step
I'd apply to any data source, not just this one.

## Tools Used

Power BI Desktop · Power Query (M) · DAX · Excel (source data)

## Files in this repo

- `dataset.pbix` - the full Power BI report
- `powerbi_practice_data (1).xlsx` - source data (Sales, Products, Customers)
- `excel_practice_.xlsx` - Excel workbook with formulas, pivot tables, and
  charts built on the same sales dataset
- `PAGE 1.png`, `PAGE 2.png`, `PAGE 3.png` - full-page screenshots of each
  report page, since `.pbix` files don't render in GitHub's file preview
- `Model relationship view.png` - the star schema and table relationships
- `Dax Formula.png` - a sample DAX measure
- `Drill through.png`, `Slicer.png` - interactivity in action
- `Data visuals 1-3.png` - individual chart/visual close-ups
