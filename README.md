# maheshfarkande_2511463_part4_tableau_dashboard

# Dataset Summary: `dashboard_sales_data.xlsx`

A single-sheet retail sales/order dataset with 4200 rows and 20 columns, one row per order line. It covers orders placed across **2024–2025** in an India-based retail context (states/cities are Indian). 

## Field Inventory by Type

### Date fields

| Field | Type | Range / Notes |
|---|---|---|
| `order_date` | datetime | 2024-01-01 → 2025-12-31 |
| `ship_date` | datetime | 2024-01-03 → 2026-01-06 |

Every `ship_date` is on or after its `order_date` (317 orders ship same-day). `delivery_days` exactly equals `ship_date − order_date` for all 4,200 rows, so it is a derived field, not an independent measure.

### Geographic fields (all text/categorical)

| Field | Unique | Values |
|---|---|---|
| `region` | 4 | East, North, South, West |
| `state` | 19 | Indian states (Punjab, Rajasthan, Goa, Kerala, …) |
| `city` | 20 | Indian cities (Ludhiana, Jaipur, Hyderabad, …) |

### Categorical fields (text)

| Field | Unique | Values |
|---|---|---|
| `customer_segment` | 3 | Consumer, Corporate, Home Office |
| `category` | 3 | Furniture, Office Supplies, Technology |
| `sub_category` | 13 | Tables, Chairs, Storage, Phones, etc. |
| `ship_mode` | 4 | First Class, Second Class, Standard Class, Same Day |
| `campaign_channel` | 5 | Email, Organic, Paid, Referral, Social |
| `customer_id` | 4,096 | High-cardinality identifier (`C-#####`) |
| `product_name` | 4,111 | High-cardinality (near-unique per row) |
| `order_id` | 4,200 | Primary key (`DB-YYYY-######`) |

### Numerical measures

| Field | Type | Min | Mean | Max | Notes |
|---|---|---|---|---|---|
| `sales` | float | 72.91 | 51,670 | 427,418 | Right-skewed |
| `quantity` | int | 1 | 5.5 | 10 | |
| `discount` | float | 0.00 | 0.14 | 0.35 | Discrete steps of 0.05 |
| `profit` | float | −17,757 | 7,930 | 133,628 | 288 rows (6.9%) are losses |
| `delivery_days` | int | 0 | 3.6 | 9 | Derived from dates |
| `customer_rating` | float | 2.1 | 4.06 | 5.0 | 1–5 scale; 32 nulls |

### Binary / flag fields

| Field | Type | Encoding |
|---|---|---|
| `return_flag` | int (0/1) | 0 = not returned (4,009), 1 = returned (191 ≈ 4.5%) |

### Missing values

| Field | Missing | Share |
|---|---|---|
| `customer_rating` | 32 | 0.8% |
| `campaign_channel` | 24 | 0.6% |

All other 18 columns are fully populated.

## Documented Assumptions

1. **Currency is unstated** — `sales`/`profit` magnitudes (up to ~427K) combined with Indian geography so assumption is  **INR**, not USD. 
2. **`delivery_days` is derived**, so treat it as redundant with the date pair rather than an independent signal.
3. **Missing values are sparse and likely "not collected"**: `customer_rating` (32, 0.8%) probably means no rating submitted; `campaign_channel` (24, 0.6%) likely an unattributed order. Decide whether to impute, drop, or label "Unknown."
4. **`discount` is a fractional rate** (0–0.35), not a currency amount.
5. **`product_name` and `customer_id` are near-unique**, so they are identifiers/labels, not low-cardinality categories — so avoid using them as grouping dimensions in a dashboard.
6. **`order_id` year prefix matches `order_date` year** in spot checks, so the ID encodes order year reliably.
7. One row's order date can be late-2025 with a ship date spilling into early **2026** — expected, not an error.
