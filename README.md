# Sales Analysis Dashboard

## Problem Statement

This dashboard helps the business understand their customers better. It helps the business know if they need to improve in any aspects. Through weekly and monthly sales trend analysis, they can know where and how to improve. It also lets them know the top selling products, evaluates the performance of different product categories.

Most sales were reported in the month of October-November. We can say that it must be a cyclic trend. 



### Steps followed 

- Step 1 : Load the sql files in MySQL and execute the queries to make database and tables.
- Step 2 : Open PwerBI and select ODBC from get data to connect the MySQL ODBC connector to import the data.
- Step 3 : Since we have lots of data, we can start by understanding the tables and columns.
- Step 4 : Starting with the 1st query "Identify top-selling products"
(a) Start by selecting the appropriate chart style, here I am using the funnel chart.
(b) Drag "product names" in category and "unit" in values.
(c) Apply Top N filter on product to display the top 10 products.

The Top 10 most selling Products are:
	(i)	Blue Razzleberry
	(ii)	CBN:THC Strawberry Melon
	(iii)	Test-Range
	(iv)	Kodo Pro
	(v)	Afghan Black Hash
	(vi)	Tiger Blood Distillate Infused Pre-roll
	(vii)	Blackberry Lemonade
	(viii)	Jungle Fruit 510 thread cartridge
	(ix)	Spark THC moonrocks
	(x)	Apricot Kush
- Step 5 : Now we will "Analyze sales trends over time (daily, weekly, monthly)".
For this we will need to seperate the Month name and week number.
We will use DAX function for this,
For Month name;
MonthName = FORMAT('order_items'[updated_at], "MMMM")
For Week number;
WeekNumber = WEEKNUM(order_items[updated_at])
For the Weekly and Monthly sale analysis,
We will select the "line and stacked column chart".
Drag "Week number" to X-axis, "units" to cloumn y-axis and "Month name" to line y-axis
For the "Daily Sales Analysis" chart, 
Drag the "updated_day" - day to X-axis and "units" to y-axis.

Week 39, day 25 and September month had the highest sale.
- Step 6 : Add a text box and give the title to the dashboard "Sales Analysis".
- Step 7 : To Evaluate the performance of different product categories;
Select Table from visualizations,
Drag "Product Categories" and "units" in the columns tab.
Top Product Categories are:
Flower
Pre Roll
Edibles
Infused Pre-roll
Accessories
Distillate Vapes
Beverages
Milled Flower
Live Resin Vapes
- Step 8 : To "Monitor inventory levels and turnover rates";
We need to create a new measure,"Inventory level"
	Inventory Level = SUM('products_pricing_inventory'[active_stock]) - SUM('products_pricing_inventory'[reserved_qty])
Select card view and drag the newly created measure "Inventory level" to it.
For Inventory Turnover Rates, we first need to calculate "COGS" and "Average Inventory"

Average Inventory = SUM('products_pricing_inventory'[active_stock]) / COUNTROWS(VALUES('products_pricing_inventory'[created_at (bins)]))

COGS = SUMX('products_pricing_inventory', 'products_pricing_inventory'[sale_price] * ('products_pricing_inventory'[active_stock] - 'products_pricing_inventory'[reserved_qty]))

Inventory Turnover Ratio = DIVIDE([COGS], [Average Inventory], 0)
Select card view and drag the newly created measure "Inventory Turnover Ratio" to it.

Inventory levels at current are: 166263 and the turnover rate is: 169.36
- Step 9 : Setting the Reordering Quantity.
calculate "Reordering Quantity" we need to have "Sales Velocity" and we can calcluate Sales Velocity by;
Sales Velocity = 
AVERAGEX(order_items, [unit])
alculating the Reorder amount.
Reorder Amount = 
([Sales Velocity] * 20) - SUM(products_pricing_inventory[active_stock])
To present Reorderinf amount select table view from visualizations and drag "Product Name" and "Reorder amount" to it.
- Step 10 : Identify products frequently purchased together (cross-selling), duplicate the "Products" table and conncet both with the column "Barcode"
To visualize this, select table from visualizations and drag "Products Name" to it.
and for the chart view for the prodcuts frequently bought together, 
select "Clustered Bar Chart" from visualizations and Drag "Product Name" to both X and Y-axis.
- Step 11 : Choose other informations to populate your dashboard like, "Business name", "Location", "Order Method", "Month" and "Total Products".
- Step 12 : Choose appropriate background colour and theme.
 
