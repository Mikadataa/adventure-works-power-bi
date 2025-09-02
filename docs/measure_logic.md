# 🧠 Measure Logic – Business Intent

This document explains the **business meaning, purpose, and usage** behind each key KPI/measure in the model.

---

## 1. CustomerCount

**Group:** Customers  
**Table:** `MeasureTable`

**Business Definition:** Distinct count of customers for reach/retention KPIs.
**Purpose / Why it matters:** Customer base growth/retention; use in cohort or segmentation views.

**Technical Note:**
```DAX
DISTINCTCOUNT(Sales[CustomerID])
```
---

## 1. SelectedCurrency

**Group:** General / Utility  
**Table:** `dimCurrency`

**Business Definition:** Helper showing the currency selected via slicer.
**Purpose / Why it matters:** Dynamic currency switching; use to present KPIs in user‑selected currency.

**Technical Note:**
```DAX
Selected Currency:  & SELECTEDVALUE(dimCurrency[symbol])
```
---

## 42. Sales Multicurrency

**Group:** General / Utility  
**Table:** `MeasureTable`

**Business Definition:** KPI derived from the underlying Sales/Plan/Operations model.
**Purpose / Why it matters:** General reporting; include in KPI tiles, tables, and time‑series visuals.

**Technical Note:**
```DAX
if(SELECTEDVALUE(dimCurrency[symbol]) = "$", [4. Sales], [41. Sales KZT])
```
---

## Max Date

**Group:** General / Utility  
**Table:** `Sales`

**Business Definition:** KPI derived from the underlying Sales/Plan/Operations model.
**Purpose / Why it matters:** General reporting; include in KPI tiles, tables, and time‑series visuals.

**Technical Note:**
```DAX
MAX('Calendar'[Date])
```
---

## 1. ActualCost

**Group:** Margin & Profitability  
**Table:** `Operations`

**Business Definition:** KPI derived from the underlying Sales/Plan/Operations model.
**Purpose / Why it matters:** General reporting; include in KPI tiles, tables, and time‑series visuals.

**Technical Note:**
```DAX
SUM(Operations[Actual Cost])
```
---

## 3. % ActualCost

**Group:** Margin & Profitability  
**Table:** `Operations`

**Business Definition:** KPI derived from the underlying Sales/Plan/Operations model.
**Purpose / Why it matters:** General reporting; include in KPI tiles, tables, and time‑series visuals.

**Technical Note:**
```DAX
[1. ActualCost]/[2. PlannedCost]
```
---

## 4. NetProfit

**Group:** Margin & Profitability  
**Table:** `Operations`

**Business Definition:** Net profit and its ratio considering sales, OPEX, and COGS.
**Purpose / Why it matters:** General reporting; include in KPI tiles, tables, and time‑series visuals.

**Technical Note:**
```DAX
[4. Sales]-[1. ActualCost]-[5. COGS]
```
---

## 5. % Profit

**Group:** Margin & Profitability  
**Table:** `Operations`

**Business Definition:** Net profit and its ratio considering sales, OPEX, and COGS.
**Purpose / Why it matters:** General reporting; include in KPI tiles, tables, and time‑series visuals.

**Technical Note:**
```DAX
Operations[4. NetProfit]/[1.SalesTotal]
```
---

## 6. Margin

**Group:** Margin & Profitability  
**Table:** `MeasureTable`

**Business Definition:** Gross margin amount: Sales minus Cost of Goods Sold.
**Purpose / Why it matters:** Profitability analysis by product/segment; use in waterfall or decomposition trees.

**Technical Note:**
```DAX
[4. Sales] - [5. COGS]
```
---

## 988. Margin What-If

**Group:** Margin & Profitability  
**Table:** `MeasureTable`

**Business Definition:** Gross margin amount: Sales minus Cost of Goods Sold.
**Purpose / Why it matters:** Profitability analysis by product/segment; use in waterfall or decomposition trees.

**Technical Note:**
```DAX
[987. Sales What-If] - [5. COGS]
```
---

## Sales - OPEX

**Group:** Margin & Profitability  
**Table:** `MeasureTable`

**Business Definition:** KPI derived from the underlying Sales/Plan/Operations model.
**Purpose / Why it matters:** Profitability analysis by product/segment; use in waterfall or decomposition trees.

**Technical Note:**
```DAX
[1.SalesTotal]-SUM(OPEX[OPEX])
```
---

## 5. COGS

**Group:** Other  
**Table:** `MeasureTable`

