# Data Dictionary

All files are in `Datasets/`. Format: **semicolon-delimited**, **comma as decimal separator** (e.g. `261,96`), dates as `dd/mm/yyyy`, Windows-1252 text encoding. In Excel/Python, set these explicitly, e.g. `pd.read_csv(path, sep=";", decimal=",", encoding="latin-1")`.

## Orders.csv (9,994 rows, one row per order line)
| Column | Type | Description |
|---|---|---|
| Row ID | integer | Unique row number |
| Order ID | text | Order identifier (an order can have several lines) |
| Order Date | date | Date the order was placed (Jan 2020 to Dec 2023) |
| Ship Date | date | Date the order shipped |
| Ship Mode | text | Standard Class, Second Class, First Class, Same Day |
| Customer ID | text | Foreign key to `Customers.csv` |
| Segment | text | Consumer, Corporate, Home Office |
| Postal Code | integer | Foreign key to `Location.csv` |
| Product ID | text | Foreign key to `Products.csv` |
| Sales | decimal | Line revenue (USD) |
| Quantity | integer | Units sold |
| Discount | decimal | Discount applied (0 to 0.8) |
| Profit | decimal | Line profit (USD), negative = loss |

## Customers.csv (793 rows)
| Column | Type | Description |
|---|---|---|
| Customer ID | text | Unique customer key |
| Customer Name | text | Customer full name |

## Products.csv (1,894 rows)
| Column | Type | Description |
|---|---|---|
| Product ID | text | Product key |
| Category | text | Furniture, Office Supplies, Technology |
| Sub-Category | text | 17 sub-categories (e.g. Chairs, Phones, Binders) |
| Product Name | text | Product description |

## Location.csv (632 rows)
| Column | Type | Description |
|---|---|---|
| Postal Code | integer | Postal code (leading zeros are missing, e.g. `6824` is `06824`) |
| City | text | City |
| State | text | US state |
| Region | text | Central, East, South, West |
| Country/Region | text | United States |

## Relationships in the Tableau data model
`orders.Customer ID = customers.Customer ID`, `orders.Postal Code = location.Postal Code`, `orders.Product ID = products.Product ID` (Tableau relationships, orders is the central table).

## Known data quality notes
- 32 `Product ID`s appear more than once in `Products.csv` (same ID, different product names).
- Postal code `92024` appears twice in `Location.csv` (San Diego and Encinitas).
- One location row (Burlington, Vermont) has no postal code.
- US postal codes starting with 0 lost the leading zero (numeric storage).
- A few names in `Customers.csv` contain encoding artefacts (e.g. `H?berlin`) and some have inconsistent capitalisation of the ID prefix (`Co-12640`, `Dl-13600`).
