# NexaAfrica Superstore Analytics

**Author:** Sihle Kalolo  
**Stage:** Week 1 — Data Cleaning and Preparation  
**Tools:** Microsoft Excel and Power Query  
**Status:** Week 1 deliverables completed and validated

## Project objective

Prepare reliable retail sales data for analysing revenue, profitability, products, customers, regions, and trends over time.

The business problem is to understand how sales and profitability vary across time, products, customers, and regions, and identify areas that warrant further investigation.

This submission preserves the original dataset, documents cleaning decisions, reconciles changes between raw and cleaned data, and provides calculated fields for analysis.

## Dataset

Source: [Superstore Dataset on Kaggle — vivek468](https://www.kaggle.com/datasets/vivek468/superstore-dataset-final/data)

Original file: `Sample - Superstore.csv`

Each row represents a recorded product line within an order. One order can contain multiple lines, so order counts use distinct Order IDs.

| Measure | Result |
| --- | --- |
| Raw dataset | 9,994 rows; 21 columns |
| Cleaned dataset | 9,993 rows; 25 columns |
| Distinct orders | 5,009 |
| Order date coverage | 3 January 2014–30 December 2017 |
| Ship date coverage | 7 January 2014–5 January 2018 |
| Added dataset columns | Days_to_Ship, Year, Month, Year-Month |
| Summary metrics | Profit Margin, Average Order Value |

Profit Margin and Average Order Value are calculated on the `Calculated_Fields` worksheet in the main workbook. They are not additional columns in the standalone cleaned dataset.

## Files

The files are stored directly in the repository root.

| File | Contents |
| --- | --- |
| `Sample - Superstore.csv` | Original, unchanged dataset |
| `Superstore_Cleaned.xlsx` | Standalone cleaned dataset containing 9,993 rows and 25 columns |
| `NexaAfrica_Week1_Data_Cleaning.xlsx` | Main workbook containing queries, loaded data, cleaning log, data dictionary, calculated fields, reconciliation, and supporting reviews |
| `README.md` | Project overview, methods, validation results, assumptions, and usage instructions |

## Week 1 deliverables

| Deliverable | Location |
| --- | --- |
| Raw dataset | `Sample - Superstore.csv` |
| Cleaned dataset | `Superstore_Cleaned.xlsx` |
| Data dictionary | `Data_Dictionary` worksheet in the main workbook |
| Data-cleaning documentation | `Cleaning_Log` and `Reconciliation` worksheets, with supporting review sheets |
| Business problem statement | `Overview` worksheet and this README |
| List of calculated fields | `Calculated_Fields` worksheet, with definitions in `Data_Dictionary` |

## Cleaning and validation

- Preserved raw values in a separate query and worksheet.
- Converted Order Date and Ship Date using the English (United States) locale.
- Converted Sales, Discount, and Profit to decimal numbers.
- Verified integer values before setting Row ID and Quantity to Whole Number.
- Preserved Postal Code as Text, including leading zeros such as `05408`.
- Checked all original columns for errors, missing values, and empty strings.
- Checked text fields for whitespace-only values and leading or trailing whitespace.
- Reviewed category, sub-category, region, segment, and shipping-service labels.
- Checked Row ID uniqueness and matching business records.
- Validated numerical ranges and chronological consistency.
- Reviewed the ten largest sales and all negative-profit records.
- Reconciled Sales, Quantity, and Profit totals after cleaning.
- Created and validated date fields and summary metrics.

Detailed decisions and evidence are recorded in `Cleaning_Log` entries CL001–CL022.

## Duplicate-handling assumption

Records with Row IDs 3406 and 3407 matched across all 20 business fields, excluding Row ID.

For this project, Row ID 3406 was retained and Row ID 3407 was removed from the cleaned query. This removed one record, reducing the dataset from 9,994 to 9,993 rows.

This is an assumption: the dataset alone cannot rule out two separately recorded identical order lines. Both original records remain available in the raw data.

## Calculated fields and metrics

| Field / Metric | Calculation | Location | Type / Format |
| --- | --- | --- | --- |
| Days_to_Ship | `Duration.Days([Ship Date] - [Order Date])` | Cleaned dataset | Whole Number |
| Year | `Date.Year([Order Date])` | Cleaned dataset | Whole Number |
| Month | `Date.Month([Order Date])` | Cleaned dataset | Whole Number, 1–12 |
| Year-Month | `Date.ToText([Order Date], "yyyy-MMM", "en-US")` | Cleaned dataset | Text, such as `2016-Nov` |
| Profit Margin | Total Profit ÷ Total Sales | `Calculated_Fields!B4` | Percentage, 2 decimal places |
| Average Order Value | Total Sales ÷ distinct Order ID count | `Calculated_Fields!B6` | Number, 2 decimal places |

### Calculation notes

- Days_to_Ship measures calendar days between order and shipment, not delivery time.
- Year-Month labels distinguish monthly periods across years. Chronological sorting uses Year and Month rather than alphabetical label order.
- Overall Profit Margin is calculated from totals, not by averaging individual row margins.
- Percentage formatting displays the margin as a percentage; the formula does not additionally multiply by 100.
- Average Order Value counts each Order ID once, even when an order contains multiple product lines.
- Summary formulas use underlying values without rounding intermediate totals.
- The current summary formulas cover the entire cleaned table. They do not automatically respond to worksheet filters.

## Validation results

- No errors, empty values, or empty strings were detected in the original 21 columns during full-dataset profiling.
- No whitespace-only values or leading/trailing whitespace were detected by the checks performed.
- Postal codes retained their text representation, including `05408`.
- Days_to_Ship ranged from 0 to 7 calendar days, with no missing results or shipment dates preceding order dates.
- Year contained four distinct values, from 2014 to 2017.
- Month contained 12 distinct values, from 1 to 12.
- Year-Month contained 48 distinct monthly periods.
- Year, Month, and Year-Month each contained 9,993 records with no errors or empty values.
- Sales ranged from 0.444 to 22,638.48.
- Quantity ranged from 1 to 14.
- Discount ranged from 0 to 0.8.
- The cleaned dataset contained 1,870 negative-profit lines. These were retained because negative profit alone does not establish a data error.
- The duplicate-check query returned no matching records after removal.

### Validated summary values

| Metric | Result |
| --- | ---: |
| Total Sales | 2,296,919.49 |
| Total Profit | 286,409.08 |
| Total Quantity | 37,871 |
| Distinct Orders | 5,009 |
| Profit Margin | 12.47% |
| Average Order Value | 458.56 |

Sales, Profit, and Average Order Value are displayed to two decimal places. No currency symbol is assumed.

## Reconciliation

Differences are calculated as **raw total minus cleaned total**.

| Metric | Difference | Explanation |
| --- | ---: | --- |
| Sales | 281.3720 | Sales recorded on removed Row ID 3407 |
| Quantity | 2 | Units recorded on removed Row ID 3407 |
| Profit | −12.0588 | Loss recorded on removed Row ID 3407 |

All three unexplained differences were zero. Monetary residuals were checked to six decimal places.

Removing a loss-making record increased the cleaned Profit total. The reconciliation verifies the numerical effect of the removal; it does not independently validate the duplicate assumption.

## Opening the workbooks

1. Download the Excel files and open them in Excel Desktop.
2. Use `Superstore_Cleaned.xlsx` for the standalone cleaned dataset.
3. Use `NexaAfrica_Week1_Data_Cleaning.xlsx` to inspect the loaded data, documentation, validation formulas, and Power Query steps.
4. Open `Calculated_Fields` to view the summary metrics and complete list of calculations.

The main workbook's CSV connection points to the author's local file location. Review its saved worksheet values without refreshing.

To reproduce the queries, download the original CSV and update the Source step in `Superstore_Raw` to your local file path before refreshing.

The distinct-order formula uses Excel's `UNIQUE` function and requires an Excel version that supports it.

## Limitations

- Source records were not independently verified against transaction documents.
- Currency was not confirmed from the source information accessed, so no currency symbol is assumed.
- Duplicate removal relies on the explicitly documented project assumption.
- Extreme-value reviews were contextual, not formal statistical outlier tests.
- The observed association between large discounts and some losses does not establish causation.
- Passing data-quality checks does not independently prove that every source transaction is accurate.
