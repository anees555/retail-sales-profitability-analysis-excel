# Retail Sales, Profitability & Returns Analysis — Excel

## Project Overview

This project analyzes retail transaction data using **Microsoft Excel** to understand sales performance, profitability, customer and product performance, regional trends, returns, discounts, and shipping operations.

The project follows a structured data-analysis workflow, starting with data profiling and cleaning before moving into exploratory analysis, PivotTable analysis, and an interactive Excel dashboard.

The main objective is to turn raw retail transaction data into meaningful business insights that can support better decisions around pricing, discounts, product performance, profitability, returns, and operations.

---

## Business Problem

A retail business needs to understand how its sales are performing and where profitability is being gained or lost.

The analysis aims to answer questions such as:

* Which product categories and sub-categories generate the most sales and profit?
* Which products contribute the most to overall profitability?
* Which regions and customer segments perform best?
* How do discounts affect profitability?
* Which products or categories have high levels of returns?
* How does shipping duration and shipping cost affect profitability?
* Which areas of the business require attention?

---

## Objectives

The project focuses on the following business areas:

1. Sales performance
2. Profitability analysis
3. Product performance
4. Customer segment analysis
5. Regional performance
6. Discount analysis
7. Returns analysis
8. Shipping and operational analysis
9. Business recommendations

---

## Dataset

The project uses a retail Superstore-style dataset containing three related tables.

### Orders

The Orders table contains **8,399 transaction rows** and **23 columns**.

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

### Users

The Users table contains **8 records** mapping regions to available manager information.

---

## Data Structure

An important finding during profiling was that `Order ID` is not unique in the Orders table.

| Metric                      | Value |
| --------------------------- | ----: |
| Transaction rows            | 8,399 |
| Unique Order IDs            | 5,496 |
| Unique Row IDs              | 8,399 |
| Returned Orders             |   572 |
| Missing Product Base Margin |    63 |

Therefore, the Orders table is treated as **transaction/order-line level data**, rather than one row representing one order.

This distinction is important when calculating order-level KPIs such as total orders and return rate.

---

## Data Profiling

The first stage of the project was data profiling.

The following checks were performed:

* Dataset structure and dimensions
* Row ID uniqueness
* Order ID uniqueness
* Missing values
* Numerical data quality
* Negative and zero values
* Discount distribution
* Date validation
* Shipping duration
* Categorical values
* Returns table
* Users table

### Key Findings

#### Missing Values

There were **63 missing Product Base Margin values**, representing approximately **0.75%** of the Orders table.

The missing values were associated with **10 unique products**.

Since no known margin existed for these products elsewhere in the dataset, the planned approach is to use the median Product Base Margin of the corresponding Product Sub-Category.

#### Profit

There were **4,264 transaction rows with negative profit**, representing approximately **50.77% of transaction rows**.

Negative profit was not considered a data error because losses can occur naturally in retail transactions.

Instead, these records will be investigated to understand relationships between profit and factors such as discount, product category, region, shipping cost, and customer segment.

#### Discounts

Most discounts were between **0% and 10%**.

Five unusual discount values were identified:

* 11%
* 16%
* 17%
* 21%
* 25%

These values occurred only once each.

They were retained because there was no documented business rule indicating that discounts above 10% were invalid.

#### Shipping Duration

Shipping duration was calculated as:

```excel
=Ship Date - Order Date
```

Results:

* Minimum: **0 days**
* Median: **2 days**
* Average: approximately **2 days**
* Maximum: **92 days**

Two unusually long shipping records of **84 and 92 days** were identified.

The dates were valid, so these records were retained and will be treated as operational anomalies for further investigation.

#### Dates

The date validation confirmed that no record had a Ship Date earlier than its Order Date.

Order Date range:

**1 January 2009 – 30 December 2012**

Ship Date range:

**2 January 2009 – 30 December 2012**

---

## Data Cleaning

The next stage of the project will create a cleaned version of the dataset while preserving the original raw data.

Planned cleaning activities include:

* Handling missing Product Base Margin values
* Validating returned Order IDs
* Creating Return Status
* Creating Shipping Days
* Checking categorical consistency
* Identifying potential data-quality anomalies
* Preserving valid business outliers

The original dataset will not be modified.

---

## Planned Analysis

After cleaning, the project will analyze:

### Sales Performance

* Total sales
* Sales by year and month
* Sales by region
* Sales by product category
* Sales by customer segment

### Profitability

* Total profit
* Profit margin
* Profit by category and sub-category
* Loss-making products
* Profitability by region
* Relationship between discount and profit

### Product Analysis

* Top-selling products
* Most profitable products
* Loss-making products
* Product category performance
* Product sub-category performance

### Customer Analysis

* Customer segment performance
* Sales by customer segment
* Profit by customer segment
* Top customers

### Returns

* Total returned orders
* Return rate
* Returns by product category
* Returns by region
* Returns by customer segment

### Shipping

* Average shipping duration
* Shipping duration by ship mode
* Shipping cost analysis
* Long-shipping orders
* Relationship between shipping and profitability

---

## Excel Skills Demonstrated

This project is designed to demonstrate practical Microsoft Excel data-analysis skills, including:

* Excel Tables
* Data cleaning
* Data profiling
* Missing-value analysis
* Duplicate detection
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
* Interactive dashboards
* Business-oriented data interpretation

---

## Workbook Structure

The final Excel workbook is planned to contain the following sheets:

```text
Raw_Data
Data_Profiling
Data_Cleaning
Calculated_Data
Exploratory_Analysis
Pivot_Analysis
Dashboard
Business_Insights
```

Each sheet has a specific purpose so that the workflow remains reproducible and easy to understand.

---

## Project Workflow

The project follows this workflow:

```text
Raw Data
   ↓
Data Profiling
   ↓
Data Cleaning
   ↓
Calculated Fields
   ↓
Exploratory Analysis
   ↓
PivotTable Analysis
   ↓
Dashboard
   ↓
Business Insights & Recommendations
```

---

## Current Progress

* [x] Dataset imported into Excel
* [x] Dataset structure reviewed
* [x] Row ID uniqueness checked
* [x] Order ID uniqueness investigated
* [x] Missing values profiled
* [x] Numerical fields profiled
* [x] Profit distribution investigated
* [x] Discount distribution investigated
* [x] Date fields validated
* [x] Shipping duration calculated and profiled
* [x] Categorical fields profiled
* [x] Returns table profiled
* [x] Users table profiled
* [ ] Data cleaning
* [ ] Calculated fields
* [ ] Exploratory analysis
* [ ] PivotTable analysis
* [ ] Dashboard
* [ ] Business insights and recommendations

---

## Key Data Profiling Conclusions

The dataset is generally suitable for analysis.

The main data-quality issue identified is the **63 missing Product Base Margin values**. These will be handled using sub-category-level median imputation.

Other unusual observations, such as negative-profit transactions, high discounts, and very long shipping durations, were retained because they may represent legitimate business conditions rather than data errors.

The next stage is to clean the data while preserving the original dataset and then begin the analytical phase.

---

## Tools

* **Microsoft Excel**
* Excel Tables
* PivotTables
* PivotCharts
* Excel formulas
* Conditional Formatting
* Slicers

---

## Project Status

**Current Stage: Data Profiling — Completed**

**Next Stage: Data Cleaning**
