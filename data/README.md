# Data provenance and local setup

No row-level dataset is distributed in this public repository.

The audited notebook reads `customer_shopping_behavior.csv` (3,900 rows × 18 columns), then exports `customer_shopping_cleaned.csv` (3,900 × 22). The original source file was located in the author's existing project folder. The rerun output was equal to both the saved cleaned CSV and the original PBIX's embedded table.

## Upstream comparison

Publisher: Sourav Banerjee. Dataset: [Customer Shopping Trends Dataset](https://www.kaggle.com/datasets/iamsouravbanerjee/customer-shopping-trends-dataset), version 2.

The publisher describes the dataset as synthetic. Direct comparison of the project's raw file with `shopping_trends_updated.csv` found identical values across all 17 non-rating columns and all 3,863 observed ratings. The project variant has 37 missing ratings; the upstream file has no missing ratings. The mechanism that removed those ratings was not established.

The similarly named `shopping_trends.csv` is a different 19-column file and is not the matching variant; its Payment Method column differs. Use the **updated** variant when obtaining the upstream file.

## Redistribution decision

The [publisher's metadata endpoint](https://www.kaggle.com/api/v1/datasets/view/iamsouravbanerjee/customer-shopping-trends-dataset) returned `Other (specified in description)` on 2 November 2026. Its educational description did not establish a clear redistribution license. A third-party CC0 label was not treated as authoritative.

CSV files, embedded-data PBIX files, imported Power BI caches and notebook row outputs are excluded from public distribution. Obtain and use data only under applicable rights and terms. No third-party data is relicensed by this repository.

## Reproduce locally

1. Obtain an authorized project CSV or review the publisher's current terms and download the upstream updated file.
2. Create `data/raw/` and place it there as `customer_shopping_behavior.csv`.
3. Run the notebook. The cleaned file is written to `data/processed/customer_shopping_cleaned.csv`.
4. Point the Power BI `DataFilePath` parameter to that cleaned file.

The original project variant is required to reproduce every audited rating/missingness result exactly. With the upstream file, revenue and segmentation results should agree, but review coverage and some rating means will differ. Do not introduce missing values merely to force a benchmark match.

Original raw SHA-256: `d1f0b8e906cde9d909361ea28f08b0f33e74659ed4a42cd1d747fd9db2ffa701`

Original cleaned SHA-256: `2913185d4f9c82fc1804f104ee2181d910ccc56409e17ca11dd5413dda2c5f8e`

## Data grain

One synthetic customer and one recorded purchase per row. There are no repeated customer IDs in this snapshot. A previous-purchase count does not provide the underlying historical transactions.
