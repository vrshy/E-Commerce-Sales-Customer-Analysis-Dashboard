# DAX: create in a new table called "_Measures" (Home > Enter Data > name it > Load)

## Date table (Modeling > New Table)
```dax
Date =
ADDCOLUMNS (
    CALENDAR ( DATE ( 2023, 1, 1 ), DATE ( 2025, 12, 31 ) ),
    "Year", YEAR ( [Date] ),
    "Month No", MONTH ( [Date] ),
    "Month", FORMAT ( [Date], "MMM" ),
    "Quarter", "Q" & QUARTER ( [Date] )
)
```
Then: select `Month` column > Column tools > Sort by column > `Month No`. Mark as Date Table.

## Core KPIs
```dax
Total Sales =
SUMX (
    FILTER ( orders, orders[OrderStatus] <> "Cancelled" ),
    orders[Quantity] * RELATED ( products[UnitPrice] ) * ( 1 - orders[Discount] )
)

Total Cost =
SUMX (
    FILTER ( orders, orders[OrderStatus] <> "Cancelled" ),
    orders[Quantity] * RELATED ( products[UnitCost] )
)

Total Profit = [Total Sales] - [Total Cost]

Profit Margin % = DIVIDE ( [Total Profit], [Total Sales] )

Total Orders =
CALCULATE ( DISTINCTCOUNT ( orders[OrderID] ), orders[OrderStatus] <> "Cancelled" )

Total Customers =
CALCULATE ( DISTINCTCOUNT ( orders[CustomerID] ), orders[OrderStatus] <> "Cancelled" )

Units Sold =
CALCULATE ( SUM ( orders[Quantity] ), orders[OrderStatus] <> "Cancelled" )

Avg Order Value = DIVIDE ( [Total Sales], [Total Orders] )
```

## Time intelligence
```dax
Sales LY = CALCULATE ( [Total Sales], SAMEPERIODLASTYEAR ( 'Date'[Date] ) )

YoY Growth % = DIVIDE ( [Total Sales] - [Sales LY], [Sales LY] )

Sales YTD = TOTALYTD ( [Total Sales], 'Date'[Date] )
```

## Order and customer insights
```dax
Return Rate % =
DIVIDE (
    CALCULATE ( DISTINCTCOUNT ( orders[OrderID] ), orders[OrderStatus] = "Returned" ),
    DISTINCTCOUNT ( orders[OrderID] )
)

Cancel Rate % =
DIVIDE (
    CALCULATE ( DISTINCTCOUNT ( orders[OrderID] ), orders[OrderStatus] = "Cancelled" ),
    DISTINCTCOUNT ( orders[OrderID] )
)

Repeat Customers =
COUNTROWS (
    FILTER (
        VALUES ( orders[CustomerID] ),
        CALCULATE ( DISTINCTCOUNT ( orders[OrderID] ) ) > 1
    )
)

Repeat Customer % = DIVIDE ( [Repeat Customers], [Total Customers] )

Payment Share % =
DIVIDE ( [Total Orders], CALCULATE ( [Total Orders], ALL ( orders[PaymentMethod] ) ) )
```
