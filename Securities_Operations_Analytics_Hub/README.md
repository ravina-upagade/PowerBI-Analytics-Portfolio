# 📊 Securities Operations Analytics Hub

### Executive Overview Dashboard — Power BI Project

---

## 📌 Project Overview

The **Securities Operations Analytics Hub** is an enterprise-grade Power BI dashboard designed to provide a comprehensive, real-time executive view of a securities brokerage firm's operational and financial performance. It enables leadership and operations teams to monitor trade activity, brokerage revenue, branch performance, product mix, and client segments — all in one unified interface.

---

## 🖥️ Dashboard Preview

> **Dashboard Title:** Securities Operations Analytics Hub — Executive Overview Dashboard

The dashboard features a dark-themed, professional layout with the following key panels:
- KPI Summary Bar
- Monthly Trade Value Trend (Area Chart)
- Top Products by Revenue (Bar Chart)
- Segment Distribution by Trade Value (Donut Chart)
- Regional Performance (Treemap)
- Top 5 Branches by Revenue (Table)

---

## 🗂️ Data Source

| Property        | Details                        |
|----------------|--------------------------------|
| **Source**      | Microsoft SQL Server           |
| **Connection**  | DirectQuery / Import Mode      |
| **Database**    | Securities Operations Database |
| **Tables Used** | Trades, Orders, Clients, Branches, Products, Regions, Segments, Financial Calendar |

> ⚠️ Ensure SQL Server credentials and connection strings are configured in Power BI Desktop under **Home → Transform Data → Data Source Settings** before publishing.

---

## 📐 Data Model

The report follows a **Star Schema** design:

```
FactTrades (Central Fact Table)
    ├── DimDate          — Financial year, month, quarter
    ├── DimRegion        — East, North, South, West
    ├── DimSegment       — Commodity, Currency, Equity, F&O, Mutual Funds
    ├── DimProduct       — Equity Cash, Futures, Options, Mutual Funds
    ├── DimBranch        — Branch name, city, region mapping
    └── DimClientSegment — HNI, Institutional, Retail, Ultra HNI
```

---

## 📊 Key Metrics & KPIs

| KPI | Value (All Years) | Description |
|-----|------------------|-------------|
| **Trade Value** | ₹ 39.23 Billion | Total value of all trades executed |
| **Brokerage Revenue** | ₹ 21.33 Million | Revenue earned from brokerage commissions |
| **Total Trades** | 3,000 | Count of all trade transactions |
| **Total Orders** | 2,000 | Count of all placed orders |
| **Active Clients** | 1,122 | Unique clients with active positions |

---

## 📈 Report Visuals & Descriptions

### 1. Monthly Trade Value Trend *(Area Chart)*
- Plots **Total Trade Value** vs. **Previous Year Trade Value** month-over-month
- Helps identify seasonal patterns and YoY growth trends
- X-Axis: Month | Y-Axis: Trade Value (₹ in billions)

### 2. Top Products by Revenue *(Horizontal Bar Chart)*
Ranked by brokerage revenue (₹ in Lakhs):
1. Equity Cash – Delivery
2. Equity Cash – Intraday
3. Silver Futures
4. Crude Oil Futures
5. Equity Mutual Funds
6. Currency Options – USDINR

### 3. Segment Distribution *(Donut Chart)*
- Displays **trade value share (%)** across segments:
  - Equity | F&O | Commodity | Currency | Mutual Funds

### 4. Regional Performance *(Treemap)*
- Revenue breakdown by region: **East, North, South, West**
- Tile size proportional to revenue contribution

### 5. Top 5 Branches by Revenue *(Table)*

| Branch | City | Revenue | Active Clients | Growth (YoY) |
|---|---|---|---|---|
| Hyderabad – Branch 5 | Hyderabad | ₹ 28.02M | 281 | 8.94% |
| Jaipur – Branch 10 | Jaipur | ₹ 25.97M | 275 | 8.25% |
| Kolkata – Branch 7 | Kolkata | ₹ 25.58M | 281 | 9.03% |
| Lucknow – Branch 1 | Lucknow | ₹ 26.46M | 243 | 8.59% |
| Mumbai – Branch 2 | Mumbai | ₹ 25.19M | 277 | 8.31% |
| **Total** | | **₹ 131.22M** | **938** | **8.63%** |

