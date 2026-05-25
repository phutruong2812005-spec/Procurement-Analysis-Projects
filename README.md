# 📊 Procurement Analytics Dashboard — Power BI

> **End-to-end procurement spend analysis** covering 5,200 purchase orders (2022–2024) across 20+ suppliers, 10 departments, and 4 global regions — built with Power BI.
---

## 📌 Table of Contents

- [Business Context](#-business-context)
- [Key Questions Explored](#-key-questions-explored)
- [Dataset Overview](#-dataset-overview)
- [Data Model](#-data-model)
- [Dashboard Pages](#-dashboard-pages)
- [Key Measures (DAX)](#-key-measures-dax)
- [Key Insights & Findings](#-key-insights--findings)
- [Technical Setup](#-technical-setup)
- [Project Structure](#-project-structure)
- [Future Enhancements](#-future-enhancements)
- [Author](#-author)

---

## 🏢 Business Context

The **Procurement (Purchasing) Department** keeps an organization running smoothly by acquiring the goods and services needed for daily operations. In practice, **cost escalation**, **supplier risk**, **delivery delays**, and **off-contract (maverick) spending** can create budget pressure and operational inefficiency.

This project delivers an interactive Power BI dashboard that enables procurement leaders to:

- **Monitor spend patterns** across categories, departments, and regions
- **Evaluate supplier performance** — on-time delivery, lead times, and ESG scores
- **Track budget vs. actuals** to identify savings and overspend
- **Detect compliance risks** — maverick spend, single-source dependency, and invoice matching issues

---

## 🔍 Key Questions Explored

| Area | Questions |
|---|---|
| **Spend Trends** | How has total procurement spend changed over time (monthly, quarterly, yearly)? |
| **Budget Allocation** | Which categories, departments, suppliers, or regions consume the largest share of spend? |
| **Budget vs. Actual** | How does actual spend compare to budgeted amounts? Where are the biggest variances? |
| **Savings Analysis** | Where are the largest savings achieved vs. budget? Where is overspend occurring? |
| **Supplier Profile** | Which suppliers have the highest transaction values? How do they compare on risk, tier, region, and ESG score? |
| **Strategic Sourcing** | Is there a performance gap between preferred and non-preferred suppliers? What % of spend is single-source? |
| **Delivery Performance** | What is the on-time delivery (OTD) rate? Which suppliers/categories experience the most delays? |
| **Lead Times** | How do lead times vary by supplier type and category? Are delays linked to high-risk suppliers? |
| **Compliance** | How much maverick (off-policy) spend exists? How often are preferred suppliers bypassed? |

---

## 📂 Dataset Overview

| Property | Detail |
|---|---|
| **Source file** | `Dataset_Procurement Project.xlsx` |
| **Records** | 5,200 purchase order lines |
| **Time range** | January 2022 – December 2024 (3 years) |
| **Sheets** | `Data` · `Calendar` · `Vocabulary & Notes` |
| **Currencies** | GBP, EUR, USD, JPY, AUD (no FX conversion — local currency) |

### Data Sheet — 57 Fields

The dataset is organized into the following field groups:

<details>
<summary><b>📋 Order Information</b> (7 fields)</summary>

| Field | Type | Description |
|---|---|---|
| `PO Number` | Text | Unique Purchase Order identifier (Primary Key) |
| `PO Date` | Date | Date the PO was raised |
| `PO Year` | Integer | Calendar year from PO Date |
| `PO Quarter` | Text | Quarter (Q1–Q4) |
| `PO Month` | Text | Full month name |
| `PO Type` | Text | Standard · Emergency · Blanket · Contract |
| `PO Status` | Text | Open · Closed · Cancelled · Disputed |

</details>

<details>
<summary><b>🏭 Supplier Dimensions</b> (12 fields)</summary>

| Field | Type | Description |
|---|---|---|
| `Supplier ID` | Text | Unique supplier identifier |
| `Supplier Name` | Text | Legal name of the supplier |
| `Supplier Country` | Text | Country of registration |
| `Supplier Region` | Text | Europe · Asia · Americas · Oceania |
| `Supplier Tier` | Integer | 1 (Strategic) · 2 (Preferred) · 3 (Transactional) |
| `Supplier Status` | Text | Preferred · Approved · Conditional |
| `Supplier Risk` | Text | Low · Medium · High |
| `Supplier Latitude` | Decimal | Latitude for map visuals |
| `Supplier Longitude` | Decimal | Longitude for map visuals |
| `Supplier ESG Score` | Decimal | Sustainability score 0–100 (higher = better) |
| `Local International` | Text | Local (UK) vs. International |
| `Payment Terms` | Text | Net 30 · Net 45 · Net 60 |

</details>

<details>
<summary><b>📦 Item Dimensions</b> (5 fields)</summary>

| Field | Type | Description |
|---|---|---|
| `Item Code` | Text | Internal item code |
| `Item Description` | Text | Short item description |
| `Category` | Text | Spend category (10 categories) |
| `Sub Category` | Text | Granular item grouping |
| `Unit of Measure` | Text | EA · Box · KG · MTR · LTR · Day · Shipment |

</details>

<details>
<summary><b>💰 Financial Measures</b> (13 fields)</summary>

| Field | Type | Description |
|---|---|---|
| `Unit Price` | Decimal | Price per unit before discount |
| `Quantity` | Integer | Units ordered |
| `Discount Pct` | Integer | Discount percentage (0–15%) |
| `Discount Amount` | Decimal | Monetary value of discount |
| `Tax Pct` | Integer | Tax rate (0% · 5% · 20%) |
| `Tax Amount` | Decimal | Tax monetary value |
| `Line Total Gross` | Decimal | Unit Price × Quantity |
| `Line Net` | Decimal | Gross minus Discount |
| `Line Total Inc Tax` | Decimal | Net plus Tax — **primary spend metric** |
| `Currency` | Text | GBP · EUR · USD · JPY · AUD |
| `Budget Unit Price` | Decimal | Budgeted benchmark price |
| `Budget Total` | Decimal | Budget Unit Price × Quantity |
| `Savings Amount` | Decimal | Budget − Net (positive = saving) |
| `Savings Pct` | Decimal | Savings ÷ Budget × 100 |

</details>

<details>
<summary><b>🚚 Delivery Metrics</b> (5 fields)</summary>

| Field | Type | Description |
|---|---|---|
| `Requested Delivery` | Date | Originally requested delivery date |
| `Actual Delivery` | Date | Actual receipt date |
| `Days Late` | Integer | Actual − Requested (negative = early) |
| `On Time Delivery` | Text | Yes if Days Late ≤ 0 |
| `Lead Time Days` | Integer | PO Date to Actual Delivery |

</details>

<details>
<summary><b>🏢 Organization & Contract</b> (8 fields)</summary>

| Field | Type | Description |
|---|---|---|
| `Department` | Text | Business department (10 departments) |
| `Cost Centre` | Text | Financial cost centre code |
| `Requestor Name` | Text | Employee who raised the requisition |
| `Approver Name` | Text | Manager who approved the PO |
| `Contract ID` | Text | Framework contract reference (NO-CONTRACT if none) |
| `Contract Type` | Text | Framework · Master Supply · Spot · Long-term |
| `Contract Start` | Date | Contract effective start date |
| `Contract End` | Date | Contract expiry date |

</details>

<details>
<summary><b>⚠️ Compliance & Risk KPIs</b> (5 fields)</summary>

| Field | Type | Description |
|---|---|---|
| `Invoice Status` | Text | Paid · Pending · Overdue · Disputed |
| `Payment Status` | Text | Paid · Pending · Overdue · On Hold |
| `Invoice Match Type` | Text | 3-Way Match (best) · 2-Way Match · No Match |
| `Maverick Spend` | Text | Yes = bypassed procurement policy |
| `Single Source Flag` | Text | Yes = only one supplier considered |
| `Preferred Supplier` | Text | Yes = supplier holds preferred status |

</details>

### Calendar Sheet

A dedicated date dimension table with **1,096 rows** (2022–2024) containing:

- Standard calendar fields (Year, Quarter, Month, Week, Day)
- Weekend/Weekday flags
- **Fiscal Year** (UK standard — starts April 1) and **Fiscal Quarter**

---

## 🗂 Data Model

The report uses a **star schema** with the following tables:

```
                    ┌──────────────┐
                    │   Calendar   │
                    │  (Date Dim)  │
                    └──────┬───────┘
                           │
    ┌──────────────┐       │       ┌─────────────────────┐
    │ Dim_Supplier │───────┼───────│ Fact_PurchaseOrder   │
    └──────────────┘       │       │   (5,200 rows)       │
                           │       └───────┬─────┬────────┘
    ┌──────────────────┐   │               │     │
    │ Dim_Item_Category│───┘               │     │
    │ (Product Catalog)│                   │     │
    └──────────────────┘    ┌──────────────┘     │
                            │                    │
                    ┌───────┴──────┐    ┌────────┴───────┐
                    │Dim_Department│    │  _KeyMeasures  │
                    └──────────────┘    │  (DAX Measures)│
                                       └────────────────┘
```

**Key Relationships:**
- `Fact_PurchaseOrder[PO Date]` → `Calendar[Date]` (date intelligence)
- `Fact_PurchaseOrder` → `Dim_Supplier` (supplier attributes)
- `Fact_PurchaseOrder` → `Dim_Item_Category` (product hierarchy)
- `Fact_PurchaseOrder` → `Dim_Department` (organizational structure)

---

## 📊 Dashboard Pages

### 1️⃣ Executive Overview

> High-level financial summary for leadership — spend trends, budget performance, and category/department breakdown.

| Visual | Purpose |
|---|---|
| **KPI Cards (×4)** | Total Spend · Total Budget · Budget Variance · % Maverick Spend |
| **Line + Stacked Column Combo** | Monthly spend vs. budget trend over time |
| **Treemap** | Spend distribution by Category → Sub Category |
| **Clustered Column Chart** | Spend breakdown by Department |
| **Slicers** | Year/Quarter · Supplier Region |

---

### 2️⃣ Supplier Performance

> Operational view of supplier delivery, lead times, and supplier characteristics across the global supply base.

| Visual | Purpose |
|---|---|
| **KPI Cards (×4)** | Total Spend · Total POs · Avg Lead Time · Avg Days Late |
| **Scatter Chart** | Avg Lead Time vs. % On-Time Delivery by Supplier |
| **Map** | Geographic distribution of spend by supplier location |
| **Matrix / Pivot Table** | Supplier scorecard — Tier, ESG Score, key metrics |
| **Slicers** | Year/Quarter · Supplier Region |

---

### 3️⃣ Compliance & Risk

> Risk-focused view tracking policy adherence, single-source dependency, and preferred supplier utilization.

| Visual | Purpose |
|---|---|
| **KPI Cards (×4)** | Total Spend · % Savings · % Maverick Spend · % Single Source Spend |
| **Donut Chart** | Single Source vs. Multi-Source spend split |
| **Donut Chart** | Maverick vs. Compliant spend split |
| **100% Stacked Column** | Preferred vs. Non-Preferred supplier spend trend over time |
| **Slicers** | Year/Quarter · Supplier Region (×2) |

---

## 📐 Key Measures (DAX)

All measures are organized in a dedicated `_KeyMeasures` table:

| Measure | Description |
|---|---|
| `Total Spend` | Sum of `Line Total Inc Tax` — primary spend metric |
| `Total Budget` | Sum of `Budget Total` |
| `Budget Variance` | Total Budget − Total Spend |
| `% Savings` | Total Savings ÷ Total Budget × 100 |
| `Total POs` | Count of distinct Purchase Orders |
| `% On-Time Delivery` | POs delivered on time ÷ Total POs |
| `Avg Lead Time` | Average days from PO creation to delivery |
| `Avg Day Late` | Average days late across all POs |
| `Avg ESG Score` | Average supplier sustainability score |
| `% Maverick Spend` | Maverick spend ÷ Total Spend × 100 |
| `% Single Source Spend` | Single-source spend ÷ Total Spend × 100 |

---

## 💡 Key Insights & Findings

| Insight | Detail |
|---|---|
| **Spend Concentration** | Top categories and departments drive the majority of total procurement spend — highlighting opportunities for strategic sourcing and bulk negotiation |
| **Budget Variance** | Notable variance between budgeted and actual spend exists across certain categories, signaling forecasting gaps |
| **Supplier Risk** | Some high-value suppliers carry Medium/High risk ratings, requiring risk mitigation strategies |
| **On-Time Delivery** | OTD rates vary significantly by supplier and region — high-risk and international suppliers tend to have longer lead times |
| **Maverick Spend** | A measurable portion of purchases bypass procurement policy, increasing compliance risk and reducing negotiated savings |
| **Single Source Dependency** | A portion of spend relies on single-source suppliers, creating supply chain vulnerability |
| **ESG Scores** | Supplier sustainability scores range widely (0–100), providing a basis for ESG-weighted supplier selection |

---

## ⚙️ Technical Setup

### Prerequisites

- **Power BI Desktop** (June 2023 or later recommended)
- Windows OS

### How to Use

1. **Clone** this repository:
   ```bash
   git clone https://github.com/<your-username>/procurement-analytics-powerbi.git
   ```

2. **Open** `Procurement Projects.pbix` in Power BI Desktop

3. **Refresh** data if needed — the Excel dataset (`Dataset_Procurement Project.xlsx`) must be in the same directory

4. **Interact** with slicers (Year, Quarter, Region) to filter across all pages

### Data Refresh

The data source is a static Excel file. To update:
1. Replace `Dataset_Procurement Project.xlsx` with updated data (same schema)
2. Click **Refresh** in Power BI Desktop
3. Verify relationships in Model View

---

## 👤 Author

**Trương Tấn Phú**
- The Dataset was from Xóm Data (https://www.facebook.com/share/g/1BDSaa3HoF/) 
- 💼 [LinkedIn](https://linkedin.com/in/your-profile)
---

> *This project is part of the **PL-300 Microsoft Power BI Data Analyst** certification preparation. The dataset is simulated for educational purposes.*

---

⭐ **If you found this project helpful, please give it a star!**
