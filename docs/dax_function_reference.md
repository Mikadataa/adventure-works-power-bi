# 📘 DAX Function Reference (Used in This Model)

This guide lists the DAX functions used across your measures, with syntax, what they do, notes/pitfalls, and short examples you can paste into your docs.

## CALCULATE

**Syntax:** `CALCULATE(<expression>, <filter1>, <filter2>, …)`

**What it does:** Changes the filter context and evaluates an expression in that new context.

**Notes / Pitfalls:**
- Filter arguments are combined with AND; use KEEPFILTERS/OR patterns for alternatives.
- Row context → filter context transition happens inside iterators (e.g., SUMX) when used with CALCULATE.
- Common with time-intelligence to shift dates.
**Example:**
```DAX
Sales LY = CALCULATE([Sales], SAMEPERIODLASTYEAR('Calendar'[Date]))
```

## FILTER

**Syntax:** `FILTER(<table>, <boolean_expression>)`

**What it does:** Returns a table of rows that meet a condition; often used inside CALCULATE to define filters.

**Notes / Pitfalls:**
- Avoid row-by-row FILTER over large tables when a simple column filter or ALL/ALLEXCEPT can do.
- FILTER keeps existing filters unless wrapped with ALL/ALLSELECTED.
**Example:**
```DAX
Red Sales = CALCULATE([Sales], FILTER(Products, Products[Color] = "Red"))
```

## DATESYTD

**Syntax:** `DATESYTD(<dates_column> [, <year_end_date_string>])`

**What it does:** Returns a YTD date range for the current context; used in CALCULATE to aggregate YTD metrics.

**Notes / Pitfalls:**
- Requires a continuous, marked Date table.
- Use the optional year-end parameter for custom fiscal years (e.g., "6-30").
**Example:**
```DAX
Sales YTD = CALCULATE([Sales], DATESYTD('Calendar'[Date]))
```

## SAMEPERIODLASTYEAR

**Syntax:** `SAMEPERIODLASTYEAR(<dates_column>)`

**What it does:** Shifts the current period one year back; useful for YoY comparisons.

**Notes / Pitfalls:**
- Requires a proper Date table with continuous dates.
- Careful with incomplete months/years; may need PARALLELPERIOD for custom offsets.
**Example:**
```DAX
Sales SPLY = CALCULATE([Sales], SAMEPERIODLASTYEAR('Calendar'[Date]))
```

## PREVIOUSMONTH

**Syntax:** `PREVIOUSMONTH(<dates_column>)`

**What it does:** Returns the set of dates for the previous month in the current context.

**Notes / Pitfalls:**
- Context-aware: if filtered to a month, it returns the prior month; otherwise, behavior follows current selection.
**Example:**
```DAX
Sales Prev Month = CALCULATE([Sales], PREVIOUSMONTH('Calendar'[Date]))
```

## DATEADD

**Syntax:** `DATEADD(<dates_column>, <number_of_intervals>, <interval>)`

**What it does:** Shifts a date column by a given number of intervals (DAY, MONTH, QUARTER, YEAR).

**Notes / Pitfalls:**
- Unlike DATEADD in SQL, this returns a table of shifted dates to use in time intelligence.
- Requires a continuous Date table.
**Example:**
```DAX
Sales 2 Months Ago = CALCULATE([Sales], DATEADD('Calendar'[Date], -2, MONTH))
```

## SUMX

**Syntax:** `SUMX(<table>, <expression>)`

**What it does:** Iterator that evaluates an expression for each row in a table and sums the results.

**Notes / Pitfalls:**
- Row context exists inside the iterator; use RELATED/RELATEDTABLE to access lookups.
- More expensive than SUM; prefer base columns when possible.
**Example:**
```DAX
Sales = SUMX(Sales, Sales[Price $/each] * Sales[Quantity])
```

## SUM / AVERAGE / MAX

**Syntax:** `SUM(<column>) / AVERAGE(<column>) / MAX(<column>)`

