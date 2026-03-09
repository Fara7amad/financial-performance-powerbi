

# 📊 Financial Performance Dashboard — Power BI Project

![Power BI](https://img.shields.io/badge/Power%20BI-F2C811?style=for-the-badge&logo=powerbi&logoColor=black)
![DAX](https://img.shields.io/badge/DAX-0078D4?style=for-the-badge&logo=microsoft&logoColor=white)
![Status](https://img.shields.io/badge/Status-Complete-brightgreen?style=for-the-badge)
![Level](https://img.shields.io/badge/Level-Beginner%20Friendly-blue?style=for-the-badge)

---

## 🗂️ Project Overview

This is a **professional financial analytics dashboard** built in Microsoft Power BI. It tracks a fictional company's revenue, expenses, profit margins, and budget vs. actual performance across multiple departments and time periods.

The project is designed to be **beginner-friendly** while showcasing industry-standard practices used by real data analysts — including a proper star schema data model, clean DAX measures, and polished visuals.

---

## 🎯 Business Questions Answered

| # | Question |
|---|----------|
| 1 | How is total revenue trending month-over-month? |
| 2 | Which departments are over or under budget? |
| 3 | What is our net profit margin this year vs. last year? |
| 4 | Which expense categories are growing the fastest? |
| 5 | Are we on track to hit our annual revenue target? |
| 6 | What is the cumulative (YTD) profit for the current year? |

---

## 📁 Project Structure

```
financial-performance-powerbi/
│
├── 📄 README.md                    ← You are here
├── 📊 FinancialDashboard.pbix      ← Main Power BI file
│
├── 📂 data/
│   ├── fact_financials.csv         ← Main transactions table
│   ├── dim_date.csv                ← Date dimension table
│   ├── dim_department.csv          ← Department dimension table
│   ├── dim_account.csv             ← Chart of accounts table
│   └── dim_budget.csv              ← Annual budget targets table
│
├── measures_cheatsheet.md      ← All DAX measures with explanations
```

---

## 🧱 Data Model (Star Schema)

```
                    ┌─────────────────┐
                    │   dim_date      │
                    │─────────────────│
                    │ DateKey (PK)    │
                    │ Date            │
                    │ Year            │
                    │ Month           │
                    │ Quarter         │
                    │ MonthName       │
                    └────────┬────────┘
                             │
         ┌───────────────────▼────────────────────┐
         │            fact_financials             │
         │────────────────────────────────────────│
┌────────┤ DateKey (FK)                           ├────────┐
│        │ DepartmentKey (FK)                     │        │
│        │ AccountKey (FK)                        │        │
│        │ ActualAmount                           │        │
│        │ BudgetAmount                           │        │
│        └────────────────────────────────────────┘        │
│                                                          │
▼                                                          ▼
┌─────────────────┐                         ┌──────────────────┐
│ dim_department  │                         │   dim_account    │
│─────────────────│                         │──────────────────│
│ DepartmentKey   │                         │ AccountKey (PK)  │
│ DepartmentName  │                         │ AccountName      │
│ Region          │                         │ Category         │
│ Manager         │                         │ Type (Rev/Exp)   │
└─────────────────┘                         └──────────────────┘
```

**Why Star Schema?**
- ✅ Faster query performance in Power BI
- ✅ Simpler DAX formulas
- ✅ Easier to understand and maintain
- ✅ Industry standard for analytics

---
Data:
5 data files: a realistic 3-year dataset (2022–2024) for a fictional company called Meridian Corp, with 1,258 rows of monthly financial transactions across 7 departments, 15 account types, and budget vs. actual figures.


| File        |  What it containts            | Rows  |
| ------------- |:-------------:| -----:|
| fact_financials.csv     | Every monthly financial transaction, actual vs. budget amounts| ~1,258 |
|dim_date.csv      | Every calendar day from 2022–2024 with year, month, quarter labels      |   1,096 |
| dim_department.csv |7 company departments with manager names and regions      |    7 |
|dim_account.csv|15 account types (revenue & expense categories)|15
|dim_kpi_targets.csv|Annual performance targets for key metrics|9|


Full Tutorial:
https://medium.com/@farahabuhamad/learn-power-bi-complete-beginner-tutorial-5520b7116df5 

---

## 💡 Key Concepts Demonstrated

| Concept | Details |
|---------|---------|
| **Star Schema** | Fact + 4 dimension tables with proper relationships |
| **DAX Measures** | 15+ measures from basic SUM to time intelligence |
| **Time Intelligence** | YTD, Prior Year, MoM % change |
| **KPI Visuals** | Variance to budget with conditional formatting |
| **Drill-Through** | Click department → see account-level detail |
| **Dynamic Titles** | Report titles that change with slicer selections |
| **Slicers** | Year, Quarter, Department, Account Type |

---

## 📚 What You'll Learn

This project teaches you the **core skills every Power BI analyst needs**:

1. **Data Modeling** — How to structure data properly (not just one big table)
2. **DAX Basics** — Writing your first measures vs. calculated columns
3. **Time Intelligence** — YTD, Prior Year comparisons that actually work
4. **Visual Best Practices** — When to use bar vs. line vs. card visuals
5. **Report Design** — Layout, spacing, and color for executive audiences

---

## 🚀 Extend This Project (Next Steps)

Once you're comfortable with the basics, try these enhancements:

- [ ] Add a **Forecast page** using Power BI's built-in analytics pane
- [ ] Create a **What-If parameter** to model different growth scenarios
- [ ] Connect to a **live data source** (Excel, SharePoint, SQL Server)
- [ ] Publish to **Power BI Service** and schedule automatic refresh
- [ ] Add **Row-Level Security (RLS)** so managers only see their department

---
## 📸 Dashboard Screenshots

<img width="1498" height="866" alt="screenshot_overview" src="https://github.com/user-attachments/assets/6848c418-bf25-4ea5-a404-49f1d3bcf613" />
<img width="1500" height="867" alt="screenshot_departments" src="https://github.com/user-attachments/assets/0ee8333e-8f16-4f48-840a-5e4447bb17f7" />

---
## 👤 Author

**Farah Abu Hamad**
📧 farahabuhamad@gmail.com

---

## 📄 License

This project is open source and available under the [MIT License](LICENSE).

---

> 💬 *"The goal is to turn data into information, and information into insight."*
> — Carly Fiorina
