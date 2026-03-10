# 📐 DAX Measures Cheat Sheet
## Financial Performance Power BI Dashboard

> **How to use this file:**
> In Power BI Desktop → Right-click your fact table → "New Measure" → Paste the DAX code below.
> Start with Section 1 (Base Measures) before creating the advanced ones — they depend on each other.

---

## 🗂️ Table of Contents

1. [Base Measures](#1-base-measures)
2. [Budget & Variance](#2-budget--variance)
3. [Profit & Margin](#3-profit--margin)
4. [Time Intelligence — YTD](#4-time-intelligence--ytd)
5. [Time Intelligence — Prior Year](#5-time-intelligence--prior-year)
6. [Month-over-Month](#6-month-over-month)
7. [KPI & Conditional Logic](#7-kpi--conditional-logic)
8. [Dynamic Titles](#8-dynamic-titles)
9. [Quick Reference Card](#9-quick-reference-card)

---

## 1. Base Measures

> **Start here.** These are the foundation. All other measures build on top of these.

---

### Total Revenue

```dax
Total Revenue =
CALCULATE(
    SUM( fact_financials[ActualAmount] ),
    dim_account[Type] = "Revenue"
)
```

**Plain English:** Add up all `ActualAmount` values where the account type is "Revenue".

**💡 Tip:** Always use `CALCULATE` + a filter when you want to sum only a subset of rows. This is much better than filtering in a visual.

---

### Total Expenses

```dax
Total Expenses =
CALCULATE(
    SUM( fact_financials[ActualAmount] ),
    dim_account[Type] = "Expense"
)
```

**Plain English:** Same pattern as Revenue — just filter for "Expense" type accounts.

---

### Total Budget Revenue

```dax
Total Budget Revenue =
CALCULATE(
    SUM( fact_financials[BudgetAmount] ),
    dim_account[Type] = "Revenue"
)
```

**Plain English:** The planned/budgeted revenue amount (same filter, different column).

---

### Total Budget Expenses

```dax
Total Budget Expenses =
CALCULATE(
    SUM( fact_financials[BudgetAmount] ),
    dim_account[Type] = "Expense"
)
```

---

### Transaction Count

```dax
Transaction Count =
COUNTROWS( fact_financials )
```

**Plain English:** Count how many rows exist in the fact table. Useful for checking data completeness.

---

## 2. Budget & Variance

> **Variance = Actual minus Budget.** Positive variance for Revenue is good. Positive variance for Expenses is bad (overspending).

---

### Revenue Variance (Actual vs Budget)

```dax
Revenue Variance =
[Total Revenue] - [Total Budget Revenue]
```

**Plain English:** How much more (or less) revenue did we earn compared to the plan?

---

### Revenue Variance %

```dax
Revenue Variance % =
DIVIDE(
    [Revenue Variance],
    [Total Budget Revenue],
    0           -- Returns 0 if budget is blank or zero (avoids divide-by-zero error)
)
```

**Plain English:** What percentage above or below budget are we?

**💡 Tip:** Always use `DIVIDE()` instead of `/` in DAX. It safely handles division by zero.

---

### Expense Variance

```dax
Expense Variance =
[Total Budget Expenses] - [Total Expenses]
```

**Plain English:** For expenses, we flip the formula — budget minus actual. A positive number means we *saved* money (favorable). A negative number means we *overspent* (unfavorable).

---

### Expense Variance %

```dax
Expense Variance % =
DIVIDE(
    [Expense Variance],
    [Total Budget Expenses],
    0
)
```

---

### Budget Achievement Rate

```dax
Budget Achievement Rate =
DIVIDE(
    [Total Revenue],
    [Total Budget Revenue],
    0
)
```

**Plain English:** What percentage of our revenue target have we achieved? 100% = exactly on target. 110% = 10% above target.

---

## 3. Profit & Margin

---

### Gross Profit

```dax
Gross Profit =
[Total Revenue] - [Total Expenses]
```

**Plain English:** Revenue minus all expenses. This is the "bottom line" — did we make money?

---

### Net Profit Margin

```dax
Net Profit Margin =
DIVIDE(
    [Gross Profit],
    [Total Revenue],
    0
)
```

**Plain English:** For every $1 of revenue, how much is left as profit? A 20% margin means $0.20 profit per $1 earned.

**Format this measure as:** Percentage (%)

---

### Budget Profit

```dax
Budget Profit =
[Total Budget Revenue] - [Total Budget Expenses]
```

---

### Profit Variance

```dax
Profit Variance =
[Gross Profit] - [Budget Profit]
```

---

## 4. Time Intelligence — YTD

> **YTD = Year-To-Date.** Cumulative totals from January 1st up to the current date.
> ⚠️ Time intelligence functions **require a proper Date table** marked as a Date Table in Power BI.

---

### Revenue YTD

```dax
Revenue YTD =
CALCULATE(
    [Total Revenue],
    DATESYTD( dim_date[Date] )
)
```

**Plain English:** Sum all revenue from the start of the year to the current date in context.

**💡 How to set up your Date table:**
1. Click your `dim_date` table in the Model view
2. Go to Table Tools → Mark as Date Table
3. Select the `Date` column

---

### Expenses YTD

```dax
Expenses YTD =
CALCULATE(
    [Total Expenses],
    DATESYTD( dim_date[Date] )
)
```

---

### Profit YTD

```dax
Profit YTD =
CALCULATE(
    [Gross Profit],
    DATESYTD( dim_date[Date] )
)
```

---

### Revenue YTD (Fiscal Year — April start)

```dax
Revenue YTD Fiscal =
CALCULATE(
    [Total Revenue],
    DATESYTD( dim_date[Date], "3/31" )   -- Fiscal year ends March 31
)
```

**Plain English:** If your company's financial year starts in April (common in UK/India), use this version. The second argument is the fiscal year-end date.

---

## 5. Time Intelligence — Prior Year

> Comparing this year to the same period last year is one of the most powerful analytics patterns.

---

### Revenue Prior Year

```dax
Revenue Prior Year =
CALCULATE(
    [Total Revenue],
    SAMEPERIODLASTYEAR( dim_date[Date] )
)
```

**Plain English:** Show the revenue for the same time period, but one year ago.

---

### Expenses Prior Year

```dax
Expenses Prior Year =
CALCULATE(
    [Total Expenses],
    SAMEPERIODLASTYEAR( dim_date[Date] )
)
```

---

### Revenue YoY Change

```dax
Revenue YoY Change =
[Total Revenue] - [Revenue Prior Year]
```

**Plain English:** Absolute dollar change compared to last year.

---

### Revenue YoY % Change

```dax
Revenue YoY % Change =
DIVIDE(
    [Revenue YoY Change],
    [Revenue Prior Year],
    0
)
```

**Format as:** Percentage (%)

---

### Profit YoY % Change

```dax
Profit YoY % Change =
DIVIDE(
    [Gross Profit] - CALCULATE( [Gross Profit], SAMEPERIODLASTYEAR( dim_date[Date] ) ),
    ABS( CALCULATE( [Gross Profit], SAMEPERIODLASTYEAR( dim_date[Date] ) ) ),
    0
)
```

**💡 Tip:** We use `ABS()` on the denominator in case prior year profit was negative — this prevents the sign from flipping unexpectedly.

---

## 6. Month-over-Month

---

### Revenue Previous Month

```dax
Revenue Previous Month =
CALCULATE(
    [Total Revenue],
    DATEADD( dim_date[Date], -1, MONTH )
)
```

**Plain English:** The revenue from exactly one month ago.

---

### Revenue MoM Change

```dax
Revenue MoM Change =
[Total Revenue] - [Revenue Previous Month]
```

---

### Revenue MoM % Change

```dax
Revenue MoM % Change =
DIVIDE(
    [Revenue MoM Change],
    [Revenue Previous Month],
    0
)
```

---

### Rolling 3-Month Average Revenue

```dax
Revenue 3M Rolling Avg =
CALCULATE(
    [Total Revenue],
    DATESINPERIOD(
        dim_date[Date],
        LASTDATE( dim_date[Date] ),   -- End at the last date in current context
        -3,                            -- Go back 3...
        MONTH                          -- ...months
    )
) / 3
```

**Plain English:** Average revenue over the current month and the two months before it. Smooths out spikes and dips.

---

## 7. KPI & Conditional Logic

> Use these to drive conditional formatting and color-coded KPI cards.

---

### Revenue Status (for KPI formatting)

```dax
Revenue Status =
IF(
    [Revenue Variance %] >= 0,
    "On Track",
    "Behind Target"
)
```

---

### Revenue Color (for conditional formatting)

```dax
Revenue Color =
IF(
    [Revenue Variance %] >= 0,
    "#00897B",    -- Teal = good
    "#E53935"     -- Red = bad
)
```

**How to use:** In a KPI card, go to Format → Data colors → fx (conditional formatting) → choose "Field value" → select this measure.

---

### Expense Status

```dax
Expense Status =
IF(
    [Expense Variance %] >= 0,
    "Under Budget ✅",
    "Over Budget ⚠️"
)
```

---

### Profit Margin Status

```dax
Profit Margin Status =
SWITCH(
    TRUE(),
    [Net Profit Margin] >= 0.25,   ">= 25% — Excellent",
    [Net Profit Margin] >= 0.15,   "15–25% — Good",
    [Net Profit Margin] >= 0.05,   "5–15% — Acceptable",
    [Net Profit Margin] >= 0,      "0–5% — Marginal",
    "Negative — Loss"
)
```

**💡 Tip:** `SWITCH(TRUE(), ...)` is DAX's way of doing multiple IF conditions cleanly. Each line is: condition, result.

---

### Departments Over Budget (Count)

```dax
Departments Over Budget =
CALCULATE(
    DISTINCTCOUNT( dim_department[DepartmentName] ),
    FILTER(
        dim_department,
        [Expense Variance %] < 0
    )
)
```

**Plain English:** Count how many departments have negative expense variance (meaning they overspent).

---

## 8. Dynamic Titles

> These measures make your report titles automatically update when a user selects a slicer. Very impressive for portfolio projects!

---

### Selected Year Title

```dax
Selected Year Title =
"Financial Performance — " &
SELECTEDVALUE( dim_date[Year], "All Years" )
```

**How to use:**
1. Add a text box or card visual
2. Format → Title → fx → Field value → this measure

---

### Selected Department Title

```dax
Selected Department Title =
"Department: " &
SELECTEDVALUE( dim_department[DepartmentName], "All Departments" )
```

---

### Dynamic Page Subtitle

```dax
Page Subtitle =
VAR SelectedYear = SELECTEDVALUE( dim_date[Year], "All Years" )
VAR SelectedDept = SELECTEDVALUE( dim_department[DepartmentName], "All Departments" )
RETURN
"Showing data for: " & SelectedYear & " | " & SelectedDept
```

**💡 Tip:** Using `VAR` (variable) makes complex DAX much easier to read and debug. Store a value once, use it multiple times.

---

## 9. Quick Reference Card

### DAX Functions Used in This Project

| Function | What It Does | Example Use |
|----------|-------------|-------------|
| `SUM()` | Adds up a column | `SUM(fact[Amount])` |
| `CALCULATE()` | Evaluates with filters applied | `CALCULATE([Revenue], Year = 2024)` |
| `DIVIDE()` | Safe division (no error on ÷0) | `DIVIDE([Profit], [Revenue], 0)` |
| `IF()` | Returns one of two values based on condition | `IF([Margin] > 0, "Good", "Bad")` |
| `SWITCH(TRUE(), ...)` | Multiple IF conditions cleanly | Profit margin banding |
| `DATESYTD()` | Year-to-date filter | `CALCULATE([Revenue], DATESYTD(Date))` |
| `SAMEPERIODLASTYEAR()` | Same period, 1 year back | Prior year comparison |
| `DATEADD()` | Shift dates by N periods | Previous month |
| `DATESINPERIOD()` | Rolling window of dates | 3-month rolling average |
| `SELECTEDVALUE()` | Gets the single selected value from a slicer | Dynamic titles |
| `DISTINCTCOUNT()` | Count unique values | Departments over budget |
| `VAR` / `RETURN` | Store a value in a variable | Complex measures |
| `ABS()` | Absolute value (removes negative sign) | Safe YoY % on negative profit |
| `FILTER()` | Row-by-row filtering | Departments over budget |
| `COUNTROWS()` | Count rows in a table | Transaction count |

---

### Measures vs. Calculated Columns — When to Use Which

| | Measure | Calculated Column |
|---|---------|------------------|
| **Where it lives** | Doesn't add rows/columns | Adds a new column to your table |
| **When it calculates** | At query time (fast, dynamic) | At data refresh (stored) |
| **Responds to slicers?** | ✅ Yes | ❌ No |
| **Use for** | KPIs, aggregations, % changes | Categories, labels, flags per row |
| **Example** | `Total Revenue` | `Profit Category = IF(Amount > 0, "Profit", "Loss")` |

> **Rule of thumb:** If you want a number that changes when a user clicks a slicer → use a **Measure**. If you want a fixed label/category for each row → use a **Calculated Column**.

---

### Common Mistakes to Avoid

| ❌ Mistake | ✅ Fix |
|-----------|-------|
| Using `/` for division | Use `DIVIDE(a, b, 0)` |
| Forgetting to mark the Date table | Model view → Mark as Date Table |
| Summing in visuals instead of measures | Write a DAX measure, use that |
| Using `FILTER()` inside CALCULATE when a simple filter works | `CALCULATE([Rev], Type = "Revenue")` is faster |
| Writing deeply nested IFs | Use `SWITCH(TRUE(), ...)` instead |
| Putting logic in a Calculated Column that should be a Measure | Ask: "Does this need to respond to slicers?" |

---

*for the Financial Performance Power BI Project*
*See README.md for full project setup instructions*
