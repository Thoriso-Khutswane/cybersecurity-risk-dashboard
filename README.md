# Cybersecurity Risk & Vulnerability Management Dashboard

## Overview
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

![bar plot](https://github.com/Thoriso-Khutswane/PrivateImagesForAllRepos/blob/main/HomeTab.png)//DashbordPicturePowerBI


### Data Sources

Simulated Data: In  the real I used real data obtained from real scans and other datasources such as sharepoint lists, but in this project we  used simulated data generated using python "Run: python generate_csv_files.py", generate_csv_files.py being the script. Running this script generated 4 files which are:
- Fact_Vulnerabilities.csv with 1000 (Fact Table)
- Dim_Critical_Servers.csv with 100(Dimention Table)
- Dim_DMZ.csv with 134 (Dimention Table)
- Dim_BYOD_Devices.csv with 500 (Dimention Table)
 
### Tools

- Excel - Data Cleaning
  - [Download here](https://microsoft.com)
- SQL Server  SSMS - Database
- PowerBI - Creating reports,Data Analysis and Visuals


### Data Cleaning/Preparation


In the initial data preparation phase, we performed the following tasks:
1. Data cleaning and formatting.
- Data was effectively cleaned using Microsoft Excel, e.g. **CSV Parsing:** converting a comma-separated file into proper columns. . Making first rows as headers in the Dim_BYOD_Devices table.
  
2.Data loading and inspection
- A database named CybersecurityRiskDashboard was created in ssms
- csv files were loaded into the database
- Inspections were conducted using sql queries

  - Query 1: LEFT JOIN - Find vulnerabilities on unknown/unmanaged IPs
```sql
PRINT '========================================';
PRINT 'QUERY 2: LEFT JOIN - Unmanaged Assets with Vulnerabilities';
PRINT '========================================';

SELECT 
    v.VulnerabilityID,
    v.IPAddress,
    COALESCE(s.ServerName, 'UNMANAGED - NOT IN INVENTORY') AS ServerName,
    COALESCE(s.BusinessUnit, 'UNKNOWN') AS BusinessUnit,
    v.VulnerabilityName,
    v.Severity,
    v.CVSSScore,
    v.DetectionDate
FROM Fact_Vulnerabilities v
LEFT JOIN Dim_Critical_Servers s 
    ON v.IPAddress = s.IPAddress
WHERE s.ServerID IS NULL  -- These are unmanaged IPs
ORDER BY v.Severity DESC, v.CVSSScore DESC;

GO
```

### Exploratory Data Analysis

EDA involved exploring the sales data to answer key questions, such as:

- What is the overall sales trend?
- Which products are top sellers?
- What are the peak sales periods?

### Data Analysis

Include some interesting code/features worked with

- Query 1:
```sql
SELECT * FROM table1
WHERE cond = 2;
```

### Results & Recommendations

The analysis results are summarized as follows:
1. The company's sales have been steadily increasing over the past year, with a noticeable peak during the holiday season.
2. Product Category A is the best-performing category in terms of sales and revenue.
3. Customer segments with high lifetime value (LTV) should be targeted for marketing efforts.



