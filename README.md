# Sales-store-preprocesing-
# Store Sales

Data cleaning and analysis of a retail store's sales transactions: find the data quality problems, fix them with documented decisions, and extract business insights.

## Dataset

| Field | Value |
|---|---|
| Source | Retail Store Sales dataset (Kaggle) |
| File | `retail_store_sales.csv` |
| Raw size | 12,575 rows x 11 columns |
| Date range | 2022-01-01 to 2025-01-18 |
| One row represents | One sales transaction |

## Problems Found

- **7,229 missing values** across 5 columns: Discount Applied (4,199), Item (1,213), Price Per Unit (609), Quantity (604), Total Spent (604).
- **Combined ID fields:** `Transaction ID`, `Customer ID` and `Item` each pack 2 to 3 pieces of information into one text cell.
- **Wrong data types:** `Transaction Date` stored as text, `Quantity` stored as decimal.
- No duplicate rows, no invalid categories, no impossible values.

## Cleaning Steps

1. **Split columns**
   - `Transaction ID` -> `Transaction_Type` + `Transaction_Number`
   - `Customer ID` -> `Customer_Type` + `Customer_Number`
   - `Item` -> `Item_Prefix` + `Item_Number` + `Item_Code`
   - The numeric parts were converted to integers and the original columns dropped.
2. **Missing values**
   - `Price Per Unit`: recovered with `Total Spent / Quantity` (609 rows).
   - `Quantity`: filled with the median, 6 (604 rows).
   - `Total Spent`: computed as `Quantity x Price Per Unit` (604 rows).
   - `Item`: rebuilt from `Category` and `Price Per Unit` (1,213 rows).
   - `Discount Applied`: filled with the mode, True (4,199 rows).
3. **Data types:** `Transaction Date` converted to datetime, `Quantity` to integer, `Discount Applied` to boolean.
4. **Outliers:** 60 values of `Total Spent` are above the IQR upper bound (402) but are valid (max 410 = 41 x 10), so all rows were kept.

## Results

| Metric | Before | After |
|---|---|---|
| Rows | 12,575 | 12,575 |
| Columns | 11 | 15 |
| Missing values | 7,229 | 0 |
| Duplicate rows | 0 | 0 |
| Rows where Total = Price x Quantity | 11,362 | 12,575 |

## Key Insights

- Total sales are 1,637,367 across 12,575 transactions (average 130.2 per transaction).
- Categories are balanced: Butchers lead with 13.3% of sales, Milk Products trail with 11.6%.
- Online brings 50.8% of sales and in-store 49.2%, with similar average sales.
- Discounts do not raise spending: the average sale is 130.5 with a discount and 130.0 without (rows with a recorded discount status only).
- Sales are spread evenly across the 25 customers (the top 5 make up 21.4%).

## Limitations

- 604 rows (4.8%) have an estimated `Total Spent`, because `Quantity` was filled with the median.
- `Discount Applied` was filled with the mode for about a third of the rows.

## Files

| File | Description |
|---|---|
| `retail_store_sales.csv` | Raw dataset |
| `cleaned_retail_store_sales_corrected.csv` | Cleaned dataset (15 columns, no missing values) |
| `1_Client_Brief.pdf` | One-page non-technical pitch |
| `2_Client_Proposal.pdf` | Non-technical client proposal |
| `3_Complete_Work
