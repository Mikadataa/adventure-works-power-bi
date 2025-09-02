# 📚 DAX Measures (Generated)

This document was generated from the uploaded ODS.
**Measures are grouped by functional area. Each entry includes the table, data type, and the exact DAX expression.**

> Generated from the uploaded ODS. Includes measure syntax, explanation, and practical use cases.

## Contents

- [Customers](#customers)
- [General / Utility](#general-utility)
- [Margin & Profitability](#margin-profitability)
- [Other](#other)
- [Planning / Targets](#planning-targets)
- [Purchases & Inventory](#purchases-inventory)
- [Sales (Actuals)](#sales-actuals)
- [Shipping & Delivery](#shipping-delivery)

## Customers

### 1. CustomerCount
**Table:** `MeasureTable`  
**Data type:** Integer
**DAX:**
```DAX
DISTINCTCOUNT(Sales[CustomerID])
```
**Explanation:** Distinct count of customers for reach/retention KPIs.
**Use case:** Customer base growth/retention; use in cohort or segmentation views.

## General / Utility

### 1. SelectedCurrency
**Table:** `dimCurrency`  
**Data type:** Text
**DAX:**
```DAX
Selected Currency:  & SELECTEDVALUE(dimCurrency[symbol])
```
**Explanation:** Helper showing the currency selected via slicer.
**Use case:** Dynamic currency switching; use to present KPIs in user‑selected currency.

### 42. Sales Multicurrency
**Table:** `MeasureTable`  
**Data type:** Integer
**DAX:**
```DAX
if(SELECTEDVALUE(dimCurrency[symbol]) = "$", [4. Sales], [41. Sales KZT])
```
**Explanation:** KPI derived from the underlying Sales/Plan/Operations model.
**Use case:** General reporting; include in KPI tiles, tables, and time‑series visuals.

### Max Date
**Table:** `Sales`  
**Data type:** Date
**DAX:**
```DAX
MAX('Calendar'[Date])
```
**Explanation:** KPI derived from the underlying Sales/Plan/Operations model.
**Use case:** General reporting; include in KPI tiles, tables, and time‑series visuals.

## Margin & Profitability

### 1. ActualCost
**Table:** `Operations`  
**Data type:** Integer
**DAX:**
```DAX
SUM(Operations[Actual Cost])
```
**Explanation:** KPI derived from the underlying Sales/Plan/Operations model.
**Use case:** General reporting; include in KPI tiles, tables, and time‑series visuals.

### 3. % ActualCost
**Table:** `Operations`  
**Data type:** Number
**DAX:**
```DAX
[1. ActualCost]/[2. PlannedCost]
```
**Explanation:** KPI derived from the underlying Sales/Plan/Operations model.
**Use case:** General reporting; include in KPI tiles, tables, and time‑series visuals.

### 4. NetProfit
**Table:** `Operations`  
**Data type:** Integer
**DAX:**
```DAX
[4. Sales]-[1. ActualCost]-[5. COGS]
```
**Explanation:** Net profit and its ratio considering sales, OPEX, and COGS.
**Use case:** General reporting; include in KPI tiles, tables, and time‑series visuals.

### 5. % Profit
**Table:** `Operations`  
**Data type:** Number
**DAX:**
```DAX
Operations[4. NetProfit]/[1.SalesTotal]
```
**Explanation:** Net profit and its ratio considering sales, OPEX, and COGS.
**Use case:** General reporting; include in KPI tiles, tables, and time‑series visuals.

### 6. Margin
**Table:** `MeasureTable`  
**Data type:** Integer
**DAX:**
```DAX
[4. Sales] - [5. COGS]
```
**Explanation:** Gross margin amount: Sales minus Cost of Goods Sold.
**Use case:** Profitability analysis by product/segment; use in waterfall or decomposition trees.

### 988. Margin What-If
**Table:** `MeasureTable`  
**Data type:** Number
**DAX:**
```DAX
[987. Sales What-If] - [5. COGS]
```
**Explanation:** Gross margin amount: Sales minus Cost of Goods Sold.
**Use case:** Profitability analysis by product/segment; use in waterfall or decomposition trees.

### Sales - OPEX
**Table:** `MeasureTable`  
**Data type:** Number
**DAX:**
```DAX
[1.SalesTotal]-SUM(OPEX[OPEX])
```
**Explanation:** KPI derived from the underlying Sales/Plan/Operations model.
**Use case:** Profitability analysis by product/segment; use in waterfall or decomposition trees.

## Other

### 5. COGS
**Table:** `MeasureTable`  
**Data type:** Integer
**DAX:**
```DAX
SUMX(Sales, Sales[Quantity] * Sales[Cost of goods])
```
**Explanation:** Cost of goods sold calculated from unit cost × quantity.
**Use case:** Profitability analysis by product/segment; use in waterfall or decomposition trees.

## Planning / Targets

### 1. SalesPlan
**Table:** `SalesPlan`  
**Data type:** Integer
**DAX:**
```DAX
SUM(SalesPlan[SalesPlan])
```
**Explanation:** Planned sales metrics for target vs actual comparisons.
**Use case:** Track performance versus monthly/annual plan; use on scorecards and pacing views.

### 2. PlannedCost
**Table:** `Operations`  
**Data type:** Integer
**DAX:**
```DAX
SUM(Operations[Planned Cost])
```
**Explanation:** KPI derived from the underlying Sales/Plan/Operations model.
**Use case:** Track performance versus monthly/annual plan; use on scorecards and pacing views.

### 4. AverageSalesPlan
**Table:** `SalesPlan`  
**Data type:** Number
**DAX:**
```DAX
AVERAGE(SalesPlan[SalesPlan])
```
**Explanation:** Planned sales metrics for target vs actual comparisons.
**Use case:** Track performance versus monthly/annual plan; use on scorecards and pacing views.

### 41. PreviousMonthSalesPlan
**Table:** `SalesPlan`  
**Data type:** Integer
**DAX:**
```DAX
CALCULATE([1. SalesPlan], PREVIOUSMONTH('Calendar'[Date]))
```
**Explanation:** Sales in the previous calendar month.
**Use case:** Track performance versus monthly/annual plan; use on scorecards and pacing views.

### 42. SalesPlanYTD
**Table:** `SalesPlan`  
**Data type:** Integer
**DAX:**
```DAX
CALCULATE([1. SalesPlan], DATESYTD('Calendar'[Date]))
```
**Explanation:** Year-to-date Sales using the Calendar date table.
**Use case:** Track performance versus monthly/annual plan; use on scorecards and pacing views.

### 43. SalesPlanFYTD
**Table:** `SalesPlan`  
**Data type:** Integer
**DAX:**
```DAX
CALCULATE([1. SalesPlan], DATESYTD('Calendar'[Date], "6-30"))
```
**Explanation:** Year-to-date Sales using the Calendar date table.
**Use case:** Track performance versus monthly/annual plan; use on scorecards and pacing views.

### 44. SalesPlanSPLY
**Table:** `SalesPlan`  
**Data type:** Integer
**DAX:**
```DAX
CALCULATE([1. SalesPlan], SAMEPERIODLASTYEAR('Calendar'[Date]))
```
**Explanation:** Same period last year comparison for Sales.
**Use case:** Track performance versus monthly/annual plan; use on scorecards and pacing views.

### 45. SalesPlan2MonthAgo
**Table:** `SalesPlan`  
**Data type:** Integer
**DAX:**
```DAX
CALCULATE([1. SalesPlan], DATEADD('Calendar'[Date], -2, MONTH))
```
**Explanation:** Planned sales metrics for target vs actual comparisons.
**Use case:** Track performance versus monthly/annual plan; use on scorecards and pacing views.

### 46. SalesPlan%
**Table:** `SalesPlan`  
**Data type:** Number
**DAX:**
```DAX
[4. Sales]/[1. SalesPlan]
```
**Explanation:** Plan achievement: Actual Sales ÷ Sales Plan.
**Use case:** Track performance versus monthly/annual plan; use on scorecards and pacing views.

### 47. SalesPlan KZT
**Table:** `SalesPlan`  
**Data type:** Integer
**DAX:**
```DAX
SUMX(SalesPlan, SalesPlan[1. SalesPlan]* RELATED('Calendar'[KZT]))
```
**Explanation:** Planned sales metrics for target vs actual comparisons.
**Use case:** Track performance versus monthly/annual plan; use on scorecards and pacing views.

### 48. SalesPlan Multicurrency
**Table:** `SalesPlan`  
**Data type:** Integer
**DAX:**
```DAX
IF(SELECTEDVALUE(dimCurrency[symbol]) = "$", [1. SalesPlan], [47. SalesPlan KZT])
```
**Explanation:** Planned sales metrics for target vs actual comparisons.
**Use case:** Track performance versus monthly/annual plan; use on scorecards and pacing views.

### 989. SalesPlanAch
**Table:** `MeasureTable`  
**Data type:** Number
**DAX:**
```DAX
DIVIDE(MeasureTable[1.SalesTotal],SalesPlan[1. SalesPlan],0)
```
**Explanation:** Planned sales metrics for target vs actual comparisons.
**Use case:** Track performance versus monthly/annual plan; use on scorecards and pacing views.

## Purchases & Inventory

### 1. Purchase Quantity
**Table:** `Purchases`  
**Data type:** Integer
**DAX:**
```DAX
SUM(Purchases[Quantity])
```
**Explanation:** KPI derived from the underlying Sales/Plan/Operations model.
**Use case:** Stock approximation where no real inventory table exists; use for on‑hand estimates.

### 2. Кол-во закуплено накопительно
**Table:** `Purchases`  
**Data type:** Integer
**DAX:**
```DAX
CALCULATE([1. Purchase Quantity], FILTER(ALL('Calendar'), 'Calendar'[Date] <= MAX('Calendar'[Date])))
```
**Explanation:** KPI derived from the underlying Sales/Plan/Operations model.
**Use case:** Stock approximation where no real inventory table exists; use for on‑hand estimates.

### 3. Остатки
**Table:** `Purchases`  
**Data type:** Integer
**DAX:**
```DAX
[2. Кол-во закуплено накопительно] - [985. CumulativeSales]
```
**Explanation:** KPI derived from the underlying Sales/Plan/Operations model.
**Use case:** Stock approximation where no real inventory table exists; use for on‑hand estimates.

## Sales (Actuals)

### 1.SalesTotal
**Table:** `MeasureTable`  
**Data type:** Integer
**DAX:**
```DAX
SUMX(Sales, Sales[Price $/each]*Sales[Quantity])
```
**Explanation:** KPI derived from the underlying Sales/Plan/Operations model.
**Use case:** General reporting; include in KPI tiles, tables, and time‑series visuals.

### 2. SalesQuantity
**Table:** `MeasureTable`  
**Data type:** Integer
**DAX:**
```DAX
SUM(Sales[Quantity])
```
**Explanation:** KPI derived from the underlying Sales/Plan/Operations model.
**Use case:** General reporting; include in KPI tiles, tables, and time‑series visuals.

### 4. Sales
**Table:** `MeasureTable`  
**Data type:** Integer
**DAX:**
```DAX
SUMX(Sales, Sales[Price $/each] * Sales[Quantity])
```
**Explanation:** KPI derived from the underlying Sales/Plan/Operations model.
**Use case:** General reporting; include in KPI tiles, tables, and time‑series visuals.

### 41. Sales KZT
**Table:** `MeasureTable`  
**Data type:** Integer
**DAX:**
```DAX
SUMX(sales, Sales[Price $/each] * Sales[Quantity] * RELATED('Calendar'[KZT]))
```
**Explanation:** KPI derived from the underlying Sales/Plan/Operations model.
**Use case:** Dynamic currency switching; use to present KPIs in user‑selected currency.

### 91. SalesPrevMonth
**Table:** `MeasureTable`  
**Data type:** Integer
**DAX:**
```DAX
CALCULATE([4. Sales], PREVIOUSMONTH('Calendar'[Date]))
```
**Explanation:** Sales in the previous calendar month.
**Use case:** General reporting; include in KPI tiles, tables, and time‑series visuals.

### 92. SalesSPLY
**Table:** `MeasureTable`  
**Data type:** Integer
**DAX:**
```DAX
CALCULATE([4. Sales], SAMEPERIODLASTYEAR('Calendar'[Date]))
```
**Explanation:** Same period last year comparison for Sales.
**Use case:** YoY/period-over-period benchmarking; add to variance visuals.

### 93. SalesYTD
**Table:** `MeasureTable`  
**Data type:** Integer
**DAX:**
```DAX
CALCULATE([4. Sales], DATESYTD('Calendar'[Date]))
```
**Explanation:** Year-to-date Sales using the Calendar date table.
**Use case:** Monitor cumulative performance within the year; use in trend charts with date slicers.

### 94. SalesFYTD
**Table:** `MeasureTable`  
**Data type:** Integer
**DAX:**
```DAX
CALCULATE([4. Sales], DATESYTD('Calendar'[Date], "6-30"))
```
**Explanation:** Year-to-date Sales using the Calendar date table.
**Use case:** Monitor cumulative performance within the year; use in trend charts with date slicers.

### 95. Sales2MonthAgo
**Table:** `MeasureTable`  
**Data type:** Integer
**DAX:**
```DAX
CALCULATE([4. Sales], DATEADD('Calendar'[Date], -2, MONTH))
```
**Explanation:** KPI derived from the underlying Sales/Plan/Operations model.
**Use case:** General reporting; include in KPI tiles, tables, and time‑series visuals.

### 97. Red Sales
**Table:** `MeasureTable`  
**Data type:** Integer
**DAX:**
```DAX
CALCULATE([4. Sales], FILTER(Products, Products[Color] = "Red"))
```
**Explanation:** KPI derived from the underlying Sales/Plan/Operations model.
**Use case:** General reporting; include in KPI tiles, tables, and time‑series visuals.

### 985. CumulativeSales
**Table:** `MeasureTable`  
**Data type:** Integer
**DAX:**
```DAX
CALCULATE([2. SalesQuantity], FILTER(ALL('Calendar'), 'Calendar'[Date] <= MAX('Calendar'[Date])))
```
**Explanation:** KPI derived from the underlying Sales/Plan/Operations model.
**Use case:** General reporting; include in KPI tiles, tables, and time‑series visuals.

### 987. Sales What-If
**Table:** `MeasureTable`  
**Data type:** Number
**DAX:**
```DAX
[4. Sales] * (1+[Sales Multiplier Value]/100)
```
**Explanation:** Scenario measure applying a percentage multiplier to Sales.
**Use case:** General reporting; include in KPI tiles, tables, and time‑series visuals.

### Sales Multiplier Value
**Table:** `Sales Multiplier`  
**Data type:** Integer
**DAX:**
```DAX
SELECTEDVALUE('Sales Multiplier'[Sales Multiplier])
```
**Explanation:** Scenario measure applying a percentage multiplier to Sales.
**Use case:** General reporting; include in KPI tiles, tables, and time‑series visuals.

## Shipping & Delivery

### 96. AVG deliverydays
**Table:** `MeasureTable`  
**Data type:** Number
**DAX:**
```DAX
AVERAGE(Sales[DeliveryDays])
```
**Explanation:** Average or max delivery time between order and shipment.
**Use case:** Fulfillment SLA monitoring; pair with order vs ship date toggles.

### 98. MAX deliverydays
**Table:** `MeasureTable`  
**Data type:** Integer
**DAX:**
```DAX
MAX(Sales[DeliveryDays])
```
**Explanation:** Average or max delivery time between order and shipment.
**Use case:** Fulfillment SLA monitoring; pair with order vs ship date toggles.

### 983. Shipped Quantity
**Table:** `MeasureTable`  
**Data type:** Integer
**DAX:**
```DAX
CALCULATE([2. SalesQuantity], USERELATIONSHIP('Calendar'[Date], Sales[Shipment date]))
```
**Explanation:** Shipment-related KPI using USERELATIONSHIP to shipment date.
**Use case:** Fulfillment SLA monitoring; pair with order vs ship date toggles.

### 984. % Shipped
**Table:** `MeasureTable`  
**Data type:** Number
**DAX:**
```DAX
DIVIDE([983. Shipped Quantity], [2. SalesQuantity], 0)
```
**Explanation:** Shipment-related KPI using USERELATIONSHIP to shipment date.
**Use case:** Fulfillment SLA monitoring; pair with order vs ship date toggles.