---

## 🔽 Filters & Slicers

The left panel provides interactive slicers for dynamic filtering:

| Slicer | Options |
|---|---|
| **Financial Year** | All / Individual Years |
| **Region** | East, North, South, West |
| **Segment** | Commodity, Currency, Equity, F&O, Mutual Funds |
| **Client Segments** | HNI, Institutional, Retail, Ultra HNI |

All visuals are **cross-filtered** — selecting any slicer or visual element updates the entire dashboard in real-time.

---

## ⚙️ DAX Measures (Key Calculations)

```dax
-- Total Trade Value
Total Trade Value = SUM(FactTrades[TradeValue])

-- Brokerage Revenue
Brokerage Revenue = SUM(FactTrades[BrokerageAmount])

-- Previous Year Trade Value
PY Trade Value = CALCULATE([Total Trade Value], SAMEPERIODLASTYEAR(DimDate[Date]))

-- YoY Growth %
YoY Growth % = DIVIDE([Total Trade Value] - [PY Trade Value], [PY Trade Value], 0)

-- Active Clients
Active Clients = DISTINCTCOUNT(FactTrades[ClientID])

-- Total Trades
Total Trades = COUNT(FactTrades[TradeID])

-- Total Orders
Total Orders = COUNT(FactOrders[OrderID])
```

---

## 🗂️ Project File Structure

```
Securities_Operations_Analytics_Hub/
│
├── Securities_Operations_Analytics_Hub.pbix   # Main Power BI report file
├── README.md                                   # Project documentation (this file)
│
├── /SQL/
│   ├── schema.sql                              # Database schema definition
│   ├── fact_trades.sql                         # Fact table query
│   └── dim_tables.sql                          # Dimension table queries
│
└── /Assets/
    └── dashboard_preview.png                   # Dashboard screenshot
```

---

## 🚀 Getting Started

### Prerequisites
- Power BI Desktop (latest version recommended)
- Access to the SQL Server instance hosting the securities database
- Appropriate database read permissions

### Setup Instructions

1. **Clone or download** this repository to your local machine.
2. **Open** `Securities_Operations_Analytics_Hub.pbix` in Power BI Desktop.
3. Navigate to **Home → Transform Data → Data Source Settings**.
4. Update the **SQL Server** connection string with your server name and database credentials.
5. Click **Refresh** to load the latest data.
6. **Publish** to Power BI Service (app.powerbi.com) for sharing and scheduling refreshes.

### Scheduled Refresh (Power BI Service)
1. Publish the report to your Power BI Workspace.
2. Configure a **Gateway connection** to your on-premises SQL Server.
3. Set up a **Scheduled Refresh** (recommended: daily or real-time via DirectQuery).

---

## 🔐 Security & Access Control

- **Row-Level Security (RLS)** is implemented by Region and Branch to restrict data visibility based on user roles.
- Roles defined:
  - `National_Manager` — Full access
  - `Regional_Manager_East/West/North/South` — Region-specific access
  - `Branch_Manager` — Branch-level access only

---

## 🛠️ Technology Stack

| Tool | Purpose |
|---|---|
| **Power BI Desktop** | Report development & visualization |
| **Microsoft SQL Server** | Primary data source |
| **DAX** | Calculated measures and KPIs |
| **Power Query (M)** | Data transformation and cleansing |
| **Power BI Service** | Publishing, sharing & scheduled refresh |

---

## 📬 Contact & Ownership

| Field | Details |
|---|---|
| **Project Owner** | Securities Operations Team |
| **Dashboard Developer** | BI & Analytics Team |
| **Data Steward** | SQL Server DBA Team |
| **Last Updated** | March 2026 |

---

## 📝 Version History

| Version | Date | Changes |
|---|---|---|
| v1.0 | March 2026 | Initial release with executive overview dashboard |

---

*For issues, enhancements, or access requests, please contact the BI & Analytics Team.*
