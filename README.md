# 🛒 Olist E-Commerce Analytics — Power BI Dashboard

> **A 9-page end-to-end Power BI analytics solution** built on the Olist Brazilian E-Commerce public dataset — transforming raw transactional data into an executive-grade intelligence platform covering Sales, Customers, Delivery & Logistics, Reviews, Payments, and Seller performance.

<br>


> ⭐ **If this project helped you, please star the repository.**

<br>

---

## 🎯 Project Overview

| | |
|---|---|
| **Tool** | Microsoft Power BI Desktop |
| **Dataset** | Olist Brazilian E-Commerce Public Dataset |
| **Data Source** | `public bi_fact_sales` (PostgreSQL / public schema) |
| **Dashboard Pages** | **9 pages** — fully interactive |
| **Total Visuals** | **55 visuals** across all pages |
| **DAX Measure Tables** | **6 custom measure tables** |
| **Total DAX Measures** | **30+ measures** including Time Intelligence |
| **Interactivity** | Cross-filtering · Drill-Through · Year/Month Slicers · Action Buttons |

<br>

---

## 💡 Business Problem

Olist is a Brazilian e-commerce marketplace that connects small businesses to customers across the country. With thousands of sellers, millions of orders, and operations spanning all 26 states, the business generates enormous volumes of transactional, logistics, payment, and review data.

**Key business questions this dashboard answers:**

- How is total revenue trending year-over-year and month-over-month?
- Which product categories and states drive the most revenue?
- How fast are orders being delivered — and how many are arriving late?
- What do customer reviews reveal about product and delivery quality?
- Which sellers are top performers — and which states do they operate from?
- How are customers paying — and what does instalment behaviour look like?

<br>

---

## 📄 Dashboard Pages — All 9 Explained

### 1. 📊 Overview (Executive Dashboard)
The command centre of the entire report. All critical KPIs visible at once with cross-filtering across every visual.

**Visuals on this page:**
- KPI Cards: Total Revenue · Total Orders · Total Customers · Total Sellers · Average Rating · AOV
- Revenue Trend (Line Chart)
- Revenue vs Last Year (KPI Visual)
- Top 10 Categories (Bar Chart)
- Category Revenue Share (Donut Chart)
- Orders Status Share (Donut Chart)
- Revenue by State (Map Visual)

---

### 2. 📈 Sales Trend
Deep-dive into revenue patterns over time with year-on-year comparison and rolling averages.

**Visuals on this page:**
- Revenue Trend (Line / Area Chart)
- Revenue vs Last Year
- Revenue YTD (Year-to-Date)
- Orders by Month-Wise (Clustered Column Chart)
- Category X Month (Pivot Table)
- Year Slicer

---

### 3. 🛍️ Category & Product
Product category performance — revenue share, rating, and order volume by category.

**Visuals on this page:**
- Top 10 Categories by Revenue (Bar Chart)
- Top 20 Categories (Table Visual)
- Category Revenue Share (Donut Chart)
- AOV vs Orders by Category (Scatter Chart)
- Average Rating by Category (Bar Chart)
- Top 10 Categories by Rating (Bar Chart)
- Category Title (Dynamic Card)

---

### 4. 👥 Customer
Geographic and behavioural analysis of Olist's customer base.

**Visuals on this page:**
- Total Customers (KPI Card)
- Top 10 Customer States by Revenue (Bar Chart)
- Top 10 Customer Cities by Revenue (Bar Chart)
- Revenue by Customer State (Map Visual)
- Customer State × Category (Pivot Table)
- Revenue by State (Bar Chart)

---

### 5. 🚚 Delivery & Logistics
Operational performance — delivery speed, late order analysis, and on-time rates by state and month.

**Visuals on this page:**
- Average Delivery Days (Card)
- Late Orders (Card)
- % of On-Time Deliveries (Card)
- % of Late Orders by Customer State (Bar Chart)
- Late Orders by Month-Wise (Line Chart)
- Average Rating by Late vs On-Time (Bar Chart)
- Top 10 States by Late % (Bar Chart)

---

### 6. ⭐ Reviews
Customer satisfaction analysis — rating distribution, sentiment classification, and delivery impact on reviews.

**Visuals on this page:**
- Average Rating (KPI Card)
- % Positive Reviews (Card)
- % Negative Reviews (Card)
- Rating Distribution (Bar Chart / Donut)
- Review Bucket breakdown (Table)
- Top 10 Categories by Rating (Bar Chart)
- Average Rating by Category (Bar Chart)
- Average Rating by Late vs On-Time (Bar Chart)

