# Data

The notebook expects a file `shipping.csv` at the repo root (or wherever you choose to place it — adjust the `pd.read_csv` path in the first cells of `notebooks/shipping-on-time-prediction.ipynb`).

## Source

E-Commerce Shipping Data, publicly available on Kaggle:
https://www.kaggle.com/datasets/prachi13/customer-analytics

## Schema

| Column | Type | Description |
|---|---|---|
| `ID` | int | Customer ID (dropped before modelling) |
| `Warehouse_block` | cat | Warehouse block the shipment originated from (A/B/C/D/F) |
| `Mode_of_Shipment` | cat | Flight / Road / Ship |
| `Customer_care_calls` | int | Number of customer care calls about the shipment |
| `Customer_rating` | int | 1 (worst) – 5 (best) |
| `Cost_of_the_Product` | int | Cost in USD |
| `Prior_purchases` | int | Number of prior purchases by this customer |
| `Product_importance` | cat | low / medium / high |
| `Gender` | cat | M / F |
| `Discount_offered` | int | Discount applied to the product |
| `Weight_in_gms` | int | Weight of the shipment in grams |
| `Reached.on.Time_Y.N` | int | **Target** — 1 = late, 0 = on time |

Total: 10,999 rows.
