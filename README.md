# 📊 E-Commerce Sales & Customer Analysis (Power BI)

An end-to-end Power BI project that turns raw, messy e-commerce CSV files into a 4-page interactive dashboard covering sales, product performance, customer behaviour, and order/payment operations.

**Tools:** Power BI Desktop · Power Query (M) · DAX · Data Modelling

---

## 🎯 Project Objective

To answer key business questions for an Indian e-commerce retailer:

- How much revenue and profit are we generating, and how is it trending year over year?
- Which categories, sub-categories, and products drive the most profit?
- Who are our best customers, and how loyal are they?
- How do payment methods, returns, cancellations, and shipping times look across categories?

---

## 📁 Dataset

Three CSV files form a star-style model:

| File | Description | Key columns |
|------|-------------|-------------|
| `customers.csv` | Customer master data | CustomerID, Segment, City, State, SignupDate |
| `products.csv` | Product catalogue | ProductID, ProductName, Category, SubCategory, UnitCost, UnitPrice |
| `orders.csv` | Transactions (the messy one) | OrderID, CustomerID, ProductID, OrderDate, ShipDate, Quantity, Discount, PaymentMethod, OrderStatus |

A separate **Date** table is created in DAX for time intelligence.

> **Note:** The dataset is synthetic and was created for practice purposes. It does not represent a real business.

---

## 🧹 Data Cleaning (Power Query)

| Problem in `orders` | Fix |
|---|---|
| Duplicate rows | Removed duplicates across all columns |
| OrderDate / ShipDate stored as text (`dd-mm-yyyy`) | Converted to Date using locale **English (India)** |
| Blank Discount values | Replaced null with `0`, set type to Decimal |
| Inconsistent PaymentMethod (` upi `, `UPI`, `upi`) | Trim → Capitalize Each Word → standardised `Upi` to `UPI` |
| Wrong data types | Quantity → Whole Number, Discount → Decimal |

Added a custom column **ShippingDays** = `Duration.Days([ShipDate] - [OrderDate])`.
`customers` and `products` had their data types verified and text columns trimmed.

Full steps: [`docs/01_PowerQuery_Steps.md`](docs/01_PowerQuery_Steps.md)

---

## 🔗 Data Model

All relationships are one-to-many with single-direction filtering:

```
customers[CustomerID] 1 ──► * orders[CustomerID]
products[ProductID]   1 ──► * orders[ProductID]
Date[Date]            1 ──► * orders[OrderDate]
```

---

## 🧮 DAX Measures

All measures live in a dedicated `_Measures` table, and cancelled orders are excluded from revenue metrics.

| Group | Measures |
|---|---|
| **Core KPIs** | Total Sales, Total Cost, Total Profit, Profit Margin %, Total Orders, Total Customers, Units Sold, Avg Order Value |
| **Time intelligence** | Sales LY, YoY Growth %, Sales YTD |
| **Order & customer insights** | Return Rate %, Cancel Rate %, Repeat Customers, Repeat Customer %, Payment Share % |

Example:

```dax
Total Sales =
SUMX (
    FILTER ( orders, orders[OrderStatus] <> "Cancelled" ),
    orders[Quantity] * RELATED ( products[UnitPrice] ) * ( 1 - orders[Discount] )
)
```

Full list: [`docs/02_DAX_Measures.md`](docs/02_DAX_Measures.md)

---

## 🖼️ Dashboard Pages

### 1. Sales Overview
KPI cards, monthly sales trend by year, sales & profit by category, sales by state, and slicers for Segment, Category and Year.

![Sales Overview](Screenshot/Sales%20Overview.png)

### 2. Product Performance
Top 10 sub-categories by profit, sales vs profit margin scatter, and a product-level detail table with conditional formatting on margin.

![Product Performance](Screenshot/Product%20Performance.png)

### 3. Customer Insights
Total and repeat customers, sales by segment, top 10 customers, and customers by city.

![Customer Insights](Screenshot/Customer%20Insights.png)

### 4. Orders & Payments
Orders by payment method, return and cancel rate by category, monthly order trend, and average shipping days.

![Orders & Payments](Screenshot/Orders%20%26%20Payments.png)

---

## 🔍 Key Insights

- **Revenue:** ₹13.66M total sales, ₹4.00M profit, at a **29.29% profit margin** across 3,319 orders (average order value ≈ ₹4.12K).
- **Categories:** Electronics leads with ~₹5.9M in sales and ~₹1.6M profit, followed by Fashion and Home & Kitchen. Books & Stationery is the smallest contributor.
- **Products:** Smartwatch, Sneakers, Cookware Set, and Headphones are the top profit-making sub-categories. Smartwatch has the highest sales but one of the lowest margins, while Sneakers and Cookware Set combine strong sales with high margins.
- **Geography:** Tamil Nadu (₹3.19M), Maharashtra (₹2.69M), and Karnataka (₹2.11M) are the top three states by sales.
- **Customers:** Consumer segment contributes **62.68%** of sales, Corporate 25.82%, Home Office 11.5%. 497 of 499 customers placed more than one order.
- **Payments:** UPI is the most-used payment method (35.46% of orders), followed by Credit Card (20.25%) and Debit Card (15.06%).
- **Operations:** Overall return rate is 7.0% and cancel rate 5.2%. Books & Stationery has the highest returns (9.4%) and cancellations (7.1%). Average shipping time is 3.98 days.

---

## 📂 Repository Structure

```
├── data/
│   ├── customers.csv
│   ├── products.csv
│   └── orders.csv
├── docs/
│   ├── 01_PowerQuery_Steps.md
│   ├── 02_DAX_Measures.md
│   └── 03_Dashboard_Layout.md
├── Screenshot/
│   ├── Sales Overview.png
│   ├── Product Performance.png
│   ├── Customer Insights.png
│   └── Orders & Payments.png
├── sales dashboard.pbix
└── README.md
```

---

## ▶️ How to Use

1. Clone the repository
   ```bash
   git clone https://github.com/vrshy/E-Commerce-Sales-Customer-Analysis.git
   ```
2. Open `sales dashboard.pbix` in **Power BI Desktop**.
3. If prompted, update the data source path: **Home → Transform Data → Data source settings** and point to the `data/` folder.
4. Click **Refresh** to reload the data.

---

## 🧠 Skills Demonstrated

- Data cleaning and transformation with Power Query
- Star-schema data modelling with relationships
- DAX: iterators, `CALCULATE`, `RELATED`, time intelligence, `DIVIDE`
- KPI design, slicers, and interactive visual storytelling
- Translating business questions into dashboard insights

---

## 👩‍💻 Author

**Varshini G**
B.E. Electrical & Electronics Engineering, transitioning into Data Science

- GitHub: [@vrshy](https://github.com/vrshy)
- LinkedIn: [linkedin.com/in/varshiganesan](https://www.linkedin.com/in/varshiganesan)
