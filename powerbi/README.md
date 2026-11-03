# Power BI source project

Open **Retail_Customer_Analytics.pbip** in a recent Power BI Desktop release supporting PBIP/PBIR. The project contains four report pages, one 22-column import table and 12 DAX measures. It is reconstructed from the supplied PBIX and intentionally contains no imported data cache.

## Configure and refresh

1. Read `../data/README.md`, supply an authorized CSV and run the Python notebook.
2. Open the PBIP.
3. In **Transform data → Manage parameters**, change `DataFilePath` to the absolute path of `data/processed/customer_shopping_cleaned.csv` on your machine. The generic example path in the repository is not executable until changed.
4. Refresh, then use the acceptance checks in `../docs/AUDIT.md`.

If your Desktop build cannot open PBIP/PBIR, use a supported current build. The [Microsoft project documentation](https://learn.microsoft.com/en-us/power-bi/developer/projects/projects-overview) describes the project structure and feature requirements.

## What was changed

- Top 10 title corrected to agree with the original filter.
- Gender averages changed from a donut to a zero-based bar comparison.
- Subscriber/discount measures intersect existing filters via `KEEPFILTERS`.
- Data path parameterized and private local paths removed.
- Static annotations explicitly labeled as full-sample insights.

The original screenshots show the pre-audit design. The reconstructed source was checked structurally, but Desktop rendering, refresh and live DAX/filter interaction were not tested here. The original PBIX is excluded because it contains row-level data without established redistribution permission.

`Measures.dax` is a readable copy of the measures installed in the TMDL model. It does not need separate import. All review KPIs use `review_rating`, never `review_rating_filled`.