---

### 7. 💳 Payments
Payment method preferences, instalment behaviour, and payment value trends over time.

**Visuals on this page:**
- Payment Types Share (Donut Chart)
- Average Installments by Payment Type (Bar Chart)
- Payment Value Trend (Area / Line Chart)
- Average Installments (Card)
- Payment type slicer

---

### 8. 🏪 Seller
Seller performance — revenue per seller, geographic concentration, and category specialisation.

**Visuals on this page:**
- Total Sellers (KPI Card)
- Revenue per Seller (Card)
- Top 10 Sellers by Revenue (Bar Chart)
- Revenue by Seller (Bar Chart)
- Seller State × Category (Pivot Table)
- Revenue by Seller State (Map Visual)

---

### 9. 🔍 Drill Through
A dedicated drill-through page accessible from any product category or state — shows full detail for the selected context with a dynamic title that updates based on selection.

**Visuals on this page:**
- Drill Title (Dynamic DAX measure-driven card)
- Full breakdown of the drilled entity
- Action Button (Back navigation)

<br>

---

## 📐 DAX Measures — Complete Reference

### Table 1: Basic Measures
```dax
Revenue =
  SUM( 'public bi_fact_sales'[payment_value] )

Orders =
  COUNTROWS( 'public bi_fact_sales' )

Customers =
  DISTINCTCOUNT( 'public bi_fact_sales'[customer_id] )

AOV =
  DIVIDE( [Revenue], [Orders], 0 )

Avg Rating =
  AVERAGE( 'public bi_fact_sales'[review_score] )

Late Orders =
  CALCULATE( [Orders], 'public bi_fact_sales'[is_late] = TRUE() )

On-time % =
  DIVIDE( [Orders] - [Late Orders], [Orders], 0 )
```

### Table 2: Time Measures (Time Intelligence)
```dax
Revenue LY =
  CALCULATE( [Revenue], SAMEPERIODLASTYEAR( DimDate[Date] ) )

YoY % =
  DIVIDE( [Revenue] - [Revenue LY], [Revenue LY], 0 )

Revenue YTD =
  TOTALYTD( [Revenue], DimDate[Date] )

Rolling 30D Revenue =
  CALCULATE(
    [Revenue],
    DATESINPERIOD( DimDate[Date], LASTDATE( DimDate[Date] ), -30, DAY )
  )
```

### Table 3: Delivery & Logistics Measures
```dax
Avg Delivery Days =
  AVERAGE( 'public bi_fact_sales'[delivery_days] )

Late % =
  DIVIDE( [Late Orders], [Orders], 0 )
```

### Table 4: Review Sentiment Measures
```dax
Positive Reviews % =
  DIVIDE(
    CALCULATE( [Orders], 'public bi_fact_sales'[review_score] >= 4 ),
    [Orders], 0
  )

Negative Reviews % =
  DIVIDE(
    CALCULATE( [Orders], 'public bi_fact_sales'[review_score] <= 2 ),
    [Orders], 0
  )
```

### Table 5: Payments Measures
```dax
Avg Installments =
  AVERAGE( 'public bi_fact_sales'[payment_installments] )
```

### Table 6: Drill Title
```dax
Drill Title =
  "Detail View — " & SELECTEDVALUE( 'public bi_fact_sales'[category], "All Categories" )
```

<br>

---

## 📊 Visual Inventory — All 55 Visuals

| Visual Type | Count | Used For |
|---|---|---|
| **Card** | 20 | All KPI metrics across every page |
| **Bar Chart** | 9 | Category, state, city, and seller comparisons |
| **Line Chart** | 4 | Revenue trend, late orders over time |
| **Clustered Column Chart** | 3 | Monthly order volumes, category breakdown |
| **Map** | 3 | Revenue by state, seller geography, customer location |
| **Donut Chart** | 3 | Category share, payment type share, order status |
| **Pivot Table** | 3 | Category × Month, Customer State × Category, Seller × Category |
| **Table (TableEx)** | 3 | Top 20 categories, review buckets, seller list |
| **KPI Visual** | 1 | Revenue vs Last Year with trend indicator |
| **Area Chart** | 1 | Payment value trend over time |
| **Scatter Chart** | 1 | AOV vs Orders by Category (quadrant analysis) |
| **Treemap** | 1 | Category or state revenue distribution |
| **Slicer** | 1 | Year / Year-Month time filter |
| **Action Button** | 1 | Back navigation on Drill Through page |
| **Total** | **55** | |

