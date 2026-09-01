# Freight Operations Intelligence Hub

<div align="center">

![Power BI](https://img.shields.io/badge/Power%20BI-F2C811?style=for-the-badge&logo=powerbi&logoColor=black)
![Microsoft Fabric](https://img.shields.io/badge/Microsoft%20Fabric-0078D4?style=for-the-badge&logo=microsoft&logoColor=white)
![PySpark](https://img.shields.io/badge/PySpark-E25A1C?style=for-the-badge&logo=apachespark&logoColor=white)
![Delta Lake](https://img.shields.io/badge/Delta%20Lake-003087?style=for-the-badge&logo=databricks&logoColor=white)
![SQL](https://img.shields.io/badge/SQL-4479A1?style=for-the-badge&logo=microsoftsqlserver&logoColor=white)

**An end-to-end LTL freight analytics platform built on Microsoft Fabric**  
*Replicating real-world freight BI work: carrier SLA tracking, cost analysis, claims risk, and lane profitability*

[View Live Dashboard](#) &nbsp;|&nbsp; [Notebooks](#notebooks) &nbsp;|&nbsp; [Architecture](#architecture) &nbsp;|&nbsp; [KPIs Built](#kpis-tracked)

</div>

---

## Table of Contents

- [Business Context](#business-context)
- [Dashboard Preview](#dashboard-preview)
- [Architecture](#architecture)
- [Tech Stack](#tech-stack)
- [Dataset](#dataset)
- [Project Structure](#project-structure)
- [Notebooks](#notebooks)
- [Gold Layer Tables](#gold-layer-tables)
- [KPIs Tracked](#kpis-tracked)
- [SQL Analytics Queries](#sql-analytics-queries)
- [How to Reproduce](#how-to-reproduce)
- [Certifications](#certifications)
- [Connect](#connect)

---

## Business Context

LTL (Less-Than-Truckload) freight carriers like XPO Logistics manage thousands of daily shipments across complex carrier networks spanning multiple lanes, service types, and industries. A freight BI team answers operational questions every day:

- Which carriers are failing their on-time delivery SLAs and by how much?
- Which lanes generate the highest revenue and which have the worst transit variance?
- Where is claims exposure concentrated and which carriers represent the highest risk?
- How does freight cost per mile differ across service types?

This project builds an end-to-end analytics platform that answers exactly those questions using Microsoft Fabric's medallion lakehouse architecture, PySpark notebooks, Delta Lake tables, and a four-page Power BI dashboard.

---

## Dashboard Preview

### Page 1: Executive Summary
<!-- HOW TO ADD: Upload your screenshot to the /screenshots folder in this repo, then the image will display automatically -->
![Executive Summary](screenshots/page1_executive.png)

### Page 2: Carrier Performance Scorecard
<!-- HOW TO ADD: Same as above - upload page2_carrier.png to /screenshots folder -->
![Carrier Scorecard](screenshots/page2_carrier.png)

### Page 3: Freight Lane Performance Analysis
<!-- HOW TO ADD: Upload page3_lane.png to /screenshots folder -->
![Lane Performance](screenshots/page3_lane.png)

### Page 4: 2024 Freight Operational Trends
<!-- HOW TO ADD: Upload page4_trends.png to /screenshots folder -->
![Operational Trends](screenshots/page4_trends.png)

> **Live Report:** [Click here to view the interactive Power BI dashboard](#)  
> <!-- HOW TO ADD LINK: Replace the # above with your published Power BI report URL -->
> <!-- To get the URL: In Power BI Service, open your report, click File > Embed report > Website or portal, copy the link -->

---

## Architecture

```
Raw Source Files (CSV + JSON)
        │
        ▼
┌─────────────────────────────┐
│      BRONZE LAYER           │
│  Lakehouse Files Section    │
│  shipments_raw.csv          │
│  carrier_master.json        │
└────────────┬────────────────┘
             │  NB_01_Bronze_DataGeneration.ipynb
             ▼
┌─────────────────────────────┐
│      SILVER LAYER           │
│  PySpark Transformations    │
│  silver_shipments (Delta)   │
│  - Date parsing             │
│  - KPI column derivation    │
│  - Carrier master join      │
└────────────┬────────────────┘
             │  NB_02_Silver_Transform.ipynb
             ▼
┌─────────────────────────────┐
│       GOLD LAYER            │
│  Business Aggregations      │
│  gold_carrier_scorecard     │
│  gold_monthly_trend         │
│  gold_lane_performance      │
│  gold_industry_analysis     │
└────────────┬────────────────┘
             │  NB_03_Gold_Aggregations.ipynb
             ▼
┌─────────────────────────────┐
│   SQL ANALYTICS ENDPOINT    │
│  4 business SQL queries     │
│  Carrier SLA breach check   │
│  Top lane revenue ranking   │
└────────────┬────────────────┘
             │
             ▼
┌─────────────────────────────┐
│   POWER BI SEMANTIC MODEL   │
│  FreightOps_SemanticModel   │
│  6 DAX measures             │
│  Relationships defined      │
└────────────┬────────────────┘
             │
             ▼
┌─────────────────────────────┐
│   POWER BI REPORT           │
│  4 pages, 14 visuals        │
│  Conditional formatting     │
│  Slicers, reference lines   │
└─────────────────────────────┘
```

---

## Tech Stack

| Layer | Tool | Purpose |
|-------|------|---------|
| Platform | Microsoft Fabric (Trial) | Unified analytics platform |
| Storage | Fabric Lakehouse | Delta Lake storage, Files + Tables |
| Ingestion | Python (pandas) | Synthetic data generation |
| Transformation | PySpark Notebooks | Bronze to Silver ETL |
| Aggregation | Spark SQL | Silver to Gold aggregation |
| Querying | SQL Analytics Endpoint | Business SQL queries |
| Modeling | Power BI Semantic Model | DAX measures, relationships |
| Visualization | Power BI Report | 4-page interactive dashboard |
| Data Formats | CSV, JSON | Multi-format ingestion demo |

---

## Dataset

- **10,000 synthetic LTL shipment records** covering full year 2024
- **5 carriers:** XPO Logistics, Estes Express, Old Dominion, ABF Freight, Saia LTL
- **10 origin-destination lanes** across major US freight corridors
- **7 industries:** Manufacturing, Retail, Healthcare, Automotive, Food & Beverage, Electronics, Chemical
- **4 service types:** LTL Standard, LTL Expedited, LTL Economy, Volume LTL
- **Data formats:** CSV (shipments), JSON (carrier master with SLA thresholds)
- Generated entirely via Python inside Microsoft Fabric — no external data source required

---

## Project Structure

```
freight-ops-intelligence-hub/
│
├── notebooks/
│   ├── NB_01_Bronze_DataGeneration.ipynb    # Synthetic data generation, Bronze layer
│   ├── NB_02_Silver_Transform.ipynb         # PySpark ETL, Silver Delta table
│   └── NB_03_Gold_Aggregations.ipynb        # Gold aggregation tables via Spark SQL
│
├── screenshots/
│   ├── page1_executive.png                  # Executive Summary dashboard
│   ├── page2_carrier.png                    # Carrier Performance Scorecard
│   ├── page3_lane.png                       # Lane Performance Analysis
│   └── page4_trends.png                     # Operational Trends
│
├── pbix/
│   └── Freight_Ops_Intelligence_Hub.pbix    # Power BI Desktop file
│   <!-- HOW TO ADD: Download your .pbix from Power BI Desktop via File > Save a copy -->
│   <!-- Then upload it to the /pbix folder in this GitHub repo -->
│
└── README.md
```

---

## Notebooks

### NB_01: Bronze Data Generation
Generates 10,000 synthetic shipment records and carrier master data using Python and pandas. Saves files to the Fabric Lakehouse Files section in Bronze layer format.

**Key outputs:**
- `Files/bronze/shipments_raw.csv` — 10,000 rows, 20 columns
- `Files/bronze/carrier_master.json` — 5 carriers with SLA thresholds

### NB_02: Silver Transformation
Reads Bronze CSV and JSON using PySpark. Applies type casting, derives calculated columns, joins carrier master data, and writes a clean Delta table.

**Key transformations:**
```python
# Derived columns added in Silver layer
.withColumn("transit_variance_days", col("actual_transit_days") - col("promised_transit_days"))
.withColumn("gross_margin_usd", col("freight_revenue_usd") - col("freight_cost_usd"))
.withColumn("gross_margin_pct", round((col("gross_margin_usd") / col("freight_revenue_usd")) * 100, 2))
.withColumn("cost_per_mile", round(col("freight_cost_usd") / col("distance_miles"), 4))
.withColumn("lane", concat_ws(" -> ", col("origin_city"), col("destination_city")))
```

### NB_03: Gold Aggregations
Creates four business-ready Gold Delta tables using Spark SQL. Each table serves a specific reporting need.

**Sample Gold query:**
```sql
CREATE OR REPLACE TABLE gold_carrier_scorecard AS
SELECT
    carrier_name,
    carrier_id,
    MAX(sla_otdr_pct)                                                           AS sla_otdr_threshold,
    COUNT(*)                                                                     AS total_shipments,
    ROUND(AVG(CASE WHEN shipment_status='Delivered' THEN on_time_flag END)*100,1) AS actual_otdr_pct,
    ROUND(SUM(freight_revenue_usd), 2)                                           AS total_revenue_usd,
    ROUND(SUM(gross_margin_usd), 2)                                              AS total_gross_margin_usd,
    ROUND(AVG(gross_margin_pct), 2)                                              AS avg_margin_pct,
    ROUND(AVG(cost_per_mile), 4)                                                 AS avg_cost_per_mile,
    SUM(has_claim)                                                               AS total_claims,
    ROUND(SUM(claim_amount_usd), 2)                                              AS total_claim_amount_usd,
    ROUND(SUM(has_claim) * 100.0 / COUNT(*), 2)                                  AS claims_rate_pct,
    ROUND(AVG(transit_variance_days), 2)                                         AS avg_transit_variance_days
FROM silver_shipments
GROUP BY carrier_name, carrier_id
```

---

## Gold Layer Tables

| Table | Rows | Description |
|-------|------|-------------|
| `gold_carrier_scorecard` | 5 | OTDR vs SLA, cost/mile, margin, claims per carrier |
| `gold_monthly_trend` | 60 | Monthly volume, revenue, OTDR by carrier and service type |
| `gold_lane_performance` | 10 | Revenue, cost, transit time by origin-destination lane |
| `gold_industry_analysis` | 28 | Revenue and margin by industry and service type |

---

## KPIs Tracked

| KPI | Definition | Business Use |
|-----|-----------|--------------|
| **OTDR %** | % of delivered shipments arriving on or before promised date | Carrier SLA compliance monitoring |
| **SLA Breach** | Carriers where OTDR % is below contracted SLA threshold | Carrier performance management |
| **Cost per Mile** | Freight cost USD divided by shipment distance in miles | Cost efficiency benchmarking |
| **Gross Margin %** | (Revenue - Cost) / Revenue × 100 | Profitability by carrier and lane |
| **Claims Rate %** | Total claims / Total shipments × 100 | Risk exposure tracking |
| **Transit Variance** | Actual transit days minus promised transit days | Delivery reliability measurement |
| **Lane Revenue** | Total freight revenue by origin-destination pair | Network profitability analysis |

---

## SQL Analytics Queries

Four analytical queries were built on the Fabric SQL Analytics Endpoint:

**Query 1 — Carrier SLA Compliance Check**
```sql
SELECT
    carrier_name,
    actual_otdr_pct,
    sla_otdr_threshold,
    CASE WHEN actual_otdr_pct >= sla_otdr_threshold 
         THEN 'SLA Met' ELSE 'SLA Breached' END AS sla_status,
    claims_rate_pct,
    avg_cost_per_mile
FROM gold_carrier_scorecard
ORDER BY actual_otdr_pct DESC;
```

**Query 2 — Top 5 Revenue Lanes**
```sql
SELECT TOP 5 lane, total_revenue_usd, otdr_pct, avg_cost_per_mile
FROM gold_lane_performance
ORDER BY total_revenue_usd DESC;
```

**Query 3 — Monthly OTDR Trend**
```sql
SELECT year_month,
       ROUND(AVG(otdr_pct), 1) AS avg_otdr_pct,
       SUM(total_revenue_usd)  AS monthly_revenue
FROM gold_monthly_trend
GROUP BY year_month
ORDER BY year_month;
```

**Query 4 — Claims Exposure by Carrier**
```sql
SELECT carrier_name, total_claims, total_claim_amount_usd, claims_rate_pct
FROM gold_carrier_scorecard
ORDER BY total_claim_amount_usd DESC;
```

---

## DAX Measures

Six DAX measures were created in the Power BI Semantic Model:

```dax
Network OTDR % =
DIVIDE(
    SUMX(gold_carrier_scorecard,
         gold_carrier_scorecard[actual_otdr_pct] * gold_carrier_scorecard[delivered_count]),
    SUM(gold_carrier_scorecard[delivered_count])
)

Total Revenue = SUM(gold_carrier_scorecard[total_revenue_usd])

Total Gross Margin = SUM(gold_carrier_scorecard[total_gross_margin_usd])

Total Shipments = SUM(gold_carrier_scorecard[total_shipments])

Network Claims Rate % =
DIVIDE(
    SUM(gold_carrier_scorecard[total_claims]),
    SUM(gold_carrier_scorecard[total_shipments])
) * 100

SLA Breached Carriers =
CALCULATE(
    COUNTROWS(gold_carrier_scorecard),
    gold_carrier_scorecard[actual_otdr_pct] < gold_carrier_scorecard[sla_otdr_threshold]
)
```

---

## How to Reproduce

**Prerequisites:**
- Microsoft Fabric Trial account (free at app.fabric.microsoft.com)
- No external data required — all data is generated inside Fabric

**Steps:**

1. Sign in to [app.fabric.microsoft.com](https://app.fabric.microsoft.com)
2. Create a new Workspace named `FreightOps-Intelligence-Hub`
3. Create a Lakehouse named `freight_lakehouse`
4. Upload and run the three notebooks in order:
   - `NB_01_Bronze_DataGeneration.ipynb`
   - `NB_02_Silver_Transform.ipynb`
   - `NB_03_Gold_Aggregations.ipynb`
5. Open the SQL Analytics Endpoint and run the four queries in `Gold tables`
6. Create a new Semantic Model selecting all four Gold tables
7. Open the `.pbix` file or build the Power BI report from the Semantic Model

---

## Certifications

| Certification | Issuer | Status |
|--------------|--------|--------|
| DP-600: Fabric Analytics Engineer Associate | Microsoft | Passed |
| PL-300: Power BI Data Analyst Associate | Microsoft | Passed |

---

## Connect

**Anand Cinenkanolu**  
Data Analyst | Power BI Developer | Microsoft Fabric Analytics Engineer

<!-- HOW TO ADD YOUR LINKS: Replace the # symbols below with your actual URLs -->

[![LinkedIn](https://img.shields.io/badge/LinkedIn-0077B5?style=for-the-badge&logo=linkedin&logoColor=white)](#)
[![GitHub](https://img.shields.io/badge/GitHub-100000?style=for-the-badge&logo=github&logoColor=white)](#)
[![Power BI Report](https://img.shields.io/badge/Live%20Dashboard-F2C811?style=for-the-badge&logo=powerbi&logoColor=black)](#)

---

*Built on Microsoft Fabric Trial | Data is synthetic and generated for portfolio demonstration purposes*
