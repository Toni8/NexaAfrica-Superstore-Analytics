# NexaAfrica Superstore Analytics

**Author:** Sihle Kalolo
**Stage:** Week 1 — Data Cleaning and Preparation
**Tools:** Microsoft Excel and Power Query

## Project objective

Prepare reliable retail sales data for analysing revenue, profitability, products, customers, regions, and trends over time.

This submission documents the cleaning process, preserves the original dataset, and reconciles changes between raw and cleaned data.

## Dataset

Source: [Superstore Dataset on Kaggle — vivek468](https://www.kaggle.com/datasets/vivek468/superstore-dataset-final/data)

Original file: `Sample - Superstore.csv`

Each row represents a recorded product line within an order. One order can contain multiple lines.

| Measure             | Result                          |
| ------------------- | ------------------------------- |
| Raw dataset         | 9,994 rows; 21 columns          |
| Cleaned dataset     | 9,993 rows; 22 columns          |
| Order date coverage | 3 January 2014–30 December 2017 |
| Ship date coverage  | 7 January 2014–5 January 2018   |
| Added field         | Days_to_Ship                    |

## Files

| Folder            | Contents                                                                                                              |
| ----------------- | --------------------------------------------------------------------------------------------------------------------- |
| `01_Raw_Data`     | Original CSV                                                                                                          |
| `02_Cleaned_Data` | `Superstore_Cleaned.xlsx`: standalone cleaned values                                                                  |
| `03_Workbook`     | `NexaAfrica_Week1_Data_Cleaning.xlsx`: queries, cleaning log, data dictionary, reconciliation, and supporting reviews |

## Cleaning and validation

* Preserved raw values in a separate query and worksheet.
* Converted Order Date and Ship Date using the English (United States) locale.
* Converted Sales, Discount, and Profit to decimal numbers.
* Verified integer values before setting Row ID and Quantity to Whole Number.
* Preserved Postal Code as Text, including leading zeros such as `05408`.
* Checked all original columns for errors, missing values, and empty strings.
* Checked text fields for whitespace-only values and leading or trailing whitespace.
* Reviewed category, sub-category, region, segment, and shipping-service labels.
* Checked Row ID uniqueness and matching business records.
* Validated numerical ranges and chronological consistency.
* Reviewed the ten largest sales and all negative-profit records.
* Reconciled Sales, Quantity, and Profit totals after cleaning.

Detailed decisions and evidence are recorded in `Cleaning_Log` entries CL001–CL017.

## Duplicate-handling assumption

Rows 3406 and 3407 matched across all 20 business fields, excluding Row ID.

For this project, one record was retained per exact business-field match. Row 3406 was retained and Row 3407 was removed from the cleaned query.

This is an assumption: the dataset alone cannot rule out two separately recorded identical order lines. Both original records remain available in the raw data.

## Validation results

* No errors, empty values, or empty strings were detected in the original 21 columns during full-dataset profiling.
* No whitespace-only values or leading/trailing whitespace were detected by the checks performed.
* Days_to_Ship ranged from 0 to 7 calendar days, with no missing results or shipment dates preceding order dates.
* Sales ranged from 0.444 to 22,638.48.
* Quantity ranged from 1 to 14.
* Discount ranged from 0 to 0.8.
* The cleaned dataset contained 1,870 negative-profit lines. These were retained because negative profit alone does not establish a data error.
* The duplicate-check query returned no matching records after removal.

## Reconciliation

Differences are calculated as **raw total minus cleaned total**.

| Metric   | Difference | Explanation                           |
| -------- | ---------: | ------------------------------------- |
| Sales    |   281.3720 | Sales recorded on removed Row ID 3407 |
| Quantity |          2 | Units recorded on removed Row ID 3407 |
| Profit   |   −12.0588 | Loss recorded on removed Row ID 3407  |

All three unexplained differences were zero. Monetary residuals were checked to six decimal places.

Removing a loss-making record increased the cleaned Profit total. The reconciliation verifies the numerical effect of the removal; it does not independently validate the duplicate assumption.

## Opening the workbooks

1. Download the Excel files and open them in Excel Desktop.
2. Use `Superstore_Cleaned.xlsx` for the standalone cleaned dataset.
3. Use the main workbook to inspect the loaded data, documentation, validation formulas, and Power Query steps.

The main workbook's CSV connection points to the author's local file location. Review its saved worksheet values without refreshing. To reproduce the queries, download the original CSV and update the Source step in `Superstore_Raw` to your local file path before refreshing.

## Limitations

* Source records were not independently verified against transaction documents.
* Currency was not confirmed from the source information accessed, so no currency symbol is assumed.
* Duplicate removal relies on the explicitly documented project assumption.
* Extreme-value reviews were contextual, not formal statistical outlier tests.
* The observed association between large discounts and some losses does not establish causation.

## Next stage

Week 2 will explore sales patterns, profitability, customer and product performance, and business insights using the prepared dataset.
