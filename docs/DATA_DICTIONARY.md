# Data dictionary and metric definitions

The cleaned model contains 22 fields. Numeric ranges below describe the audited sample, not universal business validation rules.

| Field | Type | Meaning / treatment |
|---|---|---|
| `customer_id` | Integer | Synthetic identifier; unique across 3,900 rows |
| `age` | Integer | Age, 18-70 |
| `gender` | Text | Supplied Male/Female label |
| `item_purchased` | Text | 25 product labels; not 25 SKUs or units |
| `category` | Text | Clothing, Accessories, Footwear, Outerwear |
| `purchase_amount_usd` | Integer | Recorded purchase amount, $20-$100; not profit |
| `location` | Text | Supplied U.S. state label; no street addresses |
| `size` | Text | Purchased item size |
| `color` | Text | Purchased item color |
| `season` | Text | Fall, Spring, Summer, Winter; not dated transactions |
| `review_rating` | Decimal / null | Original rating; 2.5-5.0 observed, 37 missing |
| `subscription_status` | Text | Yes/No at observation; no subscription start date |
| `shipping_type` | Text | Supplied shipping service category |
| `discount_applied` | Text | Yes/No; does not contain discount amount |
| `promo_code_used` | Text | Yes/No; identical to discount flag in this sample |
| `previous_purchases` | Integer | Supplied count, 1-50; no dated history |
| `payment_method` | Text | Method category, not account or card details |
| `frequency_of_purchases` | Text | Supplied frequency category; labels left unchanged |
| `rating_was_missing` | Boolean | True when original `review_rating` is missing |
| `review_rating_filled` | Decimal | Original rating or within-category median; excluded from observed-rating KPIs |
| `age_group` | Category | Fixed age bands: 18-29, 30-44, 45-59, 60+ |
| `purchase_history_segment` | Category | `pd.qcut` quartiles on previous purchases |

## Derived groups

| Quartile | Previous purchases | Records | Subscribers | Subscription rate |
|---|---:|---:|---:|---:|
| Q1 | 1-13 | 1,014 | 252 | 24.85% |
| Q2 | 14-25 | 951 | 256 | 26.92% |
| Q3 | 26-38 | 1,007 | 294 | 29.20% |
| Q4 | 39-50 | 928 | 251 | 27.05% |

Quartile sizes differ because ties share the same boundaries. Group assignments are calculated by Python, not recomputed by Power BI slicers. They may change for a new dataset.

Category rating medians used for filling: Accessories 3.8, Clothing 3.7, Footwear 3.8, Outerwear 3.8. All originally observed ratings remain unchanged.

## Headline measure definitions

- Total Revenue: sum of recorded purchase amounts within filter context.
- Total Customers: distinct customer IDs within filter context.
- Average Purchase: mean recorded purchase amount within filter context.
- Subscribers: count of Yes-subscribed customers, intersecting existing filters; 0 for an empty numerator.
- Subscription Rate: Subscribers / Total Customers; blank for an absent denominator.
- Discount Usage Rate: discounted row count / row count, intersecting existing filters; blank for an absent denominator.
- Average Review Rating: average of nonblank original ratings, not imputed ratings.
- Revenue % by Category: category revenue divided by revenue with the category filter removed. Other filters remain. This is an **all-category share within other active filters**, not a selected-category share.
- Total Products: distinct product labels, not inventory or units sold.

See `powerbi/Measures.dax` for the public source formulas.
