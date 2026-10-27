
#  DAX-DATA-VISUALIZATION

##  Power BI Sales Data Analysis and Visualization

This project focuses on analyzing sales data using **Microsoft Power BI** and **DAX (Data Analysis Expressions)**.

The project includes data modeling, calculated columns, DAX measures, and interactive dashboards to analyze sales performance, profit margins, order counts, revenue, and sales targets.

---

##  Tools and Technologies Used

- Microsoft Power BI Desktop
- DAX (Data Analysis Expressions)
- Power Query
- Data Modeling
- Data Visualization
- Microsoft Excel (Dataset)

---

##  Dataset Description

The project uses the following tables:

### 1. List of Orders

Contains order-level information:

- Order ID
- Order Date
- Customer Name
- City
- State

### 2. Order Details

Contains product-level sales information:

- Order ID
- Amount
- Profit
- Quantity
- Category
- Sub Category

### 3. Sales Target

Contains sales target information for different product categories.

---

#  1. Data Modeling

## Creating Relationships Between Tables

Before performing calculations, relationships were established between the tables.

### Steps Followed

1. Imported the datasets into Power BI.
2. Opened **Model View**.
3. Selected the `Order ID` column in both tables.
4. Created a relationship between:
   - `List of Orders (1)[Order ID]`
   - `Order Details (1)[Order ID]`
5. Set cardinality to **Many-to-One (*:1)** from Order Details to List of Orders.
6. Selected **Single** cross-filter direction.
7. Enabled **Make this relationship active**.
8. Clicked **Save**.


---

## Question 1: Create a Calculated Column for Category Type

### Objective

Create a new column named `Category Type` by combining the `Category` and `Sub Category` columns.

### DAX Formula

```dax
Category Type =
'Order Details (1)'[Category]
& ","
& 'Order Details (1)'[Sub Category]
```

### Steps Followed

1. Opened **Data View**.
2. Selected the **Order Details (1)** table.
3. Clicked **New Column**.
4. Entered the DAX formula.
5. Pressed Enter.
6. Verified the generated category combinations.

### Example Output

| Category | Sub Category | Category Type |
|---|---|---|
| Electronics | Phones | Electronics,Phones |
| Clothing | Shirt | Clothing,Shirt |
| Furniture | Chairs | Furniture,Chairs |

### Result

Successfully created a new column combining the main category and sub-category.

---

## Question 2: Calculate Revenue per Order

### Objective

Calculate the revenue for each order-detail record by multiplying Amount and Quantity.

### DAX Formula

```dax
Revenue =
'Order Details (1)'[Amount]
*
'Order Details (1)'[Quantity]
```

### Steps Followed

1. Selected the **Order Details (1)** table.
2. Clicked **New Column**.
3. Entered the revenue formula.
4. Pressed Enter.
5. Verified the calculated revenue values.

### Example Output

| Amount | Quantity | Revenue |
|---|---|---|
| 168 | 2 | 336 |
| 12 | 2 | 24 |
| 65 | 2 | 130 |

### Result

Successfully calculated revenue for each order-detail record.

---

## Question 3: Categorize Sales as Above Average or Below Average

### Objective

Create a calculated column named `Sales Category` to classify sales based on the average Amount.

### DAX Formula

```dax
Sales Category =
IF(
    'Order Details (1)'[Amount] >
        AVERAGE('Order Details (1)'[Amount]),
    "Above Average",
    "Below Average"
)
```

### Steps Followed

1. Opened **Data View**.
2. Selected the **Order Details (1)** table.
3. Clicked **New Column**.
4. Entered the DAX formula using the `IF()` and `AVERAGE()` functions.
5. Pressed Enter.
6. Verified the category assigned to each record.

### Explanation

- **Above Average:** Amount is greater than the overall average.
- **Below Average:** Amount is less than or equal to the overall average.

### Result

Successfully categorized sales based on the average Amount.

---

#  3. Calculated Measures Using DAX

## Question 4: Calculate Total Order Count

### Objective

Calculate the total number of unique orders.

### DAX Formula

