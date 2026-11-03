# 🛒 Olist E-Commerce Analytics — Power BI Dashboar
> An interactive 3-page Power BI dashboard built on the Brazilian Olist marketplace data (2016–2018): **96K delivered orders, 96K customers, 113K items sold and 99K reviews**.

**Graduation project — Data Analysis Diploma, Smart Investment Solution (SIS)**
**Academic Advisor : Dr.Mohamed Sobhy
**Author:** Hassan Wahba · [LinkedIn](https://www.linkedin.com/in/hassan-wahba-a0777013a) · [GitHub](https://github.com/enghassanwahba)

**Contents:** [Summary](#-executive-summary) · [Overview](#-project-overview) · [Dashboard](#-dashboard-pages) · [Visual Guide](#-how-each-visual-was-built) · [Insights](#-key-insights--recommendations) · [Data Model](#-data-model) · [Data Cleaning](#-data-cleaning-power-query) · [DAX](#-dax-measures) · [Limitations](#-limitations--future-work) · [Skills](#-tools--skills) · [Usage](#-how-to-use) · [Contact](#-contact)

---

## ⚡ Executive Summary

| 💰 Total Revenue | 📦 Delivered Orders | ⭐ Average Rating | 💳 Credit-Card Share |

| **16.01M**        |            **96K**  |      **4.09 / 5**  |   **~78%**            |

Revenue grew about **20%** from 2017 to 2018, driven by more orders, while the average order value fell from **222 to 165**. Customer satisfaction is high (**57.77%** five-star reviews), and the weak spots are concentrated in a few categories such as office furniture and audio.

---

## 🎯 Project Overview

Olist connects small businesses across Brazil to large online marketplaces. This project turns its raw, multi-table dataset into a clean data model and an executive-style dashboard that answers:

- How have **revenue and orders** evolved from 2016 to 2018?
- Which **payment methods** generate the most revenue?
- How is the **average order value** changing?
- Which **product categories** drive the most revenue?
- How **satisfied** are customers, and which categories get the best and worst reviews?

| **Dataset** | [Brazilian E-Commerce Public Dataset by Olist (Kaggle)](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce) |
| **Period** | 2016 – 2018 |
| **Tables** | customers, orders, order items, payments, reviews, products, sellers, category translation, plus a custom `DimDate` calendar table |
| **Interactivity** | Page navigation buttons and slicers for Year, Payment Type, Category and Review Score |

---

### 🔄 Project Workflow

| # | Stage | What was done |
|---
| 1 | **Load**  | Imported the Olist CSV files from Kaggle into Power BI |
| 2 | **Clean** | Cleaned and typed every table in Power Query, removing duplicates only on true keys |
| 3 | **Model** | Built a snowflake schema with relationships and a DAX calendar table |
| 4 | **Measure** | Wrote DAX measures for revenue, orders, customers and review KPIs |
| 5 | **Visualize** | Designed a 3-page interactive dashboard with slicers and navigation |
| 6 | **Analyze** | Extracted insights and business recommendations |

---

## 📊 Dashboard Pages

### 1️⃣ Revenue
---
<img width="3981" height="2732" alt="Brazilian E-Commerce Public Dataset by Olist_page-0001" src="https://github.com/user-attachments/assets/ad3a8efe-da74-4cc7-a27b-97b5a862e5aa" />
---
KPI cards (revenue, orders, customers, items sold), revenue and orders by year, sales trend over time, and revenue by payment type. Slicers: **Year**, **Payment Type**

### 2️⃣ Customer
---
<img width="3981" height="2732" alt="Brazilian E-Commerce Public Dataset by Olist_page-0002" src="https://github.com/user-attachments/assets/d96f0b63-4ff3-4704-a096-06c7a1d21d51" />
---
Average order value by year and the top 5 categories by revenue. Slicers: **Year**, **Category**.

### 3️⃣ Reviews
---
<img width="3981" height="2732" alt="Brazilian E-Commerce Public Dataset by Olist_page-0003" src="https://github.com/user-attachments/assets/30bd897a-ef64-4cce-b460-6f719b48c363" />
---
Review KPIs, distribution of reviews by score, and the top and bottom 5 categories by star-review and negative-review rate. Slicers: **Category**, **Review Score**.
---

## 🧩 How Each Visual Was Built

All pages share the same design: a dark background, green and orange accent colors, rounded borders around every visual, the Olist logo on the top left, slicers on the top right and four KPI cards on the left. All visuals are driven by the DAX measures and the data model described below.

### 🧭 Navigation (all pages)

| Element | How it was built |
| **Page buttons** (Revenue · customer · reviews) | Speech-bubble shaped buttons with a **Page navigation** action. The button of the current page is highlighted in light green |
| **Filter button** (funnel icon) | An icon button next to the slicers that gives a quick way to reset the filters |
| **Olist logo** | An image placed in a white rounded banner on the top left |
| **Slicers** | Dropdown slicers set to *All* by default. They filter every visual on the page through the model relationships. The **Year** slicer is synchronized between the Revenue and Customer pages, and the **category_name** slicer between the Customer and Reviews pages, so a selection carries over when you switch pages. The `payment_type` and `review_score` slicers exist on one page only |

### 1️⃣ Revenue page

| Visual | Type | Fields | Notes |

| **Year** slicer | Dropdown slicer | `DimDate[Year]` | Filters revenue, orders and all charts by year |
| **payment_type** slicer | Dropdown slicer | `order_payments[payment_type]` | Filters by credit card, boleto, voucher and debit card |
| **KPI cards** (4) | Card | `Total_Revenue`, `Total_Orders`, `Total_Customers`, `Items_Sold` | Label in orange, value in white |
| **Revenue and Orders by Year** | Combo chart (columns and line) | X-axis: `DimDate[Year]`. Columns: `Total_Revenue`. Line: `Total_Orders` | The line is on a secondary axis, with a **logarithmic scale** so that 2016, which has very few orders, stays visible next to 2017 and 2018. Data labels show the values on both series |
| **Sales Trend Over Time** | Smoothed line chart | X-axis: `DimDate[Year]`. Y-axis: `Total_Revenue` | Data labels show revenue in millions with three decimals (0.059M, 7.250M, 8.700M) |
| **Total_Revenue by payment_type** | Horizontal bar chart | Y-axis: `order_payments[payment_type]`. X-axis: `Total_Revenue` | Shows how much revenue each payment method brings |

### 2️⃣ Customer page

| Visual | Type | Fields | Notes |

| **Year** slicer | Dropdown slicer | `DimDate[Year]` | |
| **category_name** slicer | Dropdown slicer | English product category from `products` | Filters the page by product category |
| **KPI cards** (4) | Card | `Total_Revenue`, `Total_Orders`, `Total_Customers`, `Items_Sold` | Same cards as the Revenue page |
| **AVG_order_value by Year** | Smoothed line chart | X-axis: `DimDate[Year]`. Y-axis: `average_order_value` | Shows how the value of an average order changes from 2016 to 2018 |
| **Top 5 Categories By Revenue** | Horizontal bar chart | Y-axis: English product category. X-axis: `Total_Revenue` | A **Top N filter** keeps the 5 categories with the highest revenue |

### 3️⃣ Reviews page

| Visual | Type | Fields | Notes |

| **category_name** slicer | Dropdown slicer | English product category from `products` | |
| **review_score** slicer | Dropdown slicer | `order_reviews[review_score]` | Filters by 1 to 5 stars |
| **KPI cards** (4) | Card | `total_reviews`, `star_review_rate`, `negative_review_rate`, `Average_Rating` | Rates are formatted as percentages with two decimals |
| **star_review_rate vs negative_review_rate** | Column chart | Values: `star_review_rate` (green) and `negative_review_rate` (red) | Two measures side by side with a legend, for a quick view of the satisfaction balance |
| **total_reviews by review_score** | Donut chart | Legend: `review_score`. Values: `total_reviews` | Labels show each score's percentage of reviews, and the total (99K) is shown in the center |
| **top 5 category star review rate** | Horizontal bar chart | Y-axis and legend: English product category. X-axis: `star_review_rate` | A **Top N filter** keeps the 5 categories with the highest star-review rate |
| **Bottom 5 category negative review rate** | Horizontal bar chart | Y-axis and legend: English product category. X-axis: `negative_review_rate` | A **Top N filter** keeps the 5 categories with the highest negative-review rate, meaning the worst performers |

---

## 💡 Key Insights & Recommendations

| Area | Finding |

| **Revenue** | Total revenue of **16.01M**; grew from **7.25M (2017)** to **8.70M (2018)**, about **+20%**. |
| **Orders** | **96.48K** delivered orders; up from **43.4K (2017)** to **52.8K (2018)**, about **+22%**. |
| **Customers & items** | **96.10K** unique customers and **112.65K** items sold. Customers are almost as many as orders, which suggests few repeat purchases. |
| **Payments** | **Credit card** brings **12.54M (~78%)**, then boleto (2.87M), voucher (0.38M) and debit card (0.22M). |
| **Order value** | Average order value fell from **222 (2016)** to **167 (2017)** and **165 (2018)**. |
| **Categories** | Top by revenue: **health_beauty (1.45M)**, watches_gifts (1.31M), bed_bath_table (1.27M), sports_leisure (1.17M), computers_accessories (1.07M). |
| **Satisfaction** | Average rating **4.09 / 5**; **57.77%** of reviews are 5-star and **14.69%** are negative (1–2 stars). |
| **Weakest categories** | Highest negative-review rates: security_and_services (50.00%), fashion_male_clothing (26.13%), fashio_female_clothing (23.08%), office_furniture (22.64%), audio (21.90%). |
| **Strongest categories** | Highest star-review rates: fashion_childrens_clothes (87.50%), cds_dvds_musicals (75.00%), la_cuisine (75.00%), books_imported (73.58%), books_general_interest (73.23%). |

> The top and bottom review-rate categories are mostly small categories with few reviews, so read their rates with caution. `fashio_female_clothing` is spelled this way in the original Olist data.

**Recommendations**

1. **Reduce dependence on credit cards** by promoting boleto, debit and voucher offers and installment options.
2. **Lift average order value** (222 → 165) with bundles, free-shipping thresholds and cross-selling in the top categories.
3. **Double down on health_beauty, watches_gifts and bed_bath_table**, the largest revenue drivers.
4. **Investigate the worst-rated categories** (office_furniture, audio, fashion) for quality, delivery delays or description mismatches.
5. **Use the best-rated categories as quality benchmarks** for seller onboarding.

---

## 🧱 Data Model

The model is a **snowflake schema** built in Power BI's Model view. Instead of connecting every dimension directly to the fact tables, some dimensions (`customers` and `DimDate`) connect **through the `orders` table**, which then feeds the fact tables. This normalized, multi-level structure keeps each piece of information in one place and avoids repeating customer and date details on every row.
---
<img width="1812" height="996" alt="snow schema" src="https://github.com/user-attachments/assets/3db1f406-0bc6-4c43-b790-c58d3bb399d1" />
---
| Role | Tables |

| **Fact tables** | `order_items`, `order_payments`, `order_reviews` |
| **Dimension tables** | `customers`, `products`, `sellers`, `DimDate` |
| **Central order table** | `orders`, linking customers and dates to the fact tables |
| **Measures table** | `_Measures`, holds the DAX measures and has no relationships |

| From (One) | To (Many) | Key | Cardinality |

| `customers` | `orders` | `customer_id` | One to many |
| `DimDate` | `orders` | `Date` → purchase date | One to many |
| `orders` | `order_items` | `order_id` | One to many |
| `orders` | `order_payments` | `order_id` | One to many |
| `orders` | `order_reviews` | `order_id` | One to one |
| `products` | `order_items` | `product_id` | One to many |
| `sellers` | `order_items` | `seller_id` | One to many |

The `customers` ↔ `orders` and `orders` ↔ `order_items` relationships filter in **both directions**, so category and customer slicers also filter reviews and revenue.

The `product_category_name_translation` table is not related to anything in the model. Its English category names are merged into `products` in Power Query.

---

## 🧹 Data Cleaning (Power Query)

All tables were loaded from the Olist CSV files and cleaned in Power Query. The guiding rule: **remove duplicates only on each table's true key**, so no real transactions are lost (an order can have several items, several payments and, rarely, several reviews).

Every imported table starts with the same three steps, then continues with table-specific steps:

| Common step | What it does |

| **Source** | Loads the CSV file (comma-delimited) with the right number of columns and encoding: Windows-1252 for most tables, UTF-8 for `order_reviews` and `sellers`, which contain special characters |
| **Promoted Headers** | Turns the first row of the file into column names |
| **Changed Type** | Assigns the correct data type to every column (text for IDs, whole number for counts, decimal for money, date/time for timestamps) so relationships, sums and date logic work correctly |

### 1. `customers` (5 columns)

| Step | What it does and why |

| Changed Type | `customer_id` and `customer_unique_id` as text, zip code as whole number, city and state as text |
| Removed Duplicates on `customer_id` | Guarantees one row per `customer_id`, the key that links customers to orders (each order gets its own `customer_id`). This makes the relationship a clean one-to-many |
| Filtered Rows | Removes rows where `customer_id` is empty or null, since a customer without a key cannot be linked to any order |

`customer_unique_id` was intentionally left untouched, so the `Total_Customers` measure can count real, unique customers (the same person can have several `customer_id` values).

### 2. `orders` (8 columns)

| Step | What it does and why |

| Changed Type | IDs and `order_status` as text; the five date columns (purchase, approved, delivered to carrier, delivered to customer, estimated delivery) as date/time |
| Removed Duplicates on `order_id` | Guarantees one row per order, the key that links orders to items, payments and reviews |
| Filtered Rows | Removes rows with an empty `order_id` |
| Changed Type (purchase date) | Converts `order_purchase_timestamp` from date/time to **date only**, so it matches the date column of the `DimDate` calendar table and the Year slicer and trend charts work per day |

All order statuses are kept in this table. The "delivered only" logic is applied in the `Total_Orders` DAX measure, so no payments are left without a matching order.

### 3. `order_items` (7 columns)

| Step | What it does and why |

| Changed Type | IDs as text, `order_item_id` as whole number, shipping limit date as date/time, `price` and `freight_value` as decimals |
| Removed Duplicates on `order_id` + `order_item_id` | The real key of this table is the **combination** of the order and the item's position inside it. An order with 3 products has 3 rows, and all of them are kept |
| Filtered Rows | Removes rows with an empty `order_id` |

### 4. `order_payments` (5 columns)

| Step | What it does and why |

| Changed Type | `order_id` and `payment_type` as text, `payment_sequential` and `payment_installments` as whole numbers, `payment_value` as decimal |
| Removed Duplicates on `order_id` + `payment_sequential` | An order can be paid in several parts (for example card plus voucher), and `payment_sequential` numbers each part. Using both columns keeps every real payment, so total revenue is complete |
| Filtered Rows | Removes rows with an empty `order_id` |

### 5. `order_reviews` (7 columns)

| Step | What it does and why |

| Source (UTF-8, CSV quoting) | The review comments contain commas, line breaks and Portuguese characters, so the file is read as UTF-8 with CSV quote handling to keep rows intact |
| Changed Type | IDs as text, `review_score` as whole number, creation date and answer time as date/time |
| Sorted Rows (newest answer first) + Buffer | Sorts reviews by `review_answer_timestamp` descending and freezes that order in memory, so the next step reliably keeps the newest review |
| Removed Duplicates on `order_id` | Some orders have more than one review. Keeping only the **latest review per order** makes the relationship with `orders` one-to-one and avoids counting an order twice |
| Filtered Rows | Removes rows with an empty `order_id` |
| Removed Columns | Drops `review_comment_title` and `review_comment_message`. They are free text that the dashboard does not use, and removing them makes the model lighter |

### 6. `products` (9 columns)

| Step | What it does and why |

| Changed Type | IDs and category as text; name length, description length, photo count, weight and the three dimensions as whole numbers |
| Merged Queries (left outer join) | Joins `product_category_name_translation` on the Portuguese category name, so each product gets its English category. A left join keeps every product even when no translation exists |
| Renamed + Expanded Columns | Renames the joined table column to `product_category_name_English` and expands it to bring in the English name |
| Replaced Value: null → `Others` (English name) | Products without an English translation are grouped under **Others** instead of showing a blank category in the charts |
| Replaced Value: null → 0 (`product_photos_qty`) | Missing photo counts become 0 |
| Changed Type (`product_photos_qty`) | Re-applies whole-number type after the replacement |
| Replaced Value: empty → `Others` (`product_category_name`) | Products with an empty Portuguese category are also labeled **Others** |
| Replaced Value: null → 0 (`product_description_lenght`, `product_name_lenght`) | Missing description and name lengths become 0 |
| Removed Duplicates on `product_id` | Guarantees one row per product, the key linking products to order items |

### 7. `sellers` (4 columns)

| Step | What it does and why |

| Changed Type | `seller_id`, city and state as text, zip code as whole number |
| Removed Duplicates on `seller_id` | Guarantees one row per seller, the key linking sellers to order items |
| Filtered Rows | Removes rows with an empty `seller_id` |

### 8. `DimDate` (calendar table, built with DAX)

This table does **not** come from the Olist files. It is a **calculated table** created with DAX inside the model, and it is the date dimension that powers the Year slicer and every time-based chart.

**How it is built**

| Step | What it does |

| Variables | Finds the earliest and latest **year** of the order purchase date in `orders` (2016 to 2018), and sets the fiscal year start month to **April** |
| `CALENDAR` | Generates one row per day, from January 1, 2016 to December 31, 2018, so the calendar has no gaps |
| `ADDCOLUMNS` | Adds the descriptive columns below to every date |

**Columns (28 in total)**

| Group | Columns | Meaning |
|---|---|---|
| Key | `Date` | One row per day; related to the purchase date in `orders` |
| Year | `Year`, `Year Start`, `YearEnd` | Calendar year and its first and last day |
| Month | `MonthNumber`, `MonthName`, `MonthNameShort`, `MonthStart`, `MonthEnd`, `DaysInMonth` | Month number (1–12), full and short name, first and last day, number of days |
| Year-month | `YearMonthNumber`, `YearMonthName` | Sortable key such as 201709 and a label such as 2017-Sep |
| Day | `DayNumber`, `DayName`, `DayNameShort`, `DayOfWeek` | Day of month, full and short weekday name, weekday number |
| Quarter | `Quarter`, `QuarterName`, `YearQuarterNumber`, `YearQuarterName`, `QuarterStart`, `QuarterEnd` | Quarter number, label such as Q3, year-quarter key and label, first and last day |
| Week | `WeekNumber`, `WeekStart`, `WeekEnd` | Week of the year and the first and last day of that week |
| Fiscal | `FiscalYear`, `FiscalQuarter`, `FiscalMonth` | Fiscal calendar with the year starting in **April** |

**Why a separate calendar table:** it lets all time analysis run on one shared, gap-free date column, so year, quarter and month views stay consistent across every page.

### Design principles
- **Remove duplicates only on each table's true key**, never on descriptive columns such as zip code, category or score. This avoids deleting real transactions.
- **Keep all order statuses** in Power Query and filter delivered orders in DAX, so revenue, orders and dates stay consistent.
- **Replace missing values with clear labels** (`Others`, 0) instead of leaving blanks in the visuals.

---

## 🧮 DAX Measures

All measures live in a dedicated `_Measures` table.

| Measure | What it returns |
|---|---|
| `Total_Revenue` | Sum of payment value |
| `Total_Orders` | Distinct delivered orders |
| `Total_Customers` | Distinct unique customers |
| `Items_Sold` | Number of items sold |
| `average_order_value` | Revenue ÷ orders |
| `total_reviews` | Number of reviews |
| `Average_Rating` | Average review score (1–5) |
| `star_review_rate` | Share of 5-star reviews |
| `negative_review_rate` | Share of 1–2 star reviews |

```dax
-- ===== Sales & Customers =====

-- Total money paid by customers
Total_Revenue = CALCULATE(
    SUM(olist_order_payments_dataset[payment_value])
)

-- Distinct orders that were delivered
Total_Orders =
CALCULATE(
    DISTINCTCOUNT(olist_orders_dataset[order_id]),
    olist_orders_dataset[order_status] = "delivered"
)

-- Distinct customers (a customer may have several customer_id values)
Total_Customers = CALCULATE(
    DISTINCTCOUNT(olist_customers_dataset[customer_unique_id])
)

-- Number of items sold
Items_Sold = CALCULATE(
    COUNT(olist_order_items_dataset[product_id])
)

-- Average value of an order
average_order_value = [Total_Revenue] / [Total_Orders]


-- ===== Reviews =====

-- Number of reviews
total_reviews = CALCULATE(
    COUNT(olist_order_reviews_dataset[review_id])
)

-- Average rating (1 to 5)
Average_Rating = CALCULATE(
    AVERAGE(olist_order_reviews_dataset[review_score])
)

-- Share of 5-star reviews
star_review_rate = CALCULATE(
    COUNT(olist_order_reviews_dataset[review_score]),
    olist_order_reviews_dataset[review_score] = 5
) / [total_reviews]

-- Share of negative reviews (1 or 2 stars)
negative_review_rate = CALCULATE(
    COUNT(olist_order_reviews_dataset[review_score]),
    olist_order_reviews_dataset[review_score] <= 2
) / [total_reviews]
```

---

## 🔭 Limitations & Future Work

**Limitations**
- Review rates for very small categories are based on few reviews, so the top and bottom rankings can be misleading.
- The data has no cost or profit fields, so the analysis covers **revenue**, not profitability.
- The dataset covers 2016–2018 only, and 2016 has very few orders, so year-over-year comparison is most reliable for 2017 vs 2018.

**Future work**
- Delivery performance: actual vs estimated delivery time and its effect on review scores.
- Seller performance and geographic analysis of sales by state.
- Customer segmentation (RFM) and repeat-purchase behavior.
- Minimum-review threshold on category rankings.

---

## 🛠️ Tools & Skills

- **Power BI Desktop** — data modeling, interactive visuals, navigation buttons, slicers
- **Power Query (M)** — data cleaning and transformation
- **DAX** — KPI measures for revenue, orders, customers and reviews, plus the `DimDate` calculated table
- **Data modeling** — snowflake schema across orders, items, payments, customers, products, sellers, reviews and a DAX-built calendar table

**Skills demonstrated:** data cleaning and transformation · data modeling (snowflake schema) · DAX measures and calculated tables · dashboard design and storytelling · KPI definition · business insight and recommendation writing

## 📁 Repository Structure

```
Olist-Ecommerce-PowerBI/
│
├── data/                  # Raw CSV files (or a link to Kaggle if too large)
├── images/                # Dashboard screenshots used in this README
├── Olist_Dashboard.pbix   # Power BI report file
├── Olist_Dashboard.pdf    # PDF export of the dashboard
└── README.md
```

## 🚀 How to Use

1. Clone the repository:
   ```bash
   git clone https://github.com/enghassanwahba/Olist-Advanced-PowerBI-Pipeline-HW.git
   ```
2. Download the dataset from Kaggle and place the CSV files in the `data/` folder (if not included).
3. Open `Olist_Dashboard.pbix` in **Power BI Desktop**.
4. If prompted, update the data source path: **Transform data → Data source settings**.
5. Use the page buttons (Revenue · Customer · Reviews) and slicers to explore.

## 🙏 Acknowledgements

- Dataset: **Brazilian E-Commerce Public Dataset by Olist**, published on [Kaggle](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce). Please refer to the dataset page for its license and terms of use.
- Project completed as the final project of the **Data Analysis Diploma** at Smart Investment Solution (SIS)
- **Academic Advisor : Dr.Mohamed Sobhy**

## 📬 Contact

**Hassan Wahba** — Senior Electrical & Automation Engineer transitioning into Data Analytics & BI

- 💼 LinkedIn: [hassan-wahba](https://www.linkedin.com/in/hassan-wahba-a0777013a)
- 🐙 GitHub: [enghassanwahba](https://github.com/enghassanwahba)
- 📧 Email: eng_hassanwahbawork@gmail.com

---

⭐ Please If you found this project useful, feel free to star the repository!
