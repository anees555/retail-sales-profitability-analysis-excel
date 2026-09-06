# Data Profiling

## 1. Overview

Data profiling was performed to understand the structure, quality, completeness, and consistency of the retail dataset before starting data cleaning and analysis.

The profiling process focused on identifying missing values, duplicate records, invalid numerical values, unusual observations, date inconsistencies, categorical values, and the relationship between order-level and transaction-level data.

The dataset contains three tables:

* **Orders** — 8,399 records and 23 columns
* **Returns** — 572 records
* **Users** — 8 records

The `Orders` table contains the main retail transaction data, while `Returns` contains returned order IDs and `Users` provides region-manager information.

---

## 2. Understanding the Data Structure

The first step was to determine the structure and grain of the `Orders` table.

There are 8,399 rows in the Orders table, but only 5,496 unique Order IDs. This means that `Order ID` is not unique and that a single order can contain multiple rows.

The `Row ID` field is unique for all 8,399 records and can therefore be used to identify individual transaction rows.

This distinction is important because counting rows would not give the correct number of orders.

### Key observations

* Total transaction rows: **8,399**
* Unique Order IDs: **5,496**
* Unique Row IDs: **8,399**
* Order ID duplicates: **Expected**
* Row ID duplicates: **None**

Therefore, the dataset was treated as transaction/order-line level data rather than one row per order.

---

## 3. Missing Value Analysis

Missing values were checked across all columns in the Orders table.

The analysis found that missing values were concentrated in the `Product Base Margin` column.

### Missing Product Base Margin

* Missing rows: **63**
* Total rows: **8,399**
* Missing rate: approximately **0.75%**
* Number of affected products: **10**

Further investigation showed that the affected products did not have another known Product Base Margin value elsewhere in the dataset. Therefore, simply looking up the same product's margin would not solve the problem.

The affected products belonged to four sub-categories:

* Storage & Organization
* Chairs & Chairmats
* Tables
* Bookcases

The median Product Base Margin was calculated separately for each sub-category using only records with known margins.

The results were:

| Product Sub-Category   | Median Margin | Known Margin Records |
| ---------------------- | ------------: | -------------------: |
| Storage & Organization |          0.60 |                  525 |
| Chairs & Chairmats     |          0.60 |                  360 |
| Tables                 |          0.69 |                  349 |
| Bookcases              |          0.65 |                  185 |

The missing values will therefore be handled during the data-cleaning stage using the median margin of the corresponding Product Sub-Category.

The original Orders data will not be overwritten.

---

## 4. Numerical Data Profiling

The main numerical fields were examined using minimum, maximum, mean, median, zero-value count, and negative-value count.

| Field               |    Minimum |   Maximum |     Mean | Median | Zero Values | Negative Values |
| ------------------- | ---------: | --------: | -------: | -----: | ----------: | --------------: |
| Order Quantity      |          1 |        50 |    25.57 |     26 |           0 |               0 |
| Sales               |       2.24 | 89,061.05 | 1,775.88 | 449.42 |           0 |               0 |
| Discount            |          0 |      0.25 |   0.0497 |   0.05 |         756 |               0 |
| Profit              | -14,140.70 | 27,220.69 |   181.18 |  -1.50 |           0 |           4,264 |
| Unit Price          |       0.99 |  6,783.02 |    89.35 |  20.99 |           0 |               0 |
| Shipping Cost       |       0.49 |    164.73 |    12.84 |   6.07 |           0 |               0 |
| Product Base Margin |       0.35 |      0.85 |     0.51 |   0.52 |           0 |               0 |

### Findings

`Order Quantity`, `Sales`, `Unit Price`, and `Shipping Cost` did not contain zero or negative values that would immediately indicate invalid records.

`Profit` contained 4,264 negative values. Negative profit is possible in a retail business and therefore was not treated as a data error. Instead, these records will be investigated later to understand the factors associated with losses.

The percentage of transaction rows with negative profit is approximately:

**4,264 / 8,399 = 50.77%**

This figure represents transaction rows, not unique orders, because Order IDs can occur multiple times.

---

## 5. Discount Analysis

The `Discount` column was examined to understand its distribution and identify unusual values.

Most discount values were between **0% and 10%**.

The common discount values were:

* 0%
* 1%
* 2%
* 3%
* 4%
* 5%
* 6%
* 7%
* 8%
* 9%
* 10%

Five additional values occurred only once each:

* 11%
* 16%
* 17%
* 21%
* 25%

These values were considered unusual but were not automatically treated as errors because there was no documented rule stating that discounts above 10% were invalid.

The unusual records were retained for further analysis.

An interesting observation was that all five unusual-discount records were associated with:

* Furniture
* Critical order priority
* Central region

This observation may be useful during the later business analysis, but it was not considered sufficient evidence to classify the records as incorrect.

---

## 6. Date Validation

The `Order Date` and `Ship Date` columns were checked for valid dates and logical relationships.

### Order Date

* Earliest date: **1 January 2009**
* Latest date: **30 December 2012**

### Ship Date

* Earliest date: **2 January 2009**
* Latest date: **30 December 2012**

