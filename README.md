# Retail Sales, Profitability & Returns Analysis — Excel

## Project Overview

This project analyzes retail transaction data using **Microsoft Excel** to evaluate sales performance, profitability, product performance, customer segments, regional trends, discounts, returns, shipping operations, and time-based performance.

The project follows a structured data-analysis workflow:

**Data Profiling → Data Cleaning → Feature Engineering → Exploratory Data Analysis → PivotTable Analysis → Interactive Dashboard → Business Insights**

The main objective is to transform raw retail transaction data into meaningful business insights that can support decisions related to pricing, discounts, product performance, profitability, returns, and operations.

---

## Business Problem

A retail business needs to understand how sales and profitability are performing across products, customers, regions, and time.

This project addresses questions such as:

* Which product categories and sub-categories generate the most sales and profit?
* Which products and sub-categories have strong or weak profitability?
* How does discounting relate to profitability?
* How does performance vary across regions and customer segments?
* What proportion of orders are returned?
* Which categories have higher returned sales?
* How does shipping duration vary across transactions?
* Which areas require further business investigation?

---

## Objectives

The analysis focuses on:

1. Sales performance
2. Profitability analysis
3. Product and sub-category performance
4. Customer segment analysis
5. Regional performance
6. Discount analysis
7. Returns analysis
8. Shipping and operational analysis
9. Time-based performance analysis
10. Business insights and recommendations

---

## Dataset

The project uses a Superstore-style retail dataset containing three related tables.

### Orders

The Orders table contains:

* **8,399 transaction rows**
* **23 original columns**
* **5,496 unique Order IDs**
* **8,399 unique Row IDs**

Important fields include:

* Row ID
* Order ID
* Order Date
* Order Priority
* Order Quantity
* Sales
* Discount
* Ship Mode
* Profit
* Unit Price
* Shipping Cost
* Customer Name
* City
* State
* Region
* Customer Segment
* Product Category
* Product Sub-Category
* Product Name
* Product Container
* Product Base Margin
* Ship Date

### Returns

The Returns table contains:

* **572 records**
* **572 unique Order IDs**
* Return status

All return Order IDs were validated against the Orders table.

### Users

The Users table contains **8 records** mapping regions to manager information.

---

## Data Structure

An important finding during data profiling was that `Order ID` is not unique in the Orders table.

| Metric                      | Value |
| --------------------------- | ----: |
| Transaction rows            | 8,399 |
| Unique Order IDs            | 5,496 |
| Unique Row IDs              | 8,399 |
| Returned Orders             |   572 |
| Missing Product Base Margin |    63 |

Therefore, the Orders table is treated as **transaction/order-line level data**, rather than one row representing one complete order.

This distinction is important when calculating order-level KPIs such as unique orders and return rate.

---

## Data Preparation

The original raw dataset was preserved and a separate `Orders_Cleaned` table was created.

Data preparation included:

* Missing-value analysis and treatment
* Product Base Margin imputation
* Return Order ID validation
* Return Status creation
* Shipping Days calculation
* Date validation
* Categorical consistency checks
* Numerical data validation
* Identification of operational anomalies
* Creation of analytical and time-based fields

Missing Product Base Margin values were handled using **sub-category-level median values**, while valid business observations such as negative-profit transactions and unusually long shipping durations were retained.

Detailed methodology is documented in:

* [`data_profiling.md`](data_profiling.md)
* [`data_cleaning.md`](data_cleaning.md)

---

## Exploratory Data Analysis

The EDA phase examined:

* Sales and profit distributions
* Product and category performance
* Regional performance
* Customer segment performance
* Discount and profitability relationships
* Returns
* Shipping duration
* Annual and monthly performance
* Sub-category profitability

Detailed findings and analysis are documented in:

* [`eda.md`](eda.md)

---

# Key Results

## Overall Performance

| KPI               |         Value |
| ----------------- | ------------: |
| Total Sales       | 14,915,600.82 |
| Total Profit      |  1,521,767.96 |
| Profit Margin     |        10.20% |
| Order Quantity    |       214,777 |
| Unique Orders     |         5,496 |
| Returned Orders   |           572 |
| Order Return Rate |        10.41% |

The order return rate is calculated using unique orders:

**572 returned orders / 5,496 unique orders = 10.41%**

---

## Category Performance

