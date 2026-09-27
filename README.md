StyleHub Sales Dashboard — Power BI Project
An end-to-end Power BI dashboard for StyleHub, a fictional online clothing
retailer. Built from scratch: synthetic data → star schema → DAX → three
report pages → business recommendations.
The business questions
Are we growing? Revenue trend, 2024 vs 2025
What actually makes money? Categories by profit and margin, not just revenue
Who buys? Customer segments, regions, top customers
What should we do next? Stock, pricing, and retention actions
Data model (star schema)
        products          customers           dates
     (40 products)      (500 customers)   (calendar 2024–2025)
              \                |                /
               \               |               /
                      sales (fact)
               2,781 lines · 1,600 orders
Three relationships, single cross-filter direction, dimensions on the 1 side:
products[ProductID] (1) → sales[ProductID] (*)
customers[CustomerID] (1) → sales[CustomerID] (*)
dates[Date] (1) → sales[OrderDate] (*)
Revenue is not stored in the data — it's calculated in DAX as
Quantity × UnitPrice × (1 − Discount). Deliberate: it forces the
measures-vs-calculated-columns distinction that interviewers ask about.
DAX measures (dax/measures.dax)
Measure
Definition
Total Revenue
SUMX(sales, Quantity × UnitPrice × (1 − Discount)) via RELATED(products[UnitPrice])
Total Cost
SUMX(sales, Quantity × RELATED(products[UnitCost]))
Total Profit
[Total Revenue] − [Total Cost]
Profit Margin
DIVIDE([Total Profit], [Total Revenue])
Total Orders
DISTINCTCOUNT(sales[OrderID])
Total Units
SUM(sales[Quantity])
Avg Order Value
DIVIDE([Total Revenue], [Total Orders])
Dashboard pages
Overview — KPI cards (Revenue, Profit, Orders, AOV), monthly revenue
   trend split by year, profit by category, revenue by region, Year + Category
   slicers with full cross-filtering
Products — Category × Product matrix (revenue, profit, margin, units),
   Top-10 products bar chart (Top-N filter)
Customers — revenue by segment, customer ranking table
Key findings
$308,734 revenue · $170,292 profit · 55.2% margin across 1,600 orders
  (2,781 line items). Avg order value $192.96.
2025 is outpacing 2024: 158.78K vs 149.95K revenue, 860 vs 740 orders.
  Holiday spike every Nov–Dec — seasonality is the strongest pattern in the data.
Jackets dominate: 120.1K revenue (39% of total), 66.6K profit, and the
  top 3 products overall (Leather Jacket, Puffer Coat, Parka) are all jackets.
But margin tells a different story: Blouses carry the highest category
  margin at 58.9%; Pants the lowest at 50.3%. Revenue leaders ≠ margin leaders.
Returning customers are the engine: ~140K revenue vs ~57K from new
  customers. Retention beats acquisition here.
South is the top region at 29.5% of revenue.
Top customer: Stella Garcia — 3,418.45 across 11 orders.
Data-quality catch
The customer ranking groups by CustomerName — but names aren't unique.
There are two different "Mia Taylor"s (C-0172 in the West, C-0430 in the
North) whose spending merges into a single row of ~$6,335, which would
wrongly crown her the #1 customer. Lesson: always key analysis on
CustomerID, never on names. (This is the kind of thing that impresses in
interviews — you found it, you can explain the fix.)
Recommendations
Protect jacket inventory before Q4 — stock out risk on the Leather
   Jacket / Puffer Coat / Parka going into the Nov–Dec spike.
Push Blouses harder — highest margin category (58.9%) deserves more
   marketing weight relative to its 36.7K revenue.
Retention program for Returning/VIP — they drive ~80% of revenue;
   a small lift in repeat purchase rate beats new-customer acquisition spend.
Investigate Pants margin (50.3%) — lowest in the catalog; check whether
   discounting or unit costs are the drag.
What's in this repo
data/
  sales.csv        # 2,781 order lines, Jan 2024 – Dec 2025 (fact table)
  products.csv     # 40 products, 5 categories (dimension)
  customers.csv    # 500 customers, segments + regions (dimension)
  dates.csv        # calendar table for time intelligence
dax/
  measures.dax     # all 7 measures, copy-paste ready
guide/
  build-guide.md   # full step-by-step: install → finished dashboard
screenshots/       # add your page exports here
How to open it
Open Power BI Desktop → Get data → Text/CSV → load the four CSVs
Model view: create the three relationships listed above
   (dimensions on the 1 side, single filter direction)
Add the measures from dax/measures.dax (Home → New measure)
Build the three pages, or open the .pbix if included
For recruiters
Built to demonstrate: star-schema modeling, DAX (row context with SUMX +
RELATED, DIVIDE for safe division), Top-N filtering, slicer
cross-filtering, date hierarchies done right (MonthName sorted by MonthNumber),
and — most importantly — turning a dashboard into recommended actions.