A date validation check was created using:

```excel
=IF(W2<C2,"Invalid","Valid")
```

where `C2` represents Order Date and `W2` represents Ship Date.

All records were valid because the Ship Date was never earlier than the Order Date.

---

## 7. Shipping Duration Analysis

A `Shipping Days` calculated field was created by subtracting the Order Date from the Ship Date:

```excel
=W2-C2
```

The results were:

* Minimum shipping duration: **0 days**
* Median shipping duration: **2 days**
* Average shipping duration: approximately **2 days**
* Maximum shipping duration: **92 days**

A shipping duration of zero days is valid and represents a same-day shipment.

Two unusually long shipping records were identified:

| Product Sub-Category | Product                             | Order Date | Shipping Days |
| -------------------- | ----------------------------------- | ---------- | ------------: |
| Pens & Art Supplies  | Quartet Alpha® White Chalk, 12/Pack | 4/1/2011   |            92 |
| Envelopes            | #10 Self-Seal White Envelopes       | 3/23/2011  |            84 |

These records were checked and confirmed to have valid date relationships. They were therefore retained.

The long shipping durations will be treated as operational anomalies for later investigation rather than as confirmed data errors.

---

## 8. Categorical Data Profiling

Categorical columns were examined by counting their unique values.

| Categorical Field    | Unique Values |
| -------------------- | ------------: |
| Order Priority       |             5 |
| Ship Mode            |             3 |
| Customer Name        |           795 |
| City                 |         1,421 |
| State                |            48 |
| Region               |             4 |
| Customer Segment     |             4 |
| Product Category     |             3 |
| Product Sub-Category |            17 |
| Product Name         |         1,263 |
| Product Container    |             7 |

The cardinality of these fields was considered reasonable for the dataset.

The main controlled categorical fields were also checked to ensure that their values followed the expected categories.

These fields include:

* Order Priority
* Ship Mode
* Region
* Customer Segment
* Product Category

No obvious categorical inconsistencies were identified during profiling.

---

## 9. Returns Table Profiling

The Returns table contains:

* **572 records**
* **572 unique Order IDs**
* Return status recorded as `Returned`

Because each returned Order ID is unique in the Returns table, the table represents returned orders rather than individual transaction lines.

There are 5,496 unique orders in the Orders table.

An initial order-level return rate can therefore be estimated as:

**572 / 5,496 = 10.41%**

This value will be validated during the data-cleaning stage by checking whether all returned Order IDs exist in the Orders table.

The return rate will only be used as a final business KPI after this validation.

---

## 10. Users Table Profiling

The Users table contains 8 records and provides region-manager information.

The available regions are:

* Central
* East
* South
* West

The table was treated as a region-manager mapping table.

It will be used later to connect regional performance with the available manager information.

No data-cleaning issue was identified from the basic profiling of this table.

---

## 11. Data Quality Summary

The main findings from the profiling process are summarized below.

| Data Quality Area   | Finding                               | Decision                         |
| ------------------- | ------------------------------------- | -------------------------------- |
| Row ID uniqueness   | 8,399 unique IDs                      | Keep                             |
| Order ID uniqueness | 5,496 unique orders across 8,399 rows | Expected                         |
| Missing values      | 63 missing values                     | Handle during cleaning           |
| Product Base Margin | 63 missing across 10 products         | Impute using sub-category median |
| Order Quantity      | 1–50; no zero/negative values         | Keep                             |
| Sales               | No zero/negative values               | Keep                             |
| Discount            | Mostly 0–10%; five unusual values     | Retain and investigate           |
| Profit              | 4,264 negative transaction rows       | Keep; investigate                |
| Unit Price          | No zero/negative values               | Keep                             |
| Shipping Cost       | No zero/negative values               | Keep                             |
| Order Date          | 2009–2012                             | Valid                            |
| Ship Date           | 2009–2012                             | Valid                            |
| Shipping Days       | 0–92 days; median 2 days              | Keep; investigate outliers       |
| Categorical fields  | Reasonable cardinality                | Keep                             |
| Returns             | 572 unique returned orders            | Validate against Orders          |
| Users               | 8 region-manager records              | Keep                             |

---

## 12. Conclusion

The data profiling stage provided an initial assessment of the dataset's quality and structure before cleaning and analysis.

The dataset is generally usable, with the main data-quality issue being the **63 missing Product Base Margin values**. Several unusual observations were also identified, including very long shipping durations, unusual discount values, and a large number of negative-profit transaction rows.

These observations were not automatically removed because unusual values are not necessarily incorrect values. Instead, they will be retained and investigated during the analysis stage.

The main cleaning actions identified from profiling are:

1. Impute the 63 missing Product Base Margin values using the median margin of their respective Product Sub-Categories.
2. Validate all returned Order IDs against the Orders table.
3. Create calculated fields required for analysis, including Shipping Days and Return Status.
4. Retain valid but unusual observations for business analysis rather than deleting them.
5. Preserve the original raw dataset and perform cleaning in a separate dataset.

With profiling complete, the project can now move to the **Data Cleaning** stage.