**Business Definition:** Cost of goods sold calculated from unit cost × quantity.
**Purpose / Why it matters:** Profitability analysis by product/segment; use in waterfall or decomposition trees.

**Technical Note:**
```DAX
SUMX(Sales, Sales[Quantity] * Sales[Cost of goods])
```
---

## 1. SalesPlan

**Group:** Planning / Targets  
**Table:** `SalesPlan`

**Business Definition:** Planned sales metrics for target vs actual comparisons.
**Purpose / Why it matters:** Track performance versus monthly/annual plan; use on scorecards and pacing views.

**Technical Note:**
```DAX
SUM(SalesPlan[SalesPlan])
```
---

## 2. PlannedCost

**Group:** Planning / Targets  
**Table:** `Operations`

**Business Definition:** KPI derived from the underlying Sales/Plan/Operations model.
**Purpose / Why it matters:** Track performance versus monthly/annual plan; use on scorecards and pacing views.

**Technical Note:**
```DAX
SUM(Operations[Planned Cost])
```
---

## 4. AverageSalesPlan

**Group:** Planning / Targets  
**Table:** `SalesPlan`

**Business Definition:** Planned sales metrics for target vs actual comparisons.
**Purpose / Why it matters:** Track performance versus monthly/annual plan; use on scorecards and pacing views.

**Technical Note:**
```DAX
AVERAGE(SalesPlan[SalesPlan])
```
---

## 41. PreviousMonthSalesPlan

**Group:** Planning / Targets  
**Table:** `SalesPlan`

**Business Definition:** Sales in the previous calendar month.
**Purpose / Why it matters:** Track performance versus monthly/annual plan; use on scorecards and pacing views.

**Technical Note:**
```DAX
CALCULATE([1. SalesPlan], PREVIOUSMONTH('Calendar'[Date]))
```
---

## 42. SalesPlanYTD

**Group:** Planning / Targets  
**Table:** `SalesPlan`

**Business Definition:** Year-to-date Sales using the Calendar date table.
**Purpose / Why it matters:** Track performance versus monthly/annual plan; use on scorecards and pacing views.

**Technical Note:**
```DAX
CALCULATE([1. SalesPlan], DATESYTD('Calendar'[Date]))
```
---

## 43. SalesPlanFYTD

**Group:** Planning / Targets  
**Table:** `SalesPlan`

**Business Definition:** Year-to-date Sales using the Calendar date table.
**Purpose / Why it matters:** Track performance versus monthly/annual plan; use on scorecards and pacing views.

**Technical Note:**
```DAX
CALCULATE([1. SalesPlan], DATESYTD('Calendar'[Date], "6-30"))
```
---

## 44. SalesPlanSPLY

**Group:** Planning / Targets  
**Table:** `SalesPlan`

**Business Definition:** Same period last year comparison for Sales.
**Purpose / Why it matters:** Track performance versus monthly/annual plan; use on scorecards and pacing views.

**Technical Note:**
```DAX
CALCULATE([1. SalesPlan], SAMEPERIODLASTYEAR('Calendar'[Date]))
```
---

## 45. SalesPlan2MonthAgo

**Group:** Planning / Targets  
**Table:** `SalesPlan`

**Business Definition:** Planned sales metrics for target vs actual comparisons.
**Purpose / Why it matters:** Track performance versus monthly/annual plan; use on scorecards and pacing views.

**Technical Note:**
```DAX
CALCULATE([1. SalesPlan], DATEADD('Calendar'[Date], -2, MONTH))
```
---

## 46. SalesPlan%

**Group:** Planning / Targets  
**Table:** `SalesPlan`

**Business Definition:** Plan achievement: Actual Sales ÷ Sales Plan.
**Purpose / Why it matters:** Track performance versus monthly/annual plan; use on scorecards and pacing views.

**Technical Note:**
```DAX
[4. Sales]/[1. SalesPlan]
```
---

## 47. SalesPlan KZT

**Group:** Planning / Targets  
**Table:** `SalesPlan`

**Business Definition:** Planned sales metrics for target vs actual comparisons.
**Purpose / Why it matters:** Track performance versus monthly/annual plan; use on scorecards and pacing views.

**Technical Note:**
```DAX
SUMX(SalesPlan, SalesPlan[1. SalesPlan]* RELATED('Calendar'[KZT]))
```
---

## 48. SalesPlan Multicurrency

**Group:** Planning / Targets  
**Table:** `SalesPlan`

