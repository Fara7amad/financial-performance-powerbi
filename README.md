# financial-performance-powerbi
End-to-end financial analytics dashboard built in Power BI — star schema, DAX measures, time intelligence

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

## 📸 Dashboard Screenshots

<img width="1498" height="866" alt="screenshot_overview" src="https://github.com/user-attachments/assets/6848c418-bf25-4ea5-a404-49f1d3bcf613" />
<img width="1500" height="867" alt="screenshot_departments" src="https://github.com/user-attachments/assets/0ee8333e-8f16-4f48-840a-5e4447bb17f7" />
