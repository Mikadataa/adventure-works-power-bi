# 🚀 Power BI Portfolio — Adventure Works (Demo)

![Power BI](https://img.shields.io/badge/Power%20BI-Modeling%20•%20DAX%20•%20Power%20Query-FFB900?logo=powerbi&logoColor=white)
![DAX](https://img.shields.io/badge/DAX-Time%20Intelligence%20•%20USERELATIONSHIP%20•%20What--If-0A66C2)
![Data Modeling](https://img.shields.io/badge/Data%20Modeling-Star%20Schema%20•%20Inactive%20Dates%20•%20Role%20Dims-4CAF50)
![SQL](https://img.shields.io/badge/SQL-SQL%20Server%20•%20OPEX%20joins-CC2927?logo=microsoftsqlserver&logoColor=white)
![Git](https://img.shields.io/badge/Git-Repo%20Structure%20•%20LFS%20for%20PBIX-000000?logo=github)
![LinkedIn](https://img.shields.io/badge/LinkedIn-Open%20to%20Opportunities-0A66C2?logo=linkedin&logoColor=white)

> **Note**: This project uses **demo/synthetic data**. Some anomalies may appear. The goal is to demonstrate **hard skills** (data modeling, **complex DAX**, visualization, and **business interpretation**), **not** a real business scenario.  
> **COGS scope**: COGS **excludes most cost elements** → treat **Profit%** as **Contribution%**.  
> **Coverage**: **2020 is partial**; avoid 1:1 YoY comparisons with 2020.

---

## 🏆 Project overview
- **End‑to‑end**: Power Query (ingest/shape) → star schema → DAX measures → storytelling dashboards.
- **Complex DAX**: time intelligence (YTD/FYTD/SPLY), **inactive relationship** for shipments with `USERELATIONSHIP`, multi‑currency switch via a **disconnected slicer**, and **What‑If** scenarios.
- **Business thinking**: page‑level insights and stakeholder recommendations, not just charts.
- **Engineering quality**: measure table, naming conventions, README/docs, repo layout, Git LFS for PBIX.

### 📸 Snapshot (click to view full size)
<p align="center">
  <img src="docs/screenshots/main.png" alt="Main page" width="46%"/>
  <img src="docs/screenshots/sales.png" alt="Sales page" width="46%"/>
  <img src="docs/screenshots/products.png" alt="Products page" width="46%"/><br/>
  <img src="docs/screenshots/contribution.png" alt="Contribution page" width="46%"/>
  <img src="docs/screenshots/customers.png" alt="Customers page" width="46%"/><br/>
  <img src="docs/screenshots/stores.png" alt="Stores page" width="46%"/>
  <img src="docs/screenshots/operations.png" alt="Operations page" width="46%"/><br/>
  <img src="docs/screenshots/shipment.png" alt="Shipment page" width="46%"/>
</p>

---

## 🔧 Model & Tech Highlights
- **Schema**: multi‑fact **star/snowflake** in **Import** mode (13 tables, 12 relationships).  
- **Dates**: canonical `Calendar` table (marked as Date). Secondary **inactive** path for `Sales[Shipment date]`.  
- **Currency**: `dimCurrency` **disconnected slicer** + `Calendar[KZT]` FX rate for conversion and display.  
- **Operations/OPEX**: joined to stores & calendar to compute **Net Profit** alongside Sales/COGS.

**Docs:** [Model overview](docs/model_overview.md) • [Data documentation](docs/data_documentation.md)

---

## 🧠 DAX — representative examples

**Shipment with inactive date**  
```DAX
983. Shipped Quantity =
CALCULATE([2. SalesQuantity],
    USERELATIONSHIP('Calendar'[Date], Sales[Shipment date]))
```
**Share of sales within current product filter**  
```DAX
981. Sales Share =
VAR SalesTotal = SUMX(Sales, Sales[Price $/each] * Sales[Quantity])
VAR SalesAll   = CALCULATE([4. Sales], ALL(Products))
RETURN DIVIDE(SalesTotal, SalesAll)
```
**Margin with variables**  
```DAX
8. Margin% =
VAR SalesTotal = SUMX(Sales, Sales[Price $/each] * Sales[Quantity])
VAR COGS       = SUMX(Sales, Sales[Cost of goods] * Sales[Quantity])
VAR Margin     = SalesTotal - COGS
RETURN DIVIDE(Margin, SalesTotal)
```
**Multi‑currency switch (disconnected slicer)**  
```DAX
42. Sales Multicurrency =
IF(SELECTEDVALUE(dimCurrency[symbol]) = "$", [4. Sales], [41. Sales KZT])
```

More: [All measures](docs/dax_measures.md) • [Function reference](docs/dax_function_reference.md) • [Measure logic (business intent)](docs/measure_logic.md)

---

## 📈 Interpretation (what the report tells you)
- Sales grew to a **2019 peak**, then declined in **2020** (partial year).  
- **Mobile Devices** dominate; **Components** are smallest.  
- **Costco, K‑Mart, Best Buy, Staples** lead store performance; mid‑tier stores lag plan.  
- **Fulfilment** solid: ~**4.1 days** avg delivery; **Tuesday** has the slowest deliveries.  
- **OPEX** ~**98% achievement**; close tracking to plan.

Deep dive: [Insights](docs/insights.md) • 

---

## 🛠️ To Run locally (60 seconds)
1. Clone repo and open `adventure_works_project.pbix` in Power BI Desktop (x64).  
2. In **Transform data → Data source settings**, point to your local `Sources/` folder.  
3. Refresh; use the **currency switch** slicer to view USD/KZT.

> Tip: Make a parameter `DataFolder` and replace absolute paths in M with `File.Contents(DataFolder & "\...")` for portability.

---

## 🗂 Repo structure
```
adventure-works-power-bi/
│  adventure_works_project.pbix
│  README.md  • LICENSE  • .gitignore  • .gitattributes
├─docs/
│   model_overview.md  data_documentation.md
│   dax_measures.md    dax_function_reference.md
│   measure_logic.md   insights.md   actions.md
│   screenshots/ (page images)
└─Sources/ (demo data)
```

---

## 👤 Author & Links
**Author:** Mika Smagulova  
**GitHub:** https://github.com/Mikadataa 
**LinkedIn:** https://www.linkedin.com/in/smagulova/

---

## 📄 License
MIT — see [LICENSE](LICENSE).