**Business Definition:** Planned sales metrics for target vs actual comparisons.
**Purpose / Why it matters:** Track performance versus monthly/annual plan; use on scorecards and pacing views.

**Technical Note:**
```DAX
IF(SELECTEDVALUE(dimCurrency[symbol]) = "$", [1. SalesPlan], [47. SalesPlan KZT])
```
---

## 989. SalesPlanAch

**Group:** Planning / Targets  
**Table:** `MeasureTable`

**Business Definition:** Planned sales metrics for target vs actual comparisons.
**Purpose / Why it matters:** Track performance versus monthly/annual plan; use on scorecards and pacing views.

**Technical Note:**
```DAX
DIVIDE(MeasureTable[1.SalesTotal],SalesPlan[1. SalesPlan],0)
```
---

## 1. Purchase Quantity

**Group:** Purchases & Inventory  
**Table:** `Purchases`

**Business Definition:** KPI derived from the underlying Sales/Plan/Operations model.
**Purpose / Why it matters:** Stock approximation where no real inventory table exists; use for on‑hand estimates.

**Technical Note:**
```DAX
SUM(Purchases[Quantity])
```
---

## 2. Кол-во закуплено накопительно

**Group:** Purchases & Inventory  
**Table:** `Purchases`

**Business Definition:** KPI derived from the underlying Sales/Plan/Operations model.
**Purpose / Why it matters:** Stock approximation where no real inventory table exists; use for on‑hand estimates.

**Technical Note:**
```DAX
CALCULATE([1. Purchase Quantity], FILTER(ALL('Calendar'), 'Calendar'[Date] <= MAX('Calendar'[Date])))
```
---

## 3. Остатки

**Group:** Purchases & Inventory  
**Table:** `Purchases`

**Business Definition:** KPI derived from the underlying Sales/Plan/Operations model.
**Purpose / Why it matters:** Stock approximation where no real inventory table exists; use for on‑hand estimates.

**Technical Note:**
```DAX
[2. Кол-во закуплено накопительно] - [985. CumulativeSales]
```
---

## 1.SalesTotal

**Group:** Sales (Actuals)  
**Table:** `MeasureTable`

**Business Definition:** KPI derived from the underlying Sales/Plan/Operations model.
**Purpose / Why it matters:** General reporting; include in KPI tiles, tables, and time‑series visuals.

**Technical Note:**
```DAX
SUMX(Sales, Sales[Price $/each]*Sales[Quantity])
```
---

## 2. SalesQuantity

**Group:** Sales (Actuals)  
**Table:** `MeasureTable`

**Business Definition:** KPI derived from the underlying Sales/Plan/Operations model.
**Purpose / Why it matters:** General reporting; include in KPI tiles, tables, and time‑series visuals.

**Technical Note:**
```DAX
SUM(Sales[Quantity])
```
---

## 4. Sales

**Group:** Sales (Actuals)  
**Table:** `MeasureTable`

**Business Definition:** KPI derived from the underlying Sales/Plan/Operations model.
**Purpose / Why it matters:** General reporting; include in KPI tiles, tables, and time‑series visuals.

**Technical Note:**
```DAX
SUMX(Sales, Sales[Price $/each] * Sales[Quantity])
```
---

## 41. Sales KZT

**Group:** Sales (Actuals)  
**Table:** `MeasureTable`

**Business Definition:** KPI derived from the underlying Sales/Plan/Operations model.
**Purpose / Why it matters:** Dynamic currency switching; use to present KPIs in user‑selected currency.

**Technical Note:**
```DAX
SUMX(sales, Sales[Price $/each] * Sales[Quantity] * RELATED('Calendar'[KZT]))
```
---

## 91. SalesPrevMonth

**Group:** Sales (Actuals)  
**Table:** `MeasureTable`

**Business Definition:** Sales in the previous calendar month.
**Purpose / Why it matters:** General reporting; include in KPI tiles, tables, and time‑series visuals.

**Technical Note:**
```DAX
CALCULATE([4. Sales], PREVIOUSMONTH('Calendar'[Date]))
```
---

## 92. SalesSPLY

**Group:** Sales (Actuals)  
**Table:** `MeasureTable`

**Business Definition:** Same period last year comparison for Sales.
**Purpose / Why it matters:** YoY/period-over-period benchmarking; add to variance visuals.

**Technical Note:**
```DAX
CALCULATE([4. Sales], SAMEPERIODLASTYEAR('Calendar'[Date]))
```
---

## 93. SalesYTD

**Group:** Sales (Actuals)  
**Table:** `MeasureTable`

