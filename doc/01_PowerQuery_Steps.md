# Step 1: Load and clean data (Power Query)

Home > Get Data > Text/CSV > load `customers.csv`, `products.csv`, `orders.csv` > **Transform Data**.

## orders table (the messy one)
| # | Problem | Fix in Power Query |
|---|---------|--------------------|
| 1 | Duplicate rows | Select all columns > Remove Rows > **Remove Duplicates** |
| 2 | OrderDate / ShipDate are text (dd-mm-yyyy) | Column > Data Type > Using Locale > Date > English (India) |
| 3 | Blank Discount | Right click > Replace Values (null) with `0`, then set type to Decimal |
| 4 | PaymentMethod has ` upi `, `UPI`, `upi` | Transform > Format > **Trim**, then **Capitalize Each Word** |
| 5 | "Upi" now wrong, "Cash On Delivery" | Replace `Upi` with `UPI` |
| 6 | Wrong types | Quantity = Whole Number, Discount = Decimal |

Optional custom column for delivery days:
`Duration.Days([ShipDate] - [OrderDate])` named **ShippingDays**

## customers / products
- Check types (SignupDate = Date, UnitCost and UnitPrice = Decimal)
- Trim text columns

## Finally
Close & Apply.

# Step 2: Data model
Go to Model view and create relationships (all one-to-many, single direction):
- `customers[CustomerID]` 1 → * `orders[CustomerID]`
- `products[ProductID]` 1 → * `orders[ProductID]`
- `Date[Date]` 1 → * `orders[OrderDate]` (after creating the Date table, see DAX file)