| Category        |             Sales |           Profit | Profit Margin |
| --------------- | ----------------: | ---------------: | ------------: |
| Furniture       |      5,178,590.54 |       117,432.99 |         2.27% |
| Office Supplies |      3,752,762.10 |       518,021.46 |        13.80% |
| Technology      |      5,984,248.18 |       886,313.52 |        14.81% |
| **Total**       | **14,915,600.82** | **1,521,767.96** |    **10.20%** |

Technology generated the highest sales and profit among the three product categories.

Furniture generated substantial sales but had a considerably lower overall profit margin.

---

## Sub-Category Profitability

Profitability was analyzed across all 17 product sub-categories.

Selected results include:

| Sub-Category                   | Profit Margin |
| ------------------------------ | ------------: |
| Labels                         |           35% |
| Binders and Binder Accessories |           30% |
| Envelopes                      |           28% |
| Telephones and Communication   |           17% |
| Copiers and Fax                |           15% |
| Office Machines                |           14% |
| Bookcases                      |           -4% |
| Tables                         |           -5% |
| Scissors, Rulers and Trimmers  |          -10% |

The analysis shows substantial variation in profitability across sub-categories, including several sub-categories with negative overall profit.

---

## Regional Performance

| Region  |        Sales |     Profit | Profit Margin |
| ------- | -----------: | ---------: | ------------: |
| Central | 4,699,167.25 | 481,891.24 |        10.25% |
| East    | 3,416,466.47 | 317,852.04 |         9.30% |
| South   | 3,150,219.36 | 422,507.11 |        13.41% |
| West    | 3,649,747.75 | 299,517.56 |         8.21% |

Regional performance varies across both sales and profitability.

Further category-level analysis was used to investigate the composition of these regional differences.

---

## Customer Segment Performance

| Customer Segment |        Sales |     Profit | Profit Margin |
| ---------------- | -----------: | ---------: | ------------: |
| Consumer         | 3,063,611.08 | 287,959.98 |         9.40% |
| Corporate        | 5,498,904.88 | 599,745.92 |        10.91% |
| Home Office      | 3,564,763.88 | 318,354.10 |         8.93% |
| Small Business   | 2,788,320.99 | 315,707.96 |        11.32% |

The analysis identifies differences in sales contribution and profitability across customer segments.

---

## Discount Analysis

Profitability was analyzed across different discount levels.

Selected results:

| Discount |        Sales |     Profit | Margin |
| -------: | -----------: | ---------: | -----: |
|       0% | 1,493,748.22 | 188,188.78 |    13% |
|       3% | 1,335,274.71 | 222,349.04 |    17% |
|       6% | 1,303,602.89 |  84,837.63 |     7% |
|      10% | 1,144,692.31 |  74,445.70 |     7% |

The analysis shows an association between discount levels and profitability.

This is a descriptive analysis and does not establish that discounts directly cause changes in profit. Product mix, region, customer segment, and other transaction characteristics may also influence profitability.

---

## Returns Analysis

Two different return metrics were used because the dataset contains transaction-level records and repeated Order IDs.

### Order-Level Return Rate

**10.41%**

Calculated as:

**572 returned orders / 5,496 unique orders**

### Returned Sales Share

Sales associated with returned orders represented approximately:

**11.09% of total sales**

Returned sales by category:

| Category        | Returned Sales % |
| --------------- | ---------------: |
| Furniture       |           11.20% |
| Office Supplies |           12.51% |
| Technology      |           10.12% |
| **Overall**     |       **11.09%** |

These two return metrics use different denominators and should not be treated as interchangeable.

---

## Shipping Analysis

Shipping duration was calculated using:

```excel
=Ship Date - Order Date
```

Key observations:

* Minimum shipping duration: **0 days**
* Median shipping duration: **2 days**
* Average shipping duration: approximately **2 days**
* Maximum shipping duration: **92 days**

Two unusually long shipping records of **84 and 92 days** were identified.

The underlying dates were valid, so these records were retained as operational anomalies rather than treated as data errors.

---

## Time Analysis

Annual and monthly performance was analyzed across the 2009–2012 period.

| Year |        Sales |     Profit | Profit Margin |
| ---- | -----------: | ---------: | ------------: |
| 2009 | 4,209,139.46 | 434,538.79 |        10.32% |
| 2010 | 3,549,680.80 | 363,871.38 |        10.25% |
| 2011 | 3,436,816.70 | 381,455.99 |        11.10% |
| 2012 | 3,719,963.86 | 341,901.81 |         9.19% |

