## Cybersecurity Risk & Vulnerability Management Dashboard

### Overview
This project showcases my previous working experience as an  IT Risk Data Analyst Associate at Momentum Group, focusing on  developing a real-time vulnerability visibility dashboard for risk teams.
#### Power Bi Concepts Applied
- Dax, Calculated Columns
- Table Relationships (Cardinality[one-to-many],Filters)
- Data Modeling (star schema approach)

## Key Focus Areas:
- Data Sourcing
- Data Transformation/Cleaning/ETL
- Data Modeling
- Data Analysis And Visuals
- Dashboard Automation (Scheduled Refreshes & Gateways)

### Business Context
---
Created a centralized dashboard connecting Nessus vulnerability data with company asset inventory (Critical Servers, DMZ zones, BYOD devices). Enabled risk managers to identify, prioritize, and remediate vulnerabilities faster.

![bar plot](https://github.com/Irene-arch/Documenting_Example/assets/56026296/5ebedeb8-65e4-4f09-a2a5-0699119f5ff7)//DashbordPicturePowerBI


### Data Sources

Simulated Data: In  the real I used real data obtained from real scans and other datasources such as sharepoint lists, but in this project we  used simulated data generated using python "Run: python generate_csv_files.py", generate_csv_files.py being the script. Running this script generated 4 files which are:
- Fact_Vulnerabilities.csv with 1000 (Fact Table)
- Dim_Critical_Servers.csv with 100(Dimention Table)
- Dim_DMZ.csv with 134 (Dimention Table)
- Dim_BYOD_Devices.csv with 500 (Dimention Table)
 

Generating csv files for the dashboard: <br/>
<img src="https://github.com/Thoriso-Khutswane/cybersecurity-risk-dashboard/blob/main/GeneratingCSVFilesForCybersecurityRiskDashboard.png" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Creates 4 CSV files in current directory 
### Tools

- Excel - Data Cleaning
  - [Download here](https://microsoft.com)
- SQL Server - Data Analysis
- PowerBI - Creating reports


### Data Cleaning/Preparation

In the initial data preparation phase, we performed the following tasks:
1. Data loading and inspection.
2. Handling missing values.
3. Data cleaning and formatting.

### Exploratory Data Analysis

EDA involved exploring the sales data to answer key questions, such as:

- What is the overall sales trend?
- Which products are top sellers?
- What are the peak sales periods?

### Data Analysis

Include some interesting code/features worked with

```sql
SELECT * FROM table1
WHERE cond = 2;
```

### Results & Recommendations

The analysis results are summarized as follows:
1. The company's sales have been steadily increasing over the past year, with a noticeable peak during the holiday season.
2. Product Category A is the best-performing category in terms of sales and revenue.
3. Customer segments with high lifetime value (LTV) should be targeted for marketing efforts.



