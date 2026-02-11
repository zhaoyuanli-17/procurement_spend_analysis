# Global Procurement Spend Harmonization & Analytics

## Project Overview
This project demonstrates an end-to-end procurement analytics workflow, focusing on **integrating, harmonizing, and analyzing large-scale procurement spend data across two ERP systems**.

Rather than a data science exercise, the project is designed as a **business-facing analytics deliverable**, reflecting real enterprise constraints such as inconsistent ERP schemas, incomplete master data, and governance-driven design decisions.

---

## Business Context
Large organizations frequently operate multiple ERP systems due to historical implementations, acquisitions, or system migrations.  
As a result, procurement analytics often face challenges including:

- Inconsistent schemas across ERP systems  
- Missing or unreliable master data  
- Limited comparability for enterprise-wide spend reporting  

This project simulates such an environment and demonstrates how **spend harmonization is a prerequisite for reliable procurement analytics**.

---

## Data Sources
- Original procurement dataset (~600MB) sourced from Kaggle (public procurement records)
- Two simulated ERP systems derived from the same dataset to represent **legacy vs. modern ERP environments**

> Raw data is excluded from version control due to size and best practices.

---

## Technical Workflow

### 1. Data Profiling & Quality Assessment
- Field-level profiling of a large procurement dataset using Python
- Identification of key analytical fields:
  - Vendor
  - Spend amount
  - Fiscal year
  - Category
  - Department
- Early detection of missing, inconsistent, and non-standardized values

---

### 2. ERP Split & Harmonization
- Simulation of two ERP systems with differing schemas
- Alignment and standardization of:
  - Spend fields
  - Vendor identifiers
  - Category and department structures
- Creation of a **harmonized spend dataset** optimized for BI consumption

---

### 3. Spend Analytics & KPIs
The harmonized model supports enterprise procurement KPIs including:
- Total Spend
- Year-over-Year Spend (Fiscal Year)
- Vendor Concentration (Top-N analysis)
- Category Mix
- Active Vendors

---

## Power BI Dashboard

### Page 1 – Executive Spend Overview
Designed for senior stakeholders:
- High-level spend KPIs
- Spend trends by fiscal year
- ERP spend distribution
- Top vendors by spend

---

### Page 2 – Vendor & Category Analysis
Focused on procurement analysis:
- High-impact categories by spend
- Supplier concentration within categories
- Department-level spend visibility

---

### Page 3 – Data Quality & Governance
Explicitly addresses analytical limitations:
- Quantification of spend impacted by missing department data
- Comparison of data completeness across ERP systems
- Clear separation between *missing*, *zero*, and *not applicable* values

> Governance KPIs intentionally return **BLANK** where analysis is not applicable, preventing misinterpretation of missing data as zero values.

---

## Design Decisions & Data Limitations

### Fiscal Year as Primary Time Dimension
Although posting date fields existed in the schema, transaction-level dates were largely unavailable or unreliable due to legacy system constraints.  
To avoid artificial precision, all trend analysis is based on **fiscal year**, the only consistently populated time dimension.

---

### Missing Department Governance
Missing department values were exported as string literals (e.g. `"nan"`) rather than true nulls.  
These values were explicitly standardized in Power Query to ensure accurate governance metrics.

---

### Matrix & Top-N Constraints
Due to Power BI matrix limitations, stable Top-N filtering can only be applied to one dimension at a time.  
Category prioritization is therefore handled at the page level, while supplier concentration is evaluated within selected categories.

---

## Execution Notes & Practical Challenges

Several practical issues emerged during implementation that influenced both modeling and dashboard design:

- Transaction-level dates were present in the schema but fully empty in one ERP system, making monthly trend analysis misleading.
- Missing department values required semantic correction after CSV export.
- Initial governance KPIs returned inflated results (e.g. 100%) due to overlapping filter context, which was resolved by explicitly removing filters from the denominator.
- Power BI matrix visuals required design trade-offs to maintain analytical clarity.

These challenges reflect common enterprise analytics constraints rather than technical errors.

---

## Key Learnings
- Spend harmonization is a prerequisite for enterprise procurement analytics
- ERP schema inconsistencies directly affect downstream reporting
- Governance metrics must distinguish between *missing*, *zero*, and *not applicable*
- Explicitly documenting analytical limitations increases stakeholder trust

---

## Repository Structure
procurement_spend_analysis/
├─ data/
│ ├─ raw/ # excluded via .gitignore
│ ├─ processed/
│ └─ docs/
├─ notebooks/
├─ powerbi/
│ └─ procurement_spend_dashboard.pbix
└─ README.md

---

## Tools & Technologies
- Python (Pandas, NumPy)
- Power BI
- Git & GitHub
- VS Code


