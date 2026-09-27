# Build Guide — StyleHub Sales Dashboard, from zero

Follow this top to bottom. Each step says what to click.

## Step 0: Install Power BI Desktop
1. Go to microsoft.com and download **Power BI Desktop** (free, Windows only).
2. Install and open it. Sign in with any Microsoft account (needed later to publish).

## Step 1: Load the data
1. Home tab → **Get Data** → **Text/CSV**.
2. Load these four files from the `data/` folder, one at a time:
   `sales.csv`, `products.csv`, `customers.csv`, `dates.csv`.
3. In the Navigator preview, click **Load** for each.

## Step 2: Build the star schema (Model view)
1. Click the **Model view** icon (third icon on the left rail).
2. Drag to create these relationships (all single direction, fact → dimension):
   - `sales[ProductID]` → `products[ProductID]`
   - `sales[CustomerID]` → `customers[CustomerID]`
   - `sales[OrderDate]` → `dates[Date]`
3. Check: you should see `sales` in the middle with three tables around it.
4. Right-click each key column (`ProductID`, `CustomerID` in `sales`) → **Hide**.
   Keep the model clean — report builders should use dimension columns.

## Step 3: Add the DAX measures
1. Open `dax/measures.dax` in Notepad.
2. In Power BI, go to **Home → Enter Data**? No — instead: select the
   `sales` table, then **Modeling tab → New measure**.
3. Paste the `Total Revenue` measure, press Enter. Repeat for every measure
   in the file, one at a time.
4. Select all new measures in the Fields pane → set format to **Currency**
   (or Percentage for margins).

Key idea: `SUMX` loops row by row — that's why revenue works even though
no revenue column exists in the CSV. Interviewers love this question.

## Step 4: Page 1 — Executive Overview
Rename Page 1 to "Overview". Add:
1. **4 KPI cards** (Visualizations → Card): Total Revenue, Total Profit,
   Total Orders, Avg Order Value.
2. **Line chart**: Axis = `dates[YearMonth]`, Values = Total Revenue.
   Add `dates[Year]` as legend to compare 2024 vs 2025.
3. **Bar chart**: Y = `products[Category]`, X = Total Profit (sort descending —
   this reveals margin truth vs revenue).
4. **Donut**: Legend = `customers[Region]`, Values = Total Revenue.
5. **Slicers** at top: `dates[Year]`, `products[Category]`. Set them to
   single-select off so users can multi-select.

## Step 5: Page 2 — Product Performance
Rename Page 2 to "Products". Add:
1. **Matrix**: Rows = Category → ProductName, Values = Total Revenue,
   Total Profit, Profit Margin.
2. **Bar chart**: Top 10 products by Total Profit (use Filters → Top N).
3. Write one insight in a **Text box**, e.g. which category has the
   best margin vs the most revenue.

## Step 6: Page 3 — Customer Insights
Rename Page 3 to "Customers". Add:
1. **Bar chart**: `customers[Segment]` by Total Revenue and by Revenue
   per Customer (two measures — VIPs should stand out).
2. **Map or filled map**: Location = `customers[Region]`, Values = Total Orders.
3. **Line chart**: new customers per month — use `COUNTROWS(customers)`
   with `customers[JoinDate]` on the axis (create a simple measure).

## Step 7: Make it look professional
1. View tab → try the built-in themes; pick one and stick to it.
2. Align everything (select multiple visuals → Format → Align).
3. Give every visual a title. No visual without a title.
4. Add a text box on each page with the **one business takeaway**.

## Step 8: Portfolio it
1. File → Export → **Export to PDF** (or screenshot each page).
2. In Power BI Service: **Publish** → get a share link → add it to the README.
3. Push the project folder (data + dax + guide + screenshots) to GitHub.

## If you get stuck
- Relationship won't create? Check for duplicate IDs in the dimension tables.
- Measure shows blank? Usually a broken relationship — go back to Model view.
- SAMEPERIODLASTYEAR errors? The `dates` table must be marked as a date table:
  right-click `dates` → **Mark as date table** → choose the Date column.