<br>

---

## 🗄️ Data Model

**Fact Table:** `public bi_fact_sales`

| Key Field | Description |
|---|---|
| `payment_value` | Order revenue |
| `customer_state` · `customer_city` | Customer geography |
| `seller_id` · `seller_state` | Seller details |
| `category` | Product category |
| `payment_type` | Payment method |
| `payment_installments` | Number of instalments |
| `order_status` | Order completion status |
| `is_late` | Boolean — whether delivery was late |
| `review_score` | Customer review (1–5) |
| `Review Bucket` | Calculated sentiment group |

**Dimension Table:** `DimDate`
- Used for all time intelligence measures (YoY, YTD, Rolling 30D, SAMEPERIODLASTYEAR)

<br>

---

## 🛠️ Tools & Technologies

| Tool | Purpose |
|---|---|
| **Power BI Desktop** | Dashboard design, data modelling, all visualisations |
| **DAX (Data Analysis Expressions)** | 30+ custom measures across 6 measure tables |
| **Power Query (M Language)** | Data transformation and preparation |
| **PostgreSQL** | Source database (`public bi_fact_sales` schema) |
| **Time Intelligence Functions** | SAMEPERIODLASTYEAR, TOTALYTD, DATESINPERIOD |
| **Map Visual** | Geographical analysis across Brazilian states |
| **Drill-Through** | Page-level context navigation |
| **GitHub** | Version control and portfolio hosting |

<br>

---

## 🗂️ Repository Structure

```
olist-ecommerce-powerbi-dashboard/
│
├── 📄 README.md                              ← You are here
├── 📊 Olist_Database.pbix                   ← Full Power BI report file
├── 📁 assets/
│   ├── 🖼️ dashboard_screenshot.png          ← Overview page preview
│   ├── 🖼️ sales_trend.png                   ← Sales Trend page
│   ├── 🖼️ category_product.png              ← Category page
│   ├── 🖼️ customer.png                      ← Customer page
│   ├── 🖼️ delivery_logistics.png            ← Delivery page
│   ├── 🖼️ reviews.png                       ← Reviews page
│   ├── 🖼️ payments.png                      ← Payments page
│   └── 🖼️ seller.png                        ← Seller page
└── 📁 dax/
    └── 📄 all_measures.dax                  ← All 30+ DAX measures
```

<br>

---

## 🚀 How to Open This Project

```bash
# Step 1 — Clone this repository
git clone https://github.com/bisht5431-source/olist-ecommerce-powerbi-dashboard.git

# Step 2 — Open Power BI Desktop
# Download free: https://powerbi.microsoft.com/desktop

# Step 3 — Open the file
# File → Open → Olist_Database.pbix

# Step 4 — Explore all 9 pages
# Use the page tabs at the bottom to navigate
# Use the Year slicer to filter by time period
# Right-click any category or state → Drill Through → Detail View
```

<br>

---

## 💼 What This Project Demonstrates

| Skill | Evidence |
|---|---|
| **Multi-page Dashboard Design** | 9 structured pages, each answering a distinct business question |
| **DAX — Time Intelligence** | YoY %, YTD, Rolling 30D Revenue, SAMEPERIODLASTYEAR |
| **DAX — Measure Architecture** | 6 organised measure tables — not a single flat list |
| **Data Modelling** | Date table, fact table, star schema relationships |
| **Geospatial Analysis** | 3 Map visuals — customer, seller, and revenue by state |
| **Drill-Through Navigation** | Dynamic title measure + back-button action |
| **Scatter Chart Analysis** | AOV vs Orders quadrant — category positioning |
| **Sentiment Analysis** | Review Bucket classification + positive/negative % |
| **Logistics Analytics** | Late order tracking, on-time %, delivery day averages |
| **Payment Behaviour** | Instalment patterns, payment type share, value trends |

<br>

---

## 📬 Connect With Me

| Platform | Link |
|---|---|
| 🔗 LinkedIn | [linkedin.com/in/dataanalyst-manish](https://www.linkedin.com/in/dataanalyst-manish) |
| 💻 GitHub | [github.com/bisht5431-source](https://github.com/bisht5431-source) |
| 📧 Email | alphainsights123@gmail.com |

<br>

---

<div align="center">

**⭐ Star this repository if it helped you — it helps other analysts find it.**

*Built by Manish Bisht — Data Analyst · SQL · Power BI · DAX · Python · Excel*

</div>