**What it does:** Aggregate functions over a single column.

**Notes / Pitfalls:**
- Respect filter context; use CALCULATE + ALL to remove filters when needed.
**Example:**
```DAX
Total Qty = SUM(Sales[Quantity])
Avg Delivery Days = AVERAGE(Sales[DeliveryDays])
Max Delivery Days = MAX(Sales[DeliveryDays])
```

## DISTINCTCOUNT

**Syntax:** `DISTINCTCOUNT(<column>)`

**What it does:** Counts distinct values in a column under current filters.

**Notes / Pitfalls:**
- May be affected by blank values; consider removing blanks when necessary.
**Example:**
```DAX
Customer Count = DISTINCTCOUNT(Sales[CustomerID])
```

## DIVIDE

**Syntax:** `DIVIDE(<numerator>, <denominator> [, <alternate_result>])`

**What it does:** Safe division that avoids divide-by-zero errors.

**Notes / Pitfalls:**
- Prefer DIVIDE over / to handle zero/blank denominators gracefully.
**Example:**
```DAX
% Shipped = DIVIDE([Shipped Qty], [Sales Qty], 0)
```

## USERELATIONSHIP

**Syntax:** `USERELATIONSHIP(<column1>, <column2>)`

**What it does:** Activates an inactive relationship for the duration of a calculation.

**Notes / Pitfalls:**
- Use inside CALCULATE; useful for order vs shipment date scenarios.
**Example:**
```DAX
Shipped Qty = CALCULATE([Sales Qty], USERELATIONSHIP('Calendar'[Date], Sales[Shipment date]))
```

## SELECTEDVALUE

**Syntax:** `SELECTEDVALUE(<column> [, <alternate_result>])`

**What it does:** Returns the value when only one value is selected; otherwise returns alternate result (or BLANK by default).

**Notes / Pitfalls:**
- Great for reporting titles and slicer-driven labels.
**Example:**
```DAX
Selected Currency = "Selected Currency: " & SELECTEDVALUE(dimCurrency[symbol])
```

## IF

**Syntax:** `IF(<logical_test>, <result_true> [, <result_false>])`

**What it does:** Conditional branching for scalar expressions.

**Notes / Pitfalls:**
- For multiple conditions consider SWITCH(TRUE(), ...).
**Example:**
```DAX
Sales (Multi-Currency) = IF(SELECTEDVALUE(dimCurrency[symbol])="$", [Sales], [Sales KZT])
```

## ALL / ALLEXCEPT

**Syntax:** `ALL([<table_or_column>]) / ALLEXCEPT(<table>, <col1>, <col2>, …)`

**What it does:** Removes filters (ALL) or removes all except specified columns (ALLEXCEPT). Used for share/ratio calculations.

**Notes / Pitfalls:**
- Use sparingly; removing filters can surprise users. Prefer ALLSELECTED for visuals where you want to keep outer context.
**Example:**
```DAX
Sales Share = VAR SalesAll = CALCULATE([Sales], ALL(Products)) RETURN [Sales] / SalesAll
```

## RELATED

**Syntax:** `RELATED(<column>)`

**What it does:** Accesses a value from a related table in a row context (used often inside iterators).

**Notes / Pitfalls:**
- Requires a relationship; works from many-to-one (fact to dimension).
**Example:**
```DAX
Sales KZT = SUMX(Sales, Sales[Price $/each] * Sales[Quantity] * RELATED('Calendar'[KZT]))
```

## VAR / RETURN

**Syntax:** `VAR <name> = <expression> … RETURN <final_expression>`

**What it does:** Creates intermediate variables for readability and performance.

**Notes / Pitfalls:**
- Define complex calculations step-by-step; variables are immutable within the measure.
**Example:**
```DAX
Margin % = 
VAR SalesTotal = [Sales]
VAR COGS = [COGS]
VAR Margin = SalesTotal - COGS
RETURN DIVIDE(Margin, SalesTotal)
```