**Business Definition:** Year-to-date Sales using the Calendar date table.
**Purpose / Why it matters:** Monitor cumulative performance within the year; use in trend charts with date slicers.

**Technical Note:**
```DAX
CALCULATE([4. Sales], DATESYTD('Calendar'[Date]))
```
---

## 94. SalesFYTD

**Group:** Sales (Actuals)  
**Table:** `MeasureTable`

**Business Definition:** Year-to-date Sales using the Calendar date table.
**Purpose / Why it matters:** Monitor cumulative performance within the year; use in trend charts with date slicers.

**Technical Note:**
```DAX
CALCULATE([4. Sales], DATESYTD('Calendar'[Date], "6-30"))
```
---

## 95. Sales2MonthAgo

**Group:** Sales (Actuals)  
**Table:** `MeasureTable`

**Business Definition:** KPI derived from the underlying Sales/Plan/Operations model.
**Purpose / Why it matters:** General reporting; include in KPI tiles, tables, and time‑series visuals.

**Technical Note:**
```DAX
CALCULATE([4. Sales], DATEADD('Calendar'[Date], -2, MONTH))
```
---

## 97. Red Sales

**Group:** Sales (Actuals)  
**Table:** `MeasureTable`

**Business Definition:** KPI derived from the underlying Sales/Plan/Operations model.
**Purpose / Why it matters:** General reporting; include in KPI tiles, tables, and time‑series visuals.

**Technical Note:**
```DAX
CALCULATE([4. Sales], FILTER(Products, Products[Color] = "Red"))
```
---

## 985. CumulativeSales

**Group:** Sales (Actuals)  
**Table:** `MeasureTable`

**Business Definition:** KPI derived from the underlying Sales/Plan/Operations model.
**Purpose / Why it matters:** General reporting; include in KPI tiles, tables, and time‑series visuals.

**Technical Note:**
```DAX
CALCULATE([2. SalesQuantity], FILTER(ALL('Calendar'), 'Calendar'[Date] <= MAX('Calendar'[Date])))
```
---

## 987. Sales What-If

**Group:** Sales (Actuals)  
**Table:** `MeasureTable`

**Business Definition:** Scenario measure applying a percentage multiplier to Sales.
**Purpose / Why it matters:** General reporting; include in KPI tiles, tables, and time‑series visuals.

**Technical Note:**
```DAX
[4. Sales] * (1+[Sales Multiplier Value]/100)
```
---

## Sales Multiplier Value

**Group:** Sales (Actuals)  
**Table:** `Sales Multiplier`

**Business Definition:** Scenario measure applying a percentage multiplier to Sales.
**Purpose / Why it matters:** General reporting; include in KPI tiles, tables, and time‑series visuals.

**Technical Note:**
```DAX
SELECTEDVALUE('Sales Multiplier'[Sales Multiplier])
```
---

## 96. AVG deliverydays

**Group:** Shipping & Delivery  
**Table:** `MeasureTable`

**Business Definition:** Average or max delivery time between order and shipment.
**Purpose / Why it matters:** Fulfillment SLA monitoring; pair with order vs ship date toggles.

**Technical Note:**
```DAX
AVERAGE(Sales[DeliveryDays])
```
---

## 98. MAX deliverydays

**Group:** Shipping & Delivery  
**Table:** `MeasureTable`

**Business Definition:** Average or max delivery time between order and shipment.
**Purpose / Why it matters:** Fulfillment SLA monitoring; pair with order vs ship date toggles.

**Technical Note:**
```DAX
MAX(Sales[DeliveryDays])
```
---

## 983. Shipped Quantity

**Group:** Shipping & Delivery  
**Table:** `MeasureTable`

**Business Definition:** Shipment-related KPI using USERELATIONSHIP to shipment date.
**Purpose / Why it matters:** Fulfillment SLA monitoring; pair with order vs ship date toggles.

**Technical Note:**
```DAX
CALCULATE([2. SalesQuantity], USERELATIONSHIP('Calendar'[Date], Sales[Shipment date]))
```
---

## 984. % Shipped

**Group:** Shipping & Delivery  
**Table:** `MeasureTable`

**Business Definition:** Shipment-related KPI using USERELATIONSHIP to shipment date.
**Purpose / Why it matters:** Fulfillment SLA monitoring; pair with order vs ship date toggles.

**Technical Note:**
```DAX
DIVIDE([983. Shipped Quantity], [2. SalesQuantity], 0)
```
---
