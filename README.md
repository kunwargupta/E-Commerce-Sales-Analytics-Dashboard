# E-Commerce Sales Analytics Dashboard | Olist Brazilian E-Commerce

End-to-end Data Analytics project covering **PostgreSQL database engineering + Power BI (DirectQuery) dashboarding** on the Olist Brazilian E-Commerce dataset (100K+ orders, 2016–2018).

---

## 📊 Project Impact (Quantified)

| Metric | Value |
|---|---|
| Total Revenue analyzed | **₹14.21M** (R$) |
| Total Orders processed | **98.67K** |
| Total Items Sold | **1,42,09,250.31** GMV across states |
| Average Order Value (AOV) | **144.01** |
| On-Time Delivery Rate | **93.23%** |
| Average Review Rating | **4.03 / 5** |
| Positive Reviews (4–5★) | **75.48%** |
| Negative Reviews (1–2★) | **16.96%** |
| Active Sellers | **3,000+** |
| Revenue per Seller | **4.59K** |
| Top Seller Revenue Contribution | **245K** (highest single seller) |
| Total Payment Value Processed | **1,27,71,809.08** across 61,391 orders |
| Payment Mix | **75.32% Credit Card**, 19.44% Boleto, ~2% Voucher/Debit |
| Avg Installments (Credit Card) | **3.7** |
| Customer States Covered | **27** (all Brazilian states) |
| Top State by Revenue | **SP – 5.4M** |
| Top City by Revenue | **2.01M** |
| Product Categories Analyzed | **70+** categories |
| Top Category (Revenue) | **bed_bath_table — 10.92M, 9,417 orders** |
| Database Tables Engineered | **9 raw tables → 5 optimized BI views** |
| Indexes Created for Performance | **9 indexes** across order/customer/product/seller/payment/review keys |
| DAX Measures Built | **20+ measures** across Sales, Time Intelligence, Reviews, Payments, Logistics, Sellers |
| Report Pages Delivered | **9 interactive pages** |
| Connection Mode | **100% DirectQuery** (live, no data import) |

---

## 🧱 Tech Stack

- **Database:** PostgreSQL
- **BI Tool:** Power BI Desktop (DirectQuery mode)
- **Languages:** SQL (DDL, Views, Indexing), DAX
- **Dataset:** [Olist Brazilian E-Commerce Public Dataset](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce) (Kaggle) — 9 raw CSV tables, 100K+ orders

---

## 🔁 End-to-End Workflow

### 1. Data Ingestion & Database Setup
- Created `olist_db` in PostgreSQL.
- Designed and created **9 raw tables**: customers, geolocation, products, sellers, orders, order_items, order_payments, order_reviews, product_category_translation — with primary keys defined for 5 of them.
- Loaded all CSVs via `COPY`/pgAdmin import.

### 2. Data Validation
- Verified row counts, null checks, key uniqueness, and sample selects across all 9 tables before modeling.

### 3. Performance Optimization
- Created **9 targeted indexes** on high-traffic join/filter columns: `order_id`, `customer_id`, `product_id`, `seller_id`, `order_purchase_timestamp` — cutting query scan time on the 100K+ row fact table.

### 4. Analytical SQL Views (5 BI Views)
Reduced join complexity for Power BI by pre-aggregating into:
- `bi_dim_product` — category + dimensions + computed volume
- `bi_fact_review_latest` — deduplicated latest review per order
- `bi_fact_order` — order-level facts with `delivery_days`, `is_late` flag
- `bi_fact_sales` — the core fact table joining items + orders + products + reviews + payments + sellers
- `bi_payments_order` — order-level payment aggregation (sum, max installments, dominant payment type)

### 5. Power BI Data Modeling
- Connected via **DirectQuery** for live data (vs. Import Mode) to keep the dashboard always current.
- Built a **Star Schema**: `bi_fact_sales` as the central fact table, related to a custom `DimDate` calendar table.
- `DimDate` generated via `CALENDAR()` with Year, Month, Quarter, Weekday, Year-Month, and Month Start computed columns.

### 6. DAX Measures (20+)
Grouped into:
- **Base Measures:** Revenue, Freight, GMV, Orders, Customers, Items Sold, AOV, Items per Order, Avg Rating, Delivered Orders, Late Orders, On-time %
- **Time Intelligence:** Revenue YTD, Revenue LY, YoY %, Rolling 30-Day Revenue
- **Review Sentiment:** Positive Reviews %, Negative Reviews %, Review Bucket (SWITCH-based)
- **Payments:** Payment Value, Avg Installments
- **Logistics:** Avg Delivery Days, Late %
- **Sellers:** Sellers (count), Revenue per Seller
- **Drillthrough:** Dynamic Drill Title using `SELECTEDVALUE` + `COALESCE`

### 7. Dashboard Development — 9 Report Pages
1. **Overview Executive Dashboard** — KPIs (Revenue, Orders, Customers, AOV, On-time %, Rating) + Year/Category slicers + trend & status visuals
2. **Sales Trends** — Revenue YTD, Revenue vs Revenue YTD, Rolling 30D Revenue, Year × Category matrix (2016–2018)
3. **Category & Product** — AOV vs Orders scatter, product count by category, full category P&L table, revenue share treemap
4. **Customers Geo** — Revenue by state map, Top 10 states/cities by revenue, category × state matrix
5. **Delivery & Logistics** — Per-state Revenue/Orders/AOV/On-time %/Rating table, Revenue Trend line
6. **Review** — Positive/Negative %, Review Bucket pie, Top categories by rating, Rating by order status (late vs on-time)
7. **Payments** — Payment type pie, Avg Installments by type, Payment Value Trend, full payment summary table
8. **Sellers** — Seller count, Revenue per Seller, Revenue by Seller State map, Top 10 Sellers, category × seller-state matrix
9. **Drill Through** — State-level drillthrough table with Revenue, Orders, AOV, On-time %, Rating filtered by category


---

## 🔑 Key Highlights

- Built a fully **DirectQuery-connected** Power BI dashboard on a 100K+ row PostgreSQL dataset — no data duplication, always-live numbers.
- Reduced front-end query complexity by pushing joins into **5 SQL views**, following industry-standard BI architecture.
- Delivered **9 production-style report pages** with drillthrough, geo-mapping, time intelligence, and sentiment analysis.
- Identified actionable insights: **bed_bath_table** as top revenue category, **SP state** driving 38%+ of total revenue, **75.32% credit-card dependency**, and **93.23% on-time delivery** with late orders averaging a measurably lower rating (2.3 vs 4.1).

---

## 🚀 How to Reproduce

1. Restore the Olist dataset CSVs into PostgreSQL using `sql/01_create_tables.sql`.
2. Run `sql/02_create_indexes.sql` for performance.
3. Run `sql/03_bi_views.sql` to create the 5 BI views.
4. Open Power BI Desktop → Get Data → PostgreSQL → DirectQuery → load the BI views.
5. Import `DateDim.txt` DAX for the date table → mark as Date Table → relate to `purchase_date`.
6. Paste in `DAX_Measures.txt` measures.
7. Rebuild the 9 report pages as per `docs/Steps_To_Follow.txt`.

---

**Author:** Kunwar Ji Gupta
**Dataset:** Olist Brazilian E-Commerce (Kaggle, 2016–2018)
**Tools:** PostgreSQL · Power BI · SQL · DAX
