# 🔎 Insights & Recommendations

## ⚠️ Data & Scope Notes
- This is **demo/synthetic data**; **anomalies and unusual patterns may appear**.
- The project’s goal is to **demonstrate hard skills** (data modeling, DAX, visualization, interpretation), **not** to represent a real business scenario.
- **COGS scope:** COGS **excludes most cost elements**; treat **Profit%** as **Contribution%** until COGS is expanded.
- **2020 coverage is incomplete** (partial year), so YoY comparisons for 2020 are **not like-for-like**.
_This write‑up summarizes what the dashboards reveal across **Sales, Products, Profitability, Customers, Stores, OPEX/Operations, and Shipment** pages. Numbers are directional (screen captures); use the underlying measures for exact values.

---

## Executive Summary (2016–2020)
- **Scale:** Sales ≈ **8 bn** (multicurrency) vs **Plan ≈ 9 bn** → **Achievement ~90.9%**.
- **Profitability:** Reported **Profit/Margin value ~8 bn** with **ratio ~94%**. This is unusually high; see **caveat** below.
- **Trend:** Growth from 2016 → **peak around 2019**, then **decline in 2020** (macro effects). Quarter view shows outsized Q1 contribution.
- **Mix:** **Mobile Devices** dominate revenue; Computers, Accessories, Audio follow; **Components** smallest.
- **Store performance:** **Costco, K‑Mart, Best Buy, Staples** lead; long tail of smaller stores underperforms plan.
- **Fulfilment:** **100% shipped** with **avg delivery ≈ 4.1 days** (max ≈ 10). **Tuesdays** show slowest deliveries in the weekday chart.
- **OPEX control:** **Achievement ≈ 98%**; plan and actual OPEX closely track at the year and category levels.

> **Caveat — Profit 94%**: 94% suggests `COGS` excludes most cost elements. 

---

## Sales page
- **Sales by Year:** Climb through 2017–2019; **2020 down** materially vs 2019. Monitor YoY rebound.
- **Sales by Stores:** Clear leaders (**Costco, K‑Mart, Best Buy, Staples**). Gap to mid‑tier retailers is large.
- **Quarter mix:** Q1 bar ~highest. Consider smoothing seasonality in plan.
- **Category contribution:** Mobile Devices are ~top driver; Components least.

**Actions**
1. Revisit **plan calibration** (2019→2020 shift) and set **seasonality factors** in `SalesPlan`.
2. Drive **price/volume programs** on mid‑tier stores; replicate winning playbooks from Costco/K‑Mart.
3. Protect **Mobile Devices** availability (lead SKUs), but grow **Computers/Accessories** with bundles.

---

## Products page
- **Top brands by quantity:** **Dell, Huawei, HP, Samsung** lead the chart.
- **Top 5 subcategories:** Phones clearly #1; Desktop, Laptop next; Mice and Speakers strong value contributors.
- **Margin%** generally high across leading subcategories; maintain mix toward high‑margin SKUs.


---

## Profitability page
- **Development over the period:** Sales, Plan, and Profit move together; peak ~2019, low in 2020.
- **Geo:** Profit concentrated in major cities (KZ primary markets), but some cities underperform.
- **By Category:** Mobile Devices deliver the biggest absolute profit; Computers next.


---

## Customers page
- **Sales & AVG receipt by occupation:** Highest volumes from **Office** and **Engineer** segments; **AVG receipt** declines across segments.
- **Acquisition sources:** **Advertisement** and **Internet** dominate the treemap; **Friends/Word‑of‑mouth** meaningful; **Radio** smaller.
- **Brand preference by age:** Panels show distinct patterns between **above 55 / 45–55 / below 45** cohorts.

---

## Stores page
- **Sales & Plan by store:** Top stores are also closest to plan; many smaller stores miss plan materially.
- **Salespeople distribution:** `Walmart` shows the largest headcount; star icons highlight top sizes.
- **Geo:** Sales by city map mirrors profitability: large clusters in major KZ cities.



---

## Operations (OPEX) page
- **KPI tiles:** **OPEX Achievement ~98%**, Plan and Fact OPEX ~10M (directional).
- **By year & category:** Actuals track plan closely; categories like **Salary/Marketing** dominate spend.
- **By store size:** Large and medium stores consume most OPEX; achievement ~97–99% everywhere.



---

## Shipment page
- **KPIs:** **Shipped% 100%**, **Units ~2M**, **MAX delivery ~10 days**, **AVG ~4.06 days**.
- **By weekday:** **Tuesday** spikes to ~5 days; weekends lowest (~3 days).
- **By city:** Narrow band (~3.8–4.2 days); opportunities to pull the tail left.
- **By store:** **Costco** highest shipped quantity; some stores show higher AVG delivery vs peers.


---

## Measurement & Data Notes
- **Currency toggle:** `dimCurrency[symbol]` switches between Base and **KZT**; ensure `Calendar[KZT]` is dense daily (carry‑forward monthly rates).
- **Plan vs Actual:** Ensure `SalesPlan` grain aligns to **Store × Month** and currency.
- **Margin% definition:** If profit tiles represent gross margin, validate `COGS` scope. Otherwise label as **Contribution%** in the UI and docs.
- **Shipment metrics:** Use the inactive relationship `Sales[Shipment date]` → `Calendar[Date]` via `USERELATIONSHIP` (already in measures).

---

## Next 30‑day roadmap
1. **Rename/confirm Profit%** in visuals & docs; add footnote on COGS scope.  
2. **Create “Plan Diagnostics” page** (variance waterfall: price, mix, volume, distribution).  
3. **Introduce Service KPIs** (`P80 delivery days`, `% ≤ 3 days`) and a **weekday control chart**.  
4. **A/B test bundles** in Mobile Devices to lift AVG receipt and attach rate.  
5. **Store productivity** deck: sales per salesperson, per sqm; actions for bottom quartile.  

---

### KPI ↔ Measure Map (selection)
- Sales: `[4. Sales]`, `[42. Sales Multicurrency]`, `[93. SalesYTD]`, `[92. SalesSPLY]`
- Plan: `[1. SalesPlan]`, `[48. SalesPlan Multicurrency]`, `[989. SalesPlanAch]`
- Profitability: `[6. Margin]`, `[8. Margin%]`, `Operations[4. NetProfit]`
- OPEX: `Operations[1. ActualCost]`, `Operations[2. PlannedCost]`, `Operations[3. % ActualCost]`
- Shipment: `[983. Shipped Quantity]`, `[984. % Shipped]`, `[96. AVG deliverydays]`, `[98. MAX deliverydays]`

---