```dax
Total Orders =
DISTINCTCOUNT(
    'Order Details (1)'[Order ID]
)
```

### Steps Followed

1. Selected the **Order Details (1)** table.
2. Clicked **Modeling → New Measure**.
3. Entered the DAX formula.
4. Selected the **Card Visual**.
5. Dragged the `Total Orders` measure into the card.





### Output

**Total Orders = 500**

### Result

Successfully calculated and displayed the total number of distinct orders.

---

## Question 5: Calculate Average Profit in Delhi

### Objective

Calculate the average profit generated from orders placed in Delhi.

### DAX Formula

```dax
Average Profit in Delhi =
CALCULATE(
    AVERAGE('Order Details (1)'[Profit]),
    'List of Orders (1)'[City] = "Delhi"
)
```

### Steps Followed

1. Selected the **Order Details (1)** table.
2. Clicked **New Measure**.
3. Used the `CALCULATE()` function to filter records for Delhi.
4. Used the `AVERAGE()` function to calculate average profit.
5. Added a Card visual.
6. Dragged the measure into the card.

### Output

**Average Profit in Delhi = 43.31**

### Result

Successfully calculated the average profit per order-detail record for Delhi.

---

## Question 6: Calculate Cumulative Sales and Year-to-Date Sales

### Objective

Calculate accumulated sales over time using DAX.

### Step 1: Create Total Sales Measure

```dax
TOTAL SALES =
SUM('Order Details (1)'[Amount])
```

### Step 2: Create Cumulative Sales Measure

```dax
CUMILATIVE SALES RESULT =
CALCULATE(
    [TOTAL SALES],
    FILTER(
        ALL('List of Orders (1)'[Order Date]),
        'List of Orders (1)'[Order Date]
            <= MAX('List of Orders (1)'[Order Date])
    )
)
```

### Step 3: Optional True YTD Measure

For a year-to-date calculation that resets at the start of each year, create a calendar table:

```dax
Calendar =
CALENDAR(
    MIN('List of Orders (1)'[Order Date]),
    MAX('List of Orders (1)'[Order Date])
)
```

Create an active relationship between `Calendar[Date]` and `List of Orders (1)[Order Date]`, then use:

```dax
YTD Sales =
TOTALYTD(
    [TOTAL SALES],
    'Calendar'[Date]
)
```

### Steps Followed

1. Created the `TOTAL SALES` measure.
2. Created the `CUMILATIVE SALES RESULT` measure.
3. Selected a **Line Chart**.
4. Added `Order Date` to the X-axis.
5. Added `CUMILATIVE SALES RESULT` to the Y-axis.
6. Sorted the order dates chronologically.
7. Analyzed the cumulative sales trend.



### Result

Successfully visualized the cumulative sales trend over time.

**Note:** Cumulative sales accumulate across the entire selected date history. True YTD sales restart at the beginning of each year.

---

#  4. Data Visualization Using Power BI

## Question 7: Sales Target Achievement by Category

### Objective

Compare actual sales with sales targets for different product categories.

### DAX Formula

```dax
TOTAL SALES =
SUM('Order Details (1)'[Amount])
```

Example target measure, assuming the sales-target table has a numeric column called `Target`:

```dax
Sales Target =
SUM('sales target (1)'[Target])
```

Replace `Target` with the actual column name in your dataset.

### Steps Followed

1. Selected **Clustered Column Chart**.
2. Added `Category` to the X-axis.
3. Added `TOTAL SALES` to the Y-axis.
4. Added `Sales Target` to the Y-axis.
5. Verified that category filters correctly apply to both measures.
6. Formatted the chart using separate colors.
7. Compared actual sales with target values.



### Result

Visualized total sales and sales targets across Electronics, Furniture, and Clothing categories.

---

## Question 8: Maximum Profit Margin by Sub-Category

### Objective

Analyze the maximum profit margin across different product sub-categories.

### DAX Formula

```dax
Max Profit Margin =
MAXX(
    'Order Details (1)',
    DIVIDE(
        'Order Details (1)'[Profit],
        'Order Details (1)'[Amount]
    )
)
```

