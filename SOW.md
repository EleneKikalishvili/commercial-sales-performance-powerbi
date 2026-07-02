# Statement of Work (SOW)
**Project Title:** FloraGlobal Commercial Sales Performance Analytics  
**Data Analyst:** Elene Kikalishvili  
**Client/Sponsor:** FloraGlobal Distribution Executive Leadership Team  

---

## 1. Project Purpose

The purpose of this project is to develop an enterprise-grade Power BI Commercial Performance Dashboard for **FloraGlobal Distribution**, a multinational B2B commercial horticulture and wholesale botanical distributor. 

The dashboard transforms raw, multi-source transactional and relational data into a unified, interactive commercial intelligence system. By structuring and optimizing this analytical architecture, executive leadership and logistics managers will be able to identify:

- Real-time commercial sales trajectory across international business accounts.
- Areas of bottom-line profit erosion disguised by top-line revenue volume.
- Growth and performance variances across core product families (`Landscape`, `Outdoor`, and `Indoor`).
- Month-over-month and year-over-year progression of commercial volume and margin efficiency metrics.
- High-volume, low-margin wholesale accounts requiring contract renegotiation or freight-logistics optimization.

---

## 2. Project Scope

### In-Scope Activities
1. **Data Model Architecture Configuration** - Validating and implementing a star-schema relationship model between the central sales fact table (`Fact_Sales`) and dimensions (`Dim_Date`, `Dim_Account`, `Dim_Product`), ensuring clean, single-direction filter propagation.
2. **Dynamic Metric Switcher Framework Implementation** - Engineering a disconnected table switching parameter structure to enable users to dynamically toggle the entire visual layer between three core dimensions: **Sales (USD)**, **Gross Profit (USD)**, and **Quantity** (volumetric output).
3. **Time-Intelligence Analytics** - Building symmetrical DAX measures for Year-to-Date (`YTD_Sales`) and Prior Year-to-Date (`PYTD_Sales`) that properly isolate specific monthly evaluation periods, preventing chronological compounding errors on variance tracking charts.
4. **Dynamic Context-Aware Interface** - Building conditional string expressions to dynamically update chart headers, canvas titles, and visual tooltips based on the active metric switcher selection and targeted fiscal year.
5. **Geographical Performance Variance Analysis** - Constructing a specialized "Bottom 10" visualization framework to isolate international territories experiencing the sharpest year-over-year commercial performance contraction.
6. **Chronological Step-Variance Optimization** - Deploying a corrected monthly waterfall chart to cleanly chain sequential performance fluctuations across the timeline, flowing accurately into the net annual variance total.
7. **Advanced Profitability Quadrant Analysis Integration** - Designing an Account Profitability Segmentation matrix (scatter plot) intersecting YTD commercial value against Gross Profit Margin Percentage (`GP%`), enriched with baseline crosshairs to isolate client accounts experiencing severe logistics-driven margin erosion.

---

## 3. Out of Scope
This project does **not** include:
- Establishing live pipeline connections to real-time streaming transactional databases or ERP web APIs.
- Engineering machine learning predictive models for future botanical crop yield or multi-year sales forecasting.
- Managing tax accounting adjustments, corporate budgeting allocations, or localized currency conversions outside of USD.
- Modifying or entering data directly into the source relational operational data files.

---

## 4. Deliverables

| Deliverable | Description |
|--------------|-------------|
| **Interactive Enterprise Power BI File (`commercial-sales-performance.pbix`)** | The comprehensive local dashboard deployment file containing the configured star schema data model, calculated DAX measure library, and fully formatted report canvas. |
| **Dynamic Value Switcher Infrastructure** | The data structures and selector slicer logic enabling seamless visual pivoting across Sales, Gross Profit, and Volume metrics. |
| **Symmetrical Time-Intelligence DAX Library** | A clean, organized, and documented measures folder containing the logic for `Value_YTD`, `Value_PYTD`, `YTD_vs_PYTD`, `GP%`, and context-driven dynamic labels. |
| **Account Profitability Segmentation Matrix Visual** | An advanced scatter plot equipped with dual-axis distribution mapping and crosshair reference lines for diagnostic customer margin analysis. |
| **Chronological Monthly Variance Waterfall Chart** | A highly polished waterfall component mapped to monthly increments to track chronological net performance variations without running total distortions. |
| **Comprehensive README & Deployment Documentation** | Professional portfolio documentation detailing business background, key calculations, data model taxonomy, and core executive takeaways. |

---

## 5. Tools and Technologies
- **Power BI Desktop** - Data modeling, DAX measure development, user interface design, and visualization construction.  
- **DAX (Data Analysis Expressions)** - Implementation of advanced time intelligence, dynamic metric selection routing, and complex ratio metrics.  
- **Power Query (M Language)** - Extraction, transformation, and loading (ETL) of raw horticultural dimension and fact matrices.  
- **GitHub** - Version control host for project lifecycle tracking, folder structure management, and professional portfolio publication.  

---

## 6. Success Criteria
The project will be considered successful when:
- The data model successfully establishes unidirectional filtering from dimension tables to fact tables without circular dependencies or filter bleeding.
- The dynamic metric switcher correctly updates all summary cards, visual titles, and chart weights simultaneously across Sales, Gross Profit, and Quantity selections.
- The monthly waterfall variance chart reflects the correct isolated periodic monthly changes, cleanly reconciling with global summary KPI cards.
- The account segmentation matrix separates customer entities with geometric precision into actionable operational quadrants.
- The report is fully packaged into a polished, executive-ready asset optimized for technical and professional review.

---

**Version:** 1.0  
**Date:** July 2026  
**Author:** Elene Kikalishvili  
