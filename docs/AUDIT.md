# Project audit

Audit date: **2 November 2026**. Scope: the supplied final notebook, PBIX and four PNGs, plus the raw and cleaned CSVs located in the author's existing project folder.

## What was verified

| Check | Result | Evidence / boundary |
|---|---|---|
| Original notebook syntax and sequence | Passed | All 43 nonempty code cells executed in order; original contains 44 code cells including one empty cell |
| Original assertions | Passed | Unique IDs, positive amounts, rating bounds, missingness preservation and export schema |
| Notebook output versus saved cleaned CSV | Passed | All 3,900 rows × 22 fields equal, with row order preserved |
| PBIX embedded data versus notebook output | Passed | All fields equal after ordering by customer ID |
| Source linkage | Passed | 17 non-rating fields and 3,863 observed ratings equal to upstream updated variant |
| PBIX model inspection | Completed | One imported table, 22 columns, 12 DAX measures; source Power Query and report definitions decoded |
| Headline screenshot values | Matched at displayed precision | $233,081; 3,900; $59.76; 27%; 43%; 3.75; product/category/age/season/segment summaries |
| Portable notebook execution | Passed | Fresh Jupyter kernel; output compared to audited cleaned data |
| Public-source model serialization | Passed | Microsoft TMDL serializer parsed the model; see validation JSON |
| Public-source report structure | Passed | 57 JSON files parsed, 71 model references resolved, 48 visual containers within four page bounds |
| Microsoft JSON schema checks | Partial | 10 documents passed; the supplied visualContainer 2.14.0 schema URL returned HTTP 404, preventing formal schema validation of 48 visual definitions |
| Power BI Desktop rendering / refresh / live DAX | Not performed | Static source verification does not establish successful Desktop execution |

The original PBIX SHA-256 is `3da4821872b8576614bebc1a45bb20e8573e3ee5c7f624f548d1c97fed5f54a9`.
The original notebook SHA-256 is `789a1a125f0a185ad1a57ff05c199f86f6761deac890fb173de7719458c558c0`.

## Changes in the public portfolio

### Notebook

- Added business context, English section headings, interpretation and data-access notes.
- Replaced working-directory-specific input/output paths with repository-relative paths.
- Preserved all nonempty original code cells and analytical operations.
- Cleared saved outputs and execution counts so row-level data is not redistributed.
- Kept original rating values and missing flags separate from imputed ratings.

### Power BI source reconstruction

The public PBIP was assembled from the supplied PBIX's report definitions, theme resources, decoded schema, measures and Power Query. It does not include the embedded data model/cache, SecurityBindings, private source paths or user settings. The original PBIX was not modified or presented as a newly verified binary.

| Issue in original PBIX / screenshot | Public source change |
|---|---|
| “Top 5 Products” title with a Top 10 filter | Title changed to “Top 10 Products by Revenue”; the ranking is retained |
| Gender averages shown as donut shares | Changed to a zero-based bar chart; average values are not additive parts of a total |
| `CALCULATE` Yes filters replace a same-column No filter | Added `KEEPFILTERS` to subscriber and discount calculations; empty numerators return 0 |
| Hardcoded local CSV source path | Introduced a required `DataFilePath` parameter with a generic example path |
| Static narrative may be mistaken for a response to slicers | Relabeled text “Full-sample insight”; it remains static |

The filter issue is a code-level finding supported by [Microsoft's KEEPFILTERS documentation](https://learn.microsoft.com/en-us/dax/keepfilters-function-dax). For example, selecting Subscription=No in the original formula can replace No with Yes for the numerator. The original full-sample rate is still correct. The revised formula intersects the filters. DAX was not executed against a live Analysis Services engine during this audit.

`Revenue % by Category` retains the original all-category denominator. Its definition is explicit in the data dictionary; it does not claim to show share among only selected categories.

The screenshots are unchanged evidence of the supplied dashboard. They are not previews of the reconstructed source after these edits. Desktop presentation, text wrapping, visual interactions and filtered narrative behavior still require acceptance testing.

## Desktop acceptance checks

Using an authorized copy of the audited data:

1. Open the PBIP in a recent Power BI Desktop build with PBIP/PBIR support. Set `DataFilePath` to the generated cleaned CSV and refresh.
2. Clear filters. Confirm revenue $233,081, customers 3,900, average purchase $59.7643589744, subscribers 1,053, subscription rate 27%, discount usage 43% and observed rating mean 3.7500647165.
3. Select Subscription=No in the filter pane: Subscribers and Subscription Rate should be 0 with 2,847 customers. Select Discount=No: Discount Usage Rate should be 0 with 2,223 records.
4. Apply Subscription=Yes and Discount=No together: the selection is empty. Averages and rates should be blank; the subscriber numerator is 0.
5. Review Top 10 labels, the gender bar axis, all four pages and theme resources. Verify no chart labels are clipped and that category/season totals reconcile.
6. Exercise slicers and chart selections. Static “Full-sample insight” text should be read only as the unfiltered baseline; it is not a dynamic annotation.

These are documented checks, not claimed completed tests.

## Privacy and redistribution review

The row schema contains synthetic IDs, demographics, categorical shopping attributes and amounts. No names, email addresses, phone numbers, street addresses or payment account details were identified in the inspected tables. The notebook contains no database connection or credential. The PBIX uses a local CSV path and embeds all 3,900 rows.

Privacy inspection does not establish redistribution rights. The publisher's current label was “Other (specified in description)”; therefore the public package omits both CSVs and the data-bearing PBIX. Notebook outputs are cleared. The public-source model uses a generic path and contains definitions only.

## Analytical boundaries

The data is synthetic. There are no reliable dates, costs, prices before discounts, discount amounts, campaign assignment, returns or longitudinal order/subscription records. Descriptive comparisons cannot prove profit, growth, retention, churn, CLV, causal uplift or real-market demand. Ratings differ only slightly and no significance tests were performed. Missing reviews may be nonrandom. All subscribers have discounts, and the discount and promo-code fields are identical.