The measure calculates maximum row-level Profit divided by Amount.

### Steps Followed

1. Created the `Max Profit Margin` measure.
2. Formatted the measure as Percentage.
3. Selected a **Donut Chart**.
4. Added `Sub Category` to Legend.
5. Added `Max Profit Margin` to Values.
6. Enabled data labels.
7. Analyzed the profit margins across sub-categories.



### Result

Compared maximum profit margin values for different product sub-categories.

---

## Question 9: Monthly Sales Trend

### Objective

Analyze how total sales change over different months.

### DAX Formula

```dax
TOTAL SALES =
SUM('Order Details (1)'[Amount])
```

### Steps Followed

1. Selected a **Line Chart**.
2. Added `Order Date` to the X-axis.
3. Used the month-level date hierarchy or a properly sorted month-start field.
4. Added `TOTAL SALES` to the Y-axis.
5. Sorted the dates chronologically.
6. Analyzed monthly sales trends and fluctuations.


### Result

Successfully visualized changes in sales performance over time.

---

## Question 10: Comparison of Profit and Quantity by Sub-Category

### Objective

Compare total profit and quantity sold across different product sub-categories.

### DAX Formulas

```dax
Total Profit =
SUM('Order Details (1)'[Profit])
```

```dax
Total Quantity =
SUM('Order Details (1)'[Quantity])
```

### Steps Followed

1. Created the `Total Profit` measure.
2. Created the `Total Quantity` measure.
3. Selected a **Scatter Chart**.
4. Added `Total Quantity` to the X-axis.
5. Added `Total Profit` to the Y-axis.
6. Added `Sub Category` to Details or the appropriate category-identifying field.
7. Used Sub Category as the legend when required.
8. Analyzed the relationship between quantity and profit.



### Result

Compared product sub-categories based on their total quantity sold and profitability.

---

## Question 11: Comparison of Total Sales and Sales Target Using Cards

### Objective

Display important sales metrics using KPI cards for quick analysis.

### DAX Measures

```dax
TOTAL SALES =
SUM('Order Details (1)'[Amount])
```

```dax
Sales Target =
SUM('sales target (1)'[Target])
```

```dax
Sales Achievement % =
DIVIDE(
    [TOTAL SALES],
    [Sales Target],
    0
)
```

### Steps Followed

1. Selected a **Card Visual**.
2. Added `TOTAL SALES` to the first card.
3. Added `Sales Target` to the second card.
4. Created an optional third card for `Sales Achievement %`.
5. Formatted the cards for clear KPI comparison.

### Minimum Target Measure

```dax
Minimum Target =
MIN('sales target (1)'[Target])
```

### Multi-Row Card Steps

1. Selected the Multi-row Card or Table visual.
2. Added the segment field, if available in the dataset.
3. Added `Minimum Target` to Values.
4. Formatted the visual to display minimum targets for each segment.

### Result

Provided steps to create quick KPI comparisons between actual sales and sales targets.

**Note:** The uploaded screenshots show the Total Orders and Average Profit cards, but do not include screenshots of dedicated Total Sales, Sales Target, or Minimum Target cards.

---

## Question 12: Sales Performance Matrix

### Objective

Analyze actual sales and sales targets across different categories and months.

### DAX Measures

```dax
TOTAL SALES =
SUM('Order Details (1)'[Amount])
```

```dax
Sales Target =
SUM('sales target (1)'[Target])
```

### Steps Followed

1. Selected the **Matrix Visual**.
2. Added `Category` to Rows.
3. Added `Order Date` at month level to Columns.
4. Added `TOTAL SALES` to Values.
5. Added `Sales Target` to Values.
6. Enabled totals and subtotals.
7. Compared sales performance across categories and months.



### Result

Created a matrix for comparing total sales and sales targets by category and month.

**Note:** For accurate monthly comparisons, sales targets should also be defined at the appropriate monthly level. A category-only target can otherwise repeat across multiple months.

---

## Question 13: Geographic Sales Analysis

### Objective

