# 📊 Sales & Profit Analysis Dashboard

An interactive Power BI dashboard built on the **Sample Superstore** dataset to analyze sales performance, profitability, and customer behavior across regions, segments, and product categories.

![Power BI](https://img.shields.io/badge/Power%20BI-F2C811?style=flat&logo=powerbi&logoColor=black)
![Status](https://img.shields.io/badge/Status-Completed-brightgreen)

---

## 📁 Repository Structure

```
Sales-Profit-Analysis-Dashboard/
│
├── data/
│   └── Sample_Superstore_raw_table.xls        # Original, unmodified source data
│
├── model/
│   ├── Conceptual_Model.png                   # High-level star schema diagram
│   └── Physical_Datamodel.png                 # Table-level relationships & fields
│
├── dashboard/
│   ├── Sales_Profit_Analysis_Dashboard.pbix    # Power BI report file
│   ├── Dashboard-Page1_Overview.png            # Screenshot – Page 1
│   └── Dashboard-Page2_Trends_Performance.png  # Screenshot – Page 2
│
├── Documentation.md                            # Full project documentation
└── README.md                                   # This file
```

> All original files used to build this project are included above so the project can be reproduced end-to-end.

---

## 🖼️ Dashboard Preview

### Page 1 — Overview
![Overview Page](https://github.com/DRasool7013/Sales-Profit-Analysis-Dashboard/blob/main/Dashboard-Page1_Overview.png)
- KPI cards, regional sales, profit by segment, current vs. previous month sales, sales by segment/category, top 10 products

### Page 2 — Trends & Performance
![Trends & Performance Page](https://github.com/DRasool7013/Sales-Profit-Analysis-Dashboard/blob/main/Dashboard-Page2_Trends%20%26%20Performance.png)
- Profit by category, sales by state, orders & profit by region, avg. discount vs. profit margin, sales trend by quarter/year

---

## 🎯 Project Aim

To build a two-page executive dashboard that helps stakeholders quickly answer:
- Which product category drives the most profit?
- How do sales trend across years and quarters?
- Which ship mode is used most, and how does it affect profit?
- What is the average discount per category, and how does it impact profit?
- Which cities/states generate the highest sales?

(Full breakdown of answers is in [`Documentation.md`](./Documentation.md).)

---

## 🧮 Data Model

Star schema with one fact table and three dimension tables.

**Conceptual model:**

![Conceptual Model](model/conceptual-model.png)

**Physical data model (as built in Power BI):**

![Physical Data Model](model/physical-data-model.png)

| **Fact Sales** | Order ID, Order Date, Customer ID, Product ID, Ship Mode, Sales, Quantity, Discount, Profit |
| **Dim Product** | Product ID, Product Name, Category, Sub-Category |
| **Dim Customer** | Customer ID, Customer Name, Segment, City, State, Region, Country |
| **Dim Calender** | Date, Month, Quarter, Year, Weekday |

Relationships: `Dim Product[Product ID]` → `Fact Sales[Product ID]` (1:*), `Dim Customer[Customer ID]` → `Fact Sales[Customer ID]` (1:*), `Dim Calender[Date]` → `Fact Sales[Order Date]` (1:*).

---

## 📈 KPIs

| KPI | Description |
|---|---|
| **Total Sales** | Sum of `Sales` across all orders |
| **Total Profit** | Sum of `Profit` across all orders |
| **Total Customers** | Distinct count of `Customer ID` |
| **Total Orders** | Distinct count of `Order ID` |
| **Total Products** | Distinct count of `Product ID` |

## 🎚️ Slicers

- **Ship Mode** — Standard Class, First Class, Second Class, Same Day
- **Segment** — Consumer, Corporate, Home Office
- **Category** — Furniture, Office Supplies, Technology
- **Month**
- **Year**

## 📊 Visuals

**Page 1 – Overview**
1. Sales Performance by Region (clustered column) — compares sales across regions
2. Profit by Segment (pie chart) — contribution of profit by each customer segment
3. Current Month Sales vs. Previous Month Sales (column) — compares actual sales this month to date vs. last month
4. Total Sales by Segment and Category (clustered column) — sales of each category across different customer segments
5. Top 10 Products by Sales (bar chart)

**Page 2 – Trends & Performance**
1. Total Profit by Category (bar chart) — sales of categories across consumer segments, viewed by profit
2. Total Sales by State (bar chart) — which states generate the highest sales
3. Orders & Total Profit by Region (combo chart)
4. Avg. Discount and Profit Margin % by Category (combo bar) — average discount per category and its impact on profit
5. Sales Trend by Quarter and Year (line chart) — how sales trend across years and quarters

---

## 🛠️ How This Repository Was Built (Step-by-Step)

### 1. Set up the repository
1. Create a new repo on GitHub → **New repository** → name it `Sales-Profit-Analysis-Dashboard`.
2. Add a short description, set visibility to **Public**, initialize with a `README.md`.
3. Clone it locally: `git clone https://github.com/<your-username>/Sales-Profit-Analysis-Dashboard.git`
4. Create the folder structure shown above (`data/`, `model/`, `dashboard/`).
5. Copy in the raw dataset, model diagrams, `.pbix` file, and screenshots.
6. `git add . && git commit -m "Initial commit: raw data, model, dashboard, docs" && git push`

### 2. Clean and model the data (Power BI Desktop)
1. **Get Data** → Excel → select `Sample_Superstore_raw_table.xls`.
2. In **Power Query Editor**: remove duplicates, fix data types (dates → Date, Sales/Profit/Discount → Decimal), trim text columns, handle nulls.
3. Split the flat table into **Fact Sales**, **Dim Product**, **Dim Customer**, and a generated **Dim Calender** (via a Date table / `CALENDAR()` DAX function).
4. Go to the **Model** view and build the star-schema relationships shown above (one-to-many, single direction, from each dimension to the fact table).

### 3. Build the KPI cards
1. Insert a **Card** visual for each metric.
2. Use `Total Sales = SUM(Fact_Sales[Sales])`, `Total Profit = SUM(Fact_Sales[Profit])`, `Total Customers = DISTINCTCOUNT(Fact_Sales[Customer ID])`, `Total Orders = DISTINCTCOUNT(Fact_Sales[Order ID])`, `Total Products = DISTINCTCOUNT(Fact_Sales[Product ID])`.

### 4. Add slicers (step-by-step)
1. Select the **Slicer** visual from the Visualizations pane.
2. Drag the field onto the slicer:
   - `Fact_Sales[Ship Mode]`
   - `Dim_Customer[Segment]`
   - `Dim_Product[Category]`
   - `Dim_Calender[Month]`
   - `Dim_Calender[Year]`
3. Format each slicer: **Format pane** → set style to **List** (or dropdown for Month/Year), enable **Select All**, and turn on **Multi-select with CTRL**.
4. Resize and align all five slicers in the left-hand panel so they appear identically on every page.
5. To make slicers apply across both pages: select each slicer → **Format** → **Sync slicers** → check both **Page 1** and **Page 2**.

### 5. Build the charts
1. For each visual, select the chart type from the Visualizations pane, then drag the relevant dimension field to **Axis/Legend** and the measure to **Values**.
2. Example — *Sales Performance by Region*: Clustered Column Chart → Axis = `Dim_Customer[Region]`, Values = `Total Sales`.
3. Example — *Sales Trend by Quarter and Year*: Line Chart → Axis = `Dim_Calender[Year]` & `Dim_Calender[Quarter]`, Values = `Total Sales`.
4. Add a text box titled **"Sales & Profit Analysis Dashboard"** at the top of each page and format it as the report title/banner.

### 6. Publish and share
1. **File → Publish → Publish to Power BI** (choose your workspace).
2. Or export as PDF/image and add to the `dashboard/` folder for GitHub preview.
3. Update this `README.md` with the final screenshots.

---

## 🧰 Tools Used
- **Power BI Desktop** — data modeling, DAX, visualization
- **Power Query** — data cleaning and transformation
- **Excel** — source data (Sample Superstore dataset)
- **GitHub** — version control and portfolio hosting

## 🚀 How to Use This Repository
1. Clone or download the repo.
2. Open `dashboard/Sales_Profit_Analysis_Dashboard.pbix` in Power BI Desktop.
3. Use the slicers on the left panel to filter by Ship Mode, Segment, Category, Month, and Year.
4. Read [`Documentation.md`](./Documentation.md) for the full analysis write-up, cleaning steps, and insights.

## 👤 Author
Feel free to connect or raise an issue if you have suggestions for improving this dashboard.
