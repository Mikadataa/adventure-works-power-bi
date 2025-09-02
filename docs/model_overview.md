# 🧭 Semantic Model Overview

This describes the structure of your Power BI semantic model (based on the screenshot you shared).

- **Model mode:** Import  
- **Tables:** 13  
- **Relationships:** 12  
- **Measures:** 49 (organized mainly in `MeasureTable`)  
- **Expressions:** 13  

---

## 1) Design Pattern (High level)
A **star / light snowflake** centered on multiple fact tables with shared dimensions.

**Facts**
- `Sales` (primary fact)
- `Purchases`
- `OPEX`
- `Operations`
- `SalesPlan`

**Dimensions / Lookups**
- `Calendar` (date dimension)
- `Products`
- `dimCustomers`
- `Store`
- `dimCurrency` (disconnected slicer for currency selection)
- `currency` (FX rates lookup / staging)
- `MeasureTable` (logical table to host measures)

Relationships are predominantly **many-to-one, single-direction** from facts to dimensions. An **inactive** date path (Shipment date) is used for shipping KPIs and activated in measures with `USERELATIONSHIP`.

---

## 2) Fact Tables & Grain

### `Sales` (primary fact)
- **Grain:** receipt line (one product per receipt line).  
- **Key columns:** `Receipt Number`, `Date`, `Shipment date`, `Product`, `CustomerID`, `Store`, `Quantity`, `Price $/each`, `Cost of goods`.  
- **Relationships:**
  - `Sales[Date]` → `Calendar[Date]` (**Active**)
  - `Sales[Shipment date]` → `Calendar[Date]` (**Inactive**, used by shipping measures)
  - Product/Customer/Store to their dimensions
- **Used by measures:** `Sales`, `COGS`, `Margin`, `Margin%`, YTD/SPLY/PrevMonth, shares, shipping KPIs.

### `Purchases`
- **Grain:** invoice line (merged old/new systems).  
- **Key columns:** `Invoice #`, `Date`, `Operation Type`, `Product`, `Quantity`, `Cost Price`, `Store`, `Supplier Type`.  
- **Use:** cumulative purchased quantity and simple on‑hand approximation (`Остатки` = cumulative purchases − cumulative sales).

### `OPEX`
- **Grain:** monthly by `Category` / `Subcategory`.  
- **Relationship:** `OPEX[Month]` → `Calendar[Date]`.  
- **Use:** operating expense analysis; pairs with `Operations` for profitability.

### `Operations`
- **Grain:** daily store‑level planned/actual spend.  
- **Relationships:** `Operations[Date]` → `Calendar[Date]`, `Operations[Store]` → `Store[Store ID]`.  
- **Measures:** `ActualCost`, `PlannedCost`, `% ActualCost`, `NetProfit` (Sales − ActualCost − COGS).

### `SalesPlan`
- **Grain:** monthly by store.  
- **Relationships:** `SalesPlan[Month]` → `Calendar[Date]`, `SalesPlan[Store]` → `Store[Store ID]`.  
- **Measures:** `SalesPlan`, YTD/FYTD/SPLY/PrevMonth, `SalesPlan%`, `SalesPlanAch`.

---

## 3) Dimensions & Lookups

### `Calendar` (Date table)
- Canonical date dimension for time intelligence.  
- Typical columns: `Date`, `Year`, `Month`, `MonthNumber`, `Quarter`, `Week`, and **`KZT`** (FX rate).  
- Mark as Date table; no gaps in analysis range.

### `Products`
- Product attributes (name, category, color) + derived **Weight Category** from Power Query.  
- Joins to `Sales`/`Purchases` (currently by text name).

### `dimCustomers`
- Enriched customer master: `CustomerID`, `Customer`, `Gender`, `Date_of_birth`, `Phone Number`, `Occupation`, `Source`.  
- Built from `Customer Info` + SQL `CustomerDetails`.

### `Store`
- Store metadata (`Store ID`, `Store Name`, `City`, `Area`, `Latitude`, `Longitude`) enriched from SQL `storeinfo`: `StoreSize`, `NumberOfCheckOuts`, `DateOpened`.

### `dimCurrency` (disconnected slicer)
- Drives currency selection (USD/KZT). Used by measures to switch between raw and converted values.

### `currency`
- FX source/lookup from SQL. You presently use `Calendar[KZT]` in measures; keep `currency` as staging or map it into `Calendar` during refresh.

### `MeasureTable`
- Logical container for measures only.

---

## 4) Relationship Notes
- Single-direction filters from facts ⇢ dimensions to avoid ambiguity.  
- Inactive `Sales[Shipment date]` relationship enables `% Shipped` without changing default date semantics.  
- Avoid bi‑directional filters except where necessary.

---

## 5) Currency Handling
- **Conversion:** `Sales KZT` / `SalesPlan KZT` multiply by `Calendar[KZT]`.  
- **Switching:** `dimCurrency[symbol]` controls `Sales Multicurrency` / `SalesPlan Multicurrency`.  
- Ensure daily FX coverage (carry‑forward if base is monthly) for stable YTD/SPLY.

---

## 6) What‑If Scenario
- Parameter table **Sales Multiplier** powers `Sales What‑If` and `Margin What‑If` measures to test % uplifts.

---

## 7) Quality & Performance Tips
- Prefer **integer surrogate keys** for Product/Customer joins (better compression than text).  
- Hide raw numeric columns used only by measures from report view.  
- Disable load on staging queries not needed in the model.  
- Ensure plan grain (store/month) and currency alignment with actuals to avoid variance mismatches.

---

## 8) KPIs this model supports
- **Growth & pacing:** YTD, FYTD, SPLY.  
- **Profitability:** Margin, Margin%, Net Profit (with OPEX).  
- **Fulfilment:** `% Shipped` via shipment date path.  
- **Inventory (lightweight):** cumulative Purchases vs Sales quantity.  
- **Multi‑currency:** user‑controlled USD/KZT presentation.
