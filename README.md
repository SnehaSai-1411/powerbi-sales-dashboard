# StyleHub Sales Dashboard — Power BI Project

An end-to-end Power BI dashboard for a fictional online clothing retailer.
Built to answer real business questions, not just look pretty.

## The business story
StyleHub sells apparel online. Leadership wants to know:
1. **Are we growing?** Revenue trend and year-over-year growth
2. **What makes money?** Which categories drive profit, not just revenue
3. **Who buys?** Which customer segments and regions are most valuable
4. **What should we do next?** Stock, discount, and marketing actions

## What's in this project
```
data/
  sales.csv       # 2,781 order lines, Jan 2024 – Dec 2025 (fact table)
  products.csv    # 40 products, 5 categories (dimension)
  customers.csv   # 500 customers, segments + regions (dimension)
  dates.csv       # calendar table for time intelligence
dax/
  measures.dax    # copy-paste DAX measures
guide/
  build-guide.md  # step-by-step: from install to finished dashboard
```

## Data model (star schema)
```
        dim_product      dim_customer      dim_date
              \                |                /
               \               |               /
                    fact_sales
```
- `sales[ProductID]` → `products[ProductID]`
- `sales[CustomerID]` → `customers[CustomerID]`
- `sales[OrderDate]` → `dates[Date]`

Revenue is **not** stored — you calculate it in DAX:
`Quantity × UnitPrice × (1 − Discount)`. That's deliberate: it teaches
measures vs calculated columns, which interviewers ask about.

## Dashboard pages
1. **Executive Overview** — KPI cards (Revenue, Profit, Orders, AOV), revenue
   trend by month, revenue by category, revenue by region, YoY growth
2. **Product Performance** — profit by category, top 10 products matrix,
   profit margin analysis (which categories look good on revenue but bleed margin?)
3. **Customer Insights** — revenue by segment, new vs returning, regional map

## Key findings to highlight (verify in your build)
- Holiday spike in Nov–Dec; 2025 outpacing 2024 (~25% growth baked into the data)
- Jackets look premium on revenue — check whether margin agrees
- VIPs are a small headcount but punch above their weight in revenue

## For your portfolio
- Export each page as PDF / take screenshots → put them in this README
- Publish to Power BI Service and add the link here
- The `guide/build-guide.md` doubles as your "how I built it" writeup
