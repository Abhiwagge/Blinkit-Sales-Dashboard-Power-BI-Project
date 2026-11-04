# Blinkit Power BI – Build Guide

## 1. Load & clean (Power Query)
1. Get Data > Excel > `BlinkIT_Grocery_Data.xlsx`. Rename column `Sales` to `Total Sales` if you want it to match the SQL doc.
2. Select `Item Fat Content` > Replace Values: `LF`→`Low Fat`, `low fat`→`Low Fat`, `reg`→`Regular` (5 labels become 2).
3. Types: `Item Weight`, `Item Visibility`, `Total Sales`, `Rating` = Decimal; `Outlet Establishment Year` = Whole number (or Text for the axis).
4. Item Weight has 1,463 blanks: leave as is (not used by any KPI).

## 2. DAX measures
```DAX
Total Sales   = SUM(Blinkit[Total Sales])
Total Sales (M) = DIVIDE([Total Sales], 1000000)
Avg Sales     = AVERAGE(Blinkit[Total Sales])
No of Items   = COUNTROWS(Blinkit)
Avg Rating    = AVERAGE(Blinkit[Rating])
% of Sales    = DIVIDE([Total Sales], CALCULATE([Total Sales], ALL(Blinkit)))
```

## 3. Page layout (16:9, Page background = background_kpi.png, Fit to page)
- Top row: 4 KPI cards (Total Sales, Avg Sales, No of Items, Avg Rating); put the icons from the Images folder beside each, card fill = transparent.
- Slicers: Outlet Location Type, Outlet Type, Item Fat Content.
- Donut: Total Sales by Item Fat Content (tooltip: Avg Sales, No of Items, Avg Rating).
- Bar: Total Sales by Item Type (sorted descending, add the same tooltip fields).
- Stacked column: Outlet Location Type x Item Fat Content, value = Total Sales.
- Line: Total Sales by Outlet Establishment Year.
- Donut/pie: % of Sales by Outlet Size.
- Filled map or funnel: Sales by Outlet Location Type.
- Matrix: Outlet Type with Total Sales, Avg Sales, No of Items, Avg Rating.

## 4. Data-quality notes
- Each outlet appears with several `Outlet Size` values (e.g. OUT010 is High, Medium and Small), so size looks assigned per row, not per outlet. Treat the size chart as indicative and flag this in your write-up.
- Outlet Establishment Year 2011 has 1 outlet only, so its lower sales are expected.