Analyze sales performance across different cities.

### DAX Formula

```dax
TOTAL SALES =
SUM('Order Details (1)'[Amount])
```

### Steps Followed

1. Selected the **Map / Azure Maps Visual**.
2. Added `City` to Location.
3. Added `TOTAL SALES` to Bubble Size.
4. Added `State` to Tooltips.
5. Added `TOTAL SALES` to Tooltips.
6. Checked city locations to avoid incorrect geographic matches.
7. Analyzed the cities generating higher sales.

### Result

Provided the steps for geographic sales analysis using a map visual.

**Screenshot Status:** A map screenshot was not provided in the uploaded images.

---

## Question 14: Sales Distribution by Sub-Category

### Objective

Analyze how total sales are distributed across different product sub-categories.

### DAX Formula

```dax
TOTAL SALES =
SUM('Order Details (1)'[Amount])
```

### Steps Followed

1. Selected the **Treemap Visual**.
2. Added `Sub Category` to Group.
3. Added `TOTAL SALES` to Values.
4. Enabled data labels.
5. Formatted the treemap.
6. Compared the relative contribution of each sub-category to total sales.

### Result

Provided the steps to visualize the sales contribution of different sub-categories.

**Screenshot Status:** A treemap screenshot was not provided in the uploaded images.

---

## Question 15: Order Count Analysis by State

### Objective

Analyze the distribution of orders across different states using a funnel chart.

### DAX Formula

```dax
Order Count By State =
DISTINCTCOUNT(
    'List of Orders (1)'[Order ID]
)
```

### Steps Followed

1. Selected the **Funnel Chart**.
2. Added `State` to Category.
3. Added `Order Count By State` to Values.
4. Sorted the chart by order count in descending order.
5. Enabled data labels.
6. Compared the distribution of orders across states.

### 📸 Funnel Chart Visualization

![Order Count by State](assets/09b-state-funnel-detail.png)

### Result

Successfully visualized state-wise order counts.

From the provided dashboard:

- Madhya Pradesh: 101 orders
- Maharashtra: 90 orders
- Rajasthan: 32 orders
- Gujarat: 27 orders
- Punjab: 25 orders

Madhya Pradesh and Maharashtra have the highest order counts in the displayed funnel.



#  5. Key Insights

Based on the provided Power BI screenshots:

- **Total Orders:** 500 unique orders.
- **Average Profit in Delhi:** 43.31 per order-detail record.
- **Sales Performance:** Electronics shows higher total sales than Furniture and Clothing in the displayed category chart.
- **Cumulative Sales:** The cumulative sales chart shows growth over time.
- **Profitability:** Profit margins vary across product sub-categories.
- **State-Wise Orders:** Madhya Pradesh and Maharashtra have the highest order counts in the displayed funnel.
- **Monthly Sales:** The monthly trend shows periods of rising and falling sales.
- **Profit vs Quantity:** Higher quantities do not always correspond to higher profits.

---

#  6. Project Learning Outcomes

Through this project, I gained practical experience in:

✅ Importing and transforming datasets in Power BI.

✅ Creating relationships between multiple tables.

✅ Building calculated columns using DAX.

✅ Creating DAX measures with functions such as:

- `CALCULATE()`
- `SUM()`
- `AVERAGE()`
- `DISTINCTCOUNT()`
- `FILTER()`
- `ALL()`
- `MAX()`
- `MAXX()`
- `IF()`
- `DIVIDE()`
- `TOTALYTD()`

✅ Creating interactive sales dashboards.

✅ Comparing actual sales against sales targets.

✅ Analyzing cumulative sales, profitability, and revenue.

✅ Visualizing geographic and category-wise sales.

✅ Developing data analysis and business intelligence skills.





#  Conclusion

This project demonstrates the practical application of **Power BI and DAX** to analyze sales data.

By combining calculated columns, DAX measures, data modeling, and visualization techniques, the project provides insights into:

- Sales performance
- Order distribution
- Profitability
- Revenue analysis
- Sales target achievement
- Monthly sales trends
- Regional sales performance



