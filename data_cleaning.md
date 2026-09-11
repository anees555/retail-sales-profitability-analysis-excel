# Data Cleaning

## 1. Overview

The data cleaning stage was performed after completing the initial data profiling.

The main objective was to prepare a reliable dataset for analysis while preserving the original data. Only issues identified during profiling were addressed. Unusual values were not removed unless there was sufficient evidence that they were data errors.

The cleaned data was created in a separate `Orders_Cleaned` sheet, while the original `Orders` data was preserved.

---

## 2. Cleaning Approach

The following principles were followed during the cleaning process:

* The original raw data was not modified.
* Confirmed data-quality issues were addressed.
* Valid business observations were retained.
* No transaction rows were unnecessarily deleted.
* Cleaning decisions were based on the results of data profiling.
* Changes were made in a separate cleaned dataset.

---

## 3. Product Base Margin

### Problem

During data profiling, 63 records were found with missing `Product Base Margin` values.

The missing values belonged to 10 unique products.

Further investigation showed that these products did not have another known margin value that could be used directly.

### Cleaning Method

The missing margins were filled using the **median Product Base Margin of the corresponding Product Sub-Category**.

A separate `Margin_Reference` table was created:

| Product Sub-Category   | Median Margin |
| ---------------------- | ------------: |
| Storage & Organization |          0.60 |
| Chairs & Chairmats     |          0.60 |
| Tables                 |          0.69 |
| Bookcases              |          0.65 |

A new column called `Product Base Margin Cleaned` was added to `Orders_Cleaned`.

The original `Product Base Margin` column was preserved.

The following logic was used:

```excel
=IF(V2<>"",V2,XLOOKUP(S2,Margin_Reference!$A$2:$A$5,Margin_Reference!$B$2:$B$5))
```

The formula keeps the original margin when it exists and uses the appropriate sub-category median when the original value is missing.

### Validation

Before cleaning:

* Missing Product Base Margin: **63**

After cleaning:

* Missing Product Base Margin Cleaned: **0**

No transaction rows were removed.

---

## 4. Returns Validation

The Returns table contains 572 returned Order IDs.

Since the Orders table contains 5,496 unique Order IDs, each returned Order ID was checked against the Orders table.

An `Order Validation` column was created in the Returns table using `XLOOKUP`.

### Result

| Validation                 | Records |
| -------------------------- | ------: |
| Returned Order IDs checked |     572 |
| Found in Orders            |     572 |
| Not Found                  |       0 |

Therefore, all returned Order IDs have a corresponding order in the Orders table.

---

## 5. Return Status

After validating the Returns table, a `Return Status` column was added to `Orders_Cleaned`.

The status was assigned based on whether the Order ID exists in the Returns table.

The logic was:

```text
Order ID exists in Returns
        ↓
     Returned

Order ID does not exist
        ↓
   Not Returned
```

The resulting transaction-level counts were:

| Return Status | Transaction Rows |
| ------------- | ---------------: |
| Returned      |              872 |
| Not Returned  |            7,527 |
| Total         |            8,399 |

The 872 returned transaction rows should not be interpreted as 872 returned orders. There are only 572 unique returned orders because one order can contain multiple transaction rows.

The `Return Status` column was checked for missing values, and no blanks were found.

---

## 6. Categorical Consistency

The major categorical fields were checked for unexpected values and obvious spelling or capitalization inconsistencies.

The following fields were reviewed:

* Region
* Order Priority
* Ship Mode
* Customer Segment
* Product Category
* Product Sub-Category
* Product Container

The expected categories were present, and no obvious inconsistencies such as different capitalization or duplicate-looking categories were identified.

No categorical values were changed.

---

## 7. Date Validation

The relationship between `Order Date` and `Ship Date` was validated.

The following rule was applied:

```text
Ship Date >= Order Date
```

All records passed the validation.

Therefore, no date values were changed or removed.

A `Shipping Days` field was also created:

```excel
=Ship Date - Order Date
```

The resulting shipping duration ranged from **0 to 92 days**.

The unusually long shipping durations were retained because the underlying dates were valid.

---

## 8. Unusual Values

Several unusual values were identified during profiling:

* 4,264 transaction rows with negative profit
* Five unusually high discount values
* Shipping durations of 84 and 92 days

These values were not removed.

The reason is that unusual business outcomes are not automatically data errors. For example, negative profit can result from high discounts or shipping costs, while long shipping times can represent genuine operational delays.

These observations will be investigated during the analysis stage.

---

## 9. Data Integrity

The cleaning process did not remove any transaction records.

The Orders dataset remains:

**8,399 transaction rows**

The cleaned dataset retains the original structure while adding analysis-ready fields such as:

* `Product Base Margin Cleaned`
* `Shipping Days`
* `Return Status`

The original data remains available for comparison and auditing.

---

## 10. Cleaning Summary

| Cleaning Task                     | Result              |
| --------------------------------- | ------------------- |
| Missing Product Base Margin       | 63 values handled   |
| Product Base Margin method        | Sub-category median |
| Remaining missing cleaned margins | 0                   |
| Returns validated                 | 572                 |
| Invalid return references         | 0                   |
| Return Status added               | Yes                 |
| Blank Return Status values        | 0                   |
| Categorical consistency           | Passed              |
| Order/Ship Date validation        | Passed              |
| Transaction rows removed          | 0                   |
| Final Orders rows                 | 8,399               |

---

## 11. Conclusion

The data cleaning stage is complete.

The dataset is now prepared for the next stage of the project. Missing Product Base Margin values have been handled, return records have been validated, return status has been added, and the main categorical and date-related consistency checks have been completed.

The cleaned dataset preserves the original transaction records and keeps unusual but potentially valid business observations for further analysis.

The next stage is **Calculated Columns and Exploratory Analysis**, where additional business metrics will be created and used to investigate sales, profitability, returns, customers, products, regions, discounts, and shipping performance.
