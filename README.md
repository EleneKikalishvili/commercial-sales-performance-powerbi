# FloraGlobal Distribution: Commercial Sales Performance

**Analyst:** Elene Kikalishvili  
**Date:** July 2026  
**Tools Used:** Power BI Desktop, DAX (Data Analysis Expressions), Power Query  

---

## Table of Contents
- [Executive Summary](#executive-summary)
- [Business Context](#business-context)
- [Business Questions Answered](#business-questions-answered)
- [Methodology](#methodology)
- [Skills & Tools Demonstrated](#skills--tools-demonstrated)
- [Analytical Architecture & DAX Highlights](#analytical-architecture--dax-highlights)
- [Results & Executive Insights](#results--executive-insights)
- [Next Steps & Future Enhancements](#next-steps--future-enhancements)

---

## Executive Summary  

### The Company & Business Problem
**FloraGlobal Distribution** is a global B2B wholesale supplier of horticultural and landscaping products. Due to specialized freight overhead, heavy product weights, and regional shipping variables, the company faced hidden margin erosion. Because legacy reporting relied on static files that only tracked top-line revenue, these high delivery and operational costs were completely obscured behind large wholesale order volumes, leaving leadership unable to identify which transactions were actually losing money.

### The Solution
To resolve this, I built a dynamic **Sales Performance Dashboard** in Power BI utilizing a centralized star schema to model transactional records alongside product, date, and account dimensions. By deploying a disconnected parameter table, the dashboard enables users to instantly toggle the entire report canvas between **Sales Revenue**, **Gross Profit**, and **Unit Volume**. A custom DAX time-intelligence layer handles Year-to-Date (YTD) and Prior Year-to-Date (PYTD) metrics, ensuring accurate parallel-period variance comparisons across the active reporting timeline. 

### Data-Driven Next Steps
Based on the dashboard's analytical and diagnostic visuals, commercial leadership can immediately execute three strategic actions:
1. **Address Low-Margin Accounts:** Audit and renegotiate contract pricing for high-volume, low-margin wholesale accounts isolated in the underperforming zones of the profitability scatter plot.
2. **Optimize Product Mix:** Identify and redirect sales focus toward high-margin, lower-volume accounts to capture profitable growth where margins are already optimized.
3. **Remedial Action for Key Regions:** Target the specific global territories flagged by the bottom-performing ranking visual (such as Canada and Germany) to investigate and correct severe monthly performance anomalies.

---

## Business Context  

FloraGlobal Distribution required a reliable, automated tool to evaluate Year-to-Date (YTD) sales performance against historical baselines. Prior reporting relied on static flat files that lacked consistent time-intelligence comparisons and failed to map shipping volumes against actual profit margins.

**Key Objectives:**
- Centralize transactional sales logs with product, date, and customer account dimensions into a single source of truth.
- Establish a normalized framework for tracking YTD and Prior Year-to-Date (PYTD) metrics automatically.
- Provide dynamic switching capabilities allowing users to toggle the entire dashboard between revenue, gross profit, and unit volume metrics.
- Segment international markets to separate highly profitable distribution lanes from high-volume, low-margin accounts.

---

## Business Questions Answered

This application allows commercial managers and supply chain directors to directly answer the following operational questions:
1. **What is our exact year-to-date performance variance compared to the same calendar window last year?** The primary KPI blocks and the monthly waterfall visualization instantly reconcile the exact net progression or retraction.
2. **Which global regions are driving the largest negative performance gaps?** The geographical variance matrix isolates the specific country markets creating the heaviest drag on current-year performance.
3. **Are there major wholesale accounts that generate high sales volume but deliver unacceptable profit margins?** The profitability segmentation scatter plot exposes specific client entities falling into the high-volume, low-margin quadrant, highlighting exactly where distribution costs may be outstripping contract values.

---

## Methodology   

The project was developed end-to-end within Power BI, focusing on clean data preparation, an optimized relational model, and dynamic DAX expressions:

### 1. Power Query ETL & Data Cleaning
- Imported transactional tables and dimensions into Power Query to standardize data types and fix naming anomalies.
- Wrote a custom date-boundary flag in M code to automatically restrict the reporting calendar to the maximum available transaction date, preventing future-date skewing.

### 2. Star Schema Data Modeling
- Designed a centralized star schema database design to optimize performance.
- Established clean, one-to-many relationships from the dimension tables (`Dim_Date`, `Dim_Account`, `Dim_Product`) to the core transaction log (`Fact_Sales`). 
- Enforced single-direction filter propagation throughout the model to ensure strict calculation accuracy and eliminate relationship ambiguity.

### 3. DAX Analytics Layer
- Created a disconnected metric selection table (`Slc_Values`) to act as the dynamic parameter toggle for the user interface.
- Wrote core base measures (Revenue, Cost, Gross Profit) and nested them inside dynamic `SWITCH` selection blocks for standardizing Year-to-Date (YTD) and Prior Year-to-Date (PYTD) logic.
- Managed the time-intelligence calculations to ensure parallel periods aligned correctly across months, automatically suppressing blank historical periods.

### 4. Dashboard Design & Dynamic UX
- Structured an executive layout focused on scannability, pairing high-level KPIs with detailed trend breakdowns.
- Developed conditional DAX text measures to dynamically rewrite visual titles, visual headers, and chart axes based on the user's active metric selection.

---

## Skills & Tools Demonstrated  

- **Data Modeling:** Star Schema design, unidirectional filter propagation, one-to-many relationship optimization.
- **Advanced DAX Development:** Dynamic metric switching (disconnected tables), time-intelligence calculations (YTD/PYTD), and dynamic conditional formatting.
- **Data Preparation (Power Query):** Custom M code date-boundary flags, ETL transformations, data type standardization, and structural anomaly resolution.
- **Business Analytics:** Profitability quadrant analysis (scatter plots), year-over-year performance variance reporting, and global margin tracking.

---

## Analytical Architecture & DAX Highlights

The data model's semantic layer separates core business aggregations, time-intelligence calculations, and user-driven metric parameters to ensure optimal performance and accuracy.

### 1. Base Core Measures
These foundational calculations establish the core transactional metrics before any chronological or conditional logic is applied:

```dax
Sales = SUM(Fact_Sales[Sales_USD])
```
```dax
Quantity = SUM(Fact_Sales[Quantity])
```
```dax
COGs = SUM(Fact_Sales[COGS_USD])
```
```dax
Gross Profit = [Sales] - [COGs]
```
```dax
GP% = DIVIDE([Gross Profit], [Sales])
```

### 2. Time-Intelligence Calculations
To ensure accurate year-over-year comparisons and prevent trending charts from projecting flat lines into future months with no transaction data, custom date boundaries were implemented:

```dax
YTD_GrossProfit = 
VAR LastDayOfSales = CALCULATE(MAX(Fact_Sales[Order_Date]), ALL(Fact_Sales))
RETURN
    IF(
        MIN(Dim_Date[Date]) <= LastDayOfSales,
        CALCULATE([Gross Profit], DATESYTD(Dim_Date[Date])),
        BLANK()
    )
```

```dax
PYTD_GrossProfit = 
CALCULATE(
    [Gross Profit],
    SAMEPERIODLASTYEAR(Dim_Date[Date]),
    Dim_Date[Inpast] = TRUE
)
```

### 3. Dynamic Metric Switching Engine
The dashboard utilizes two core routing measures to intercept user selections on the `Slc_Values` table, instantly rebuilding the active data profiles for both the current and prior periods:

```dax
S_YTD = 
VAR selected_value = SELECTEDVALUE(Slc_Values[Values])
VAR result = SWITCH(selected_value,
    "Sales", [YTD_Sales],
    "Quantity", [YTD_Quantity],
    "Gross Profit", [YTD_GrossProfit],
    BLANK()
)
RETURN
    result
```

```dax
S_PYTD = 
VAR selected_value = SELECTEDVALUE(Slc_Values[Values])
VAR result = SWITCH(selected_value,
    "Sales", [PYTD_Sales],
    "Quantity", [PYTD_Quantity],
    "Gross Profit", [PYTD_GrossProfit],
    BLANK()
)
RETURN
    result
```

```dax
YTD vs PYTD = [S_YTD] - [S_PYTD]
```

### 4. Dynamic Visual Titles
To keep the report layout intuitive when users toggle perspectives, separate text measures dynamically rewrite the visual titles and headers based on the active selection:

```dax
_Report title = "FloraGlobal " & SELECTEDVALUE(Slc_Values[Values]) & " Performance " & SELECTEDVALUE(Dim_Date[Date].[Year])
```
```dax
_Column Chart title = SELECTEDVALUE(Slc_Values[Values]) & " YTD & PYTD | Month"
```
```dax
_Waterfall title = SELECTEDVALUE(Slc_Values[Values]) & " YTD vs PYTD | Month - Country - Product"
```
```dax
_Scatter title = "Account Profitability Segmentation | GP% and " & SELECTEDVALUE(Slc_Values[Values])
```

---

## Results & Executive Insights

A comprehensive analysis of FloraGlobal’s 2024 active performance reveals a critical inflection point: a highly profitable, high-volume opening quarter was completely neutralized by compounding operational and demand headwinds in Q2. 

### 1. Macro Organizational Health & Portfolio Resilience
* **The Volume-to-Profit Correlation:** Total Year-to-Date (YTD) product volume shipped contracted by **-12.37K units** (down to **148.47K** vs. a **160.84K** PYTD baseline) as seen in. This volume drop directly drove a **-$77.62K** contraction in YTD Gross Profit, bringing the bottom line down to **$1.40M**.
* **Margin Stability:** Despite the drop in absolute volume and profit dollars, the global portfolio maintained a rock-solid, consistent Gross Profit Margin (**GP%**) of **39.15%**. This tells us that the downturn was not caused by aggressive company-wide price slashing or discounting, but rather by localized demand drops and supply chain disruptions.
* **The Late-Quarter Performance Shift:** The business experienced two starkly contrasting phases across the 4-month reporting window:
  * **Early Q1 Growth Surge:** The year started with strong momentum. February achieved a massive performance expansion, pushing monthly volume up by +8.2K units and expanding Gross Profit by +$117K over the prior year's baseline.
  * **March & April Volatility:** In March, the momentum broke completely, closing out Q1 with a monthly Gross Profit plunge of -$96K on a volume drop of -11.6K units. April compounded the issue to start the next period, dropping another -$90K in profit and bottoming out at just 18.20K units shipped (against a historical parallel of 27.62K units).

### 2. Deep-Dive: Monthly Variance & Geographic Hotspots

By leveraging the dashboard's cross-filtering capabilities, we can move past high-level monthly trends and isolate the exact territorial distribution channels responsible for our sharpest commercial erosions:

* **The March Euro-Asian Bottleneck:** The severe global Gross Profit drop seen in March (-$96K) was driven almost entirely by synchronized margin collapses across Europe and Asia. **Poland** represented the most severe localized drag with a **-$29.24K** profit contraction, followed closely by intensive margin erosion in **Germany (-$24.78K)**, **Portugal (-$24.65K)**, and **China (-$23.76K)**. 
* **The April Canadian Crater:** In April, the operational risk shifted entirely to our North American lane. Isolating **Canada** reveals that after a stable first quarter, its commercial performance suffered an immediate **-$39K drop in April alone**—responsible for nearly half of the entire global deficit for that month. This abrupt collapse points to an immediate need for an audit of localized logistics, unexpected freight expenditures, or bulk-account contract pricing in the Canadian corridor.
* **Macro YTD Drags:** Looking at the full-year macro trajectory without active monthly filters, **Canada** and **Germany** remain the primary corporate priority areas. Canada anchors the global bottom-performer list with a cumulative **-$41.59K Gross Profit deficit**, while Germany follows as the second largest structural drag at a net **-$25.51K**.

### 3. Account Portfolio Profitability & Structural Risks

The **Account Profitability Segmentation Matrix** exposes key structural realities within our wholesale B2B client base, mapping physical volumetric contributions against account margin efficiency:

* **The High-Volume Whale Divergence:** The scatter plot reveals a critical strategic split among our highest-capacity buyers (those purchasing between 1.5K and 2.5K units). While three of these high-volume accounts operate as ideal "champions" well above the 50% margin mark, the visual flags a dangerous downside risk: multiple high-volume accounts drop below the **39.15%** corporate floor. Most notably, a massive operational outlier sinks down to **26% GP%**, proving that high physical distribution volume is actively masking severe margin erosion.
* **The High-Margin Upsell Cluster:** Conversely, a dense, highly efficient cluster of accounts sits securely in the upper-left quadrant, bounded to the left of the dynamic baseline crosshair. These accounts order lower physical volumes (predominantly under 650 units) but boast exceptional, premium margin profiles tracking between **60% and 70% GP%**. This represents a clear, risk-free tactical runway for the commercial team to incentivize bulk ordering and scale transaction sizes within an already highly profitable client pool.

---

## Next Steps & Future Enhancements

To build on this dashboard and make it more valuable for everyday business decisions, the next phases of the project would focus on automating the backend workflow and adding a few practical tracking features:

### 1. Automate Data Collection & ETL
* **Automate Data Refreshes:** Transition from manual file loading to automated scheduled refreshes in Power BI, ensuring the dashboard updates daily without needing manual data imports.
* **Centralize Staging:** Move the messy source flat files into a proper database or a single structured network folder to create a much cleaner, more stable Power Query ETL pipeline.

### 2. Expand Operational Tracking
* **Integrate Shipping Costs:** Add basic logistics and freight expense columns to the sales data. This will let the dashboard show exactly whether the profit drops in Canada and Portugal were caused by drop-offs in sales or spikes in delivery costs.
* **Broaden Filters:** Introduce standard filters like product categories and customer tiers to make it easier for sales teams to slice through the data and isolate underperforming accounts faster.

### 3. Enhance User Practicality
* **Set Up Simple Performance Alerts:** Configure basic data alerts in the Power BI Service to automatically email team leads whenever a specific country's Gross Profit drops below a target threshold.

---

> ⚠️ **Disclaimer:** > *This repository is a portfolio case study engineered for educational and technical validation purposes.* > *FloraGlobal Distribution is a fictional corporation. All underlying transactional entries, customer accounts, and geographic associations are synthetically generated datasets structured to replicate realistic enterprise operations.*
