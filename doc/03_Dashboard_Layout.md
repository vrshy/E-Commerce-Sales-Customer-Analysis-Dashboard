# Dashboard build guide (Report view)

## Page 1: Sales Overview
| Visual | Fields |
|---|---|
| 5 KPI cards (top) | Total Sales, Total Profit, Profit Margin %, Total Orders, Avg Order Value |
| Line chart | Axis: Date[Year], Date[Month]; Values: Total Sales, Sales LY |
| Clustered bar | Axis: products[Category]; Values: Total Sales, Total Profit |
| Map / filled map | Location: customers[State]; Size: Total Sales |
| Slicers | Year, Category, Segment |

## Page 2: Product Performance
- Bar chart: Top 10 SubCategory by Total Profit (Visual filter > Top N)
- Scatter: X = Total Sales, Y = Profit Margin %, Details = SubCategory
- Table: ProductName, Units Sold, Total Sales, Profit Margin % (conditional formatting on margin)

## Page 3: Customer Insights
- Cards: Total Customers, Repeat Customer %
- Donut: Total Sales by Segment
- Bar: Top 10 customers by Total Sales
- Column: Total Customers by City

## Page 4: Orders and Payments
- Donut: Total Orders by PaymentMethod
- Column: Return Rate % and Cancel Rate % by Category
- Line: Total Orders by Month
- Card: Avg ShippingDays

## Design tips
- One theme: 2 colours + grey (View > Themes)
- KPI cards on top, charts below, slicers on the left
- Add page navigation buttons, and tooltips on charts
- Take screenshots of every page for your README