A monthly analysis covering the full 48-month period was also created to examine changes in sales and profit over time.

---

# Interactive Dashboard

The final Excel dashboard combines KPI cards, PivotCharts, and interactive slicers.

### Dashboard KPIs

* Total Sales
* Total Profit
* Profit Margin
* Unique Orders
* Return Rate

### Dashboard Charts

* Monthly Sales & Profit Trend
* Sales & Profit by Category
* Profit Margin by Subcategory
* Regional Sales & Profit Margin
* Sales & Profit Margin by Customer Segment
* Returned Sales % by Category

### Interactive Slicers

* Order Year
* Region
* Product Category
* Customer Segment

The slicers are connected to the dashboard PivotTables and allow users to interactively filter the analysis.

---

## Dashboard Preview

![Retail Sales Analysis Dashboard](screenshots/dashboard.png)

---

# Excel Techniques Demonstrated

This project demonstrates practical Excel data-analysis and business intelligence skills, including:

* Excel Tables
* Data profiling
* Data cleaning
* Missing-value analysis
* Median-based imputation
* Duplicate and uniqueness analysis
* Conditional logic
* `IF`
* `COUNT`
* `COUNTA`
* `COUNTIF`
* `COUNTIFS`
* `SUMIFS`
* `AVERAGEIFS`
* `MEDIAN`
* `FILTER`
* `UNIQUE`
* `XLOOKUP`
* Date calculations
* Descriptive statistics
* Conditional formatting
* PivotTables
* PivotCharts
* Slicers
* Power Pivot
* DAX measures
* Dashboard development
* Business-oriented data interpretation

---

# Workbook Structure

The final workbook contains dedicated sheets for data preparation, analysis, PivotTables, and dashboard development.

```text
Raw_Data
Data_Profiling
Data_Cleaning
Calculated_Data
Exploratory_Analysis
Pivot_Category
Pivot_SubCategory
Pivot_Discount
Pivot_Category_Discount
Pivot_Returns
Pivot_Segment
Pivot_Region
Pivot_Region_Category
Pivot_Time
Dashboard_Pivots
Dashboard
Business_Insights
Orders_Cleaned
Margin_Reference
Returns
Users
```

---

# Project Documentation

Detailed project documentation is available in separate Markdown files:

| File                                     | Description                                                                      |
| ---------------------------------------- | -------------------------------------------------------------------------------- |
| [`data_profiling.md`](data_profiling.md) | Dataset structure, quality checks, profiling results, and anomalies              |
| [`data_cleaning.md`](data_cleaning.md)   | Cleaning methodology, missing-value treatment, validation, and calculated fields |
| [`eda.md`](eda.md)                       | Exploratory analysis, PivotTable findings, and business observations             |

---

# Project Workflow

```text
Raw Data
   ↓
Data Profiling
   ↓
Data Cleaning
   ↓
Calculated Fields
   ↓
Exploratory Data Analysis
   ↓
PivotTable Analysis
   ↓
PivotCharts
   ↓
Interactive Dashboard
   ↓
Business Insights
```

---

# Repository Structure

```text
retail-sales-profitability-analysis-excel/
│
├── README.md
│
├── retail_sales_analysis_excel.xlsx
│
├── data_profiling.md
├── data_cleaning.md
├── eda.md
│
└── screenshots/
    └── dashboard.png
```

If the workbook or screenshots are stored in separate folders, update the paths accordingly.

---

# Tools

* **Microsoft Excel**
* **Power Pivot**
* **DAX**
* Excel Tables
* PivotTables
* PivotCharts
* Slicers
* Conditional Formatting

---

# Project Status

**Completed**

The project includes:

* Data profiling
* Data cleaning
* Missing-value treatment
* Return validation
* Feature engineering
* Exploratory data analysis
* PivotTable analysis
* PivotCharts
* Interactive dashboard
* Interactive slicers
* Business insights
* Project documentation

---

## Author

**Anish Dahal**

Computer Engineering Graduate | Data Analytics & Data Science

[GitHub](https://github.com/anees555) · [LinkedIn](https://www.linkedin.com/in/aneesh-dahal-98b171338/)
