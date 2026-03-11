# Cybersecurity Risk & Vulnerability Management Dashboard

## Overview
This project showcases my previous working experience as an  IT Risk Data Analyst Associate at Momentum Group, focusing on  developing a real-time vulnerability visibility dashboard for risk teams.
#### Power Bi Concepts Applied
- Dax, Measures
- Table Relationships (Cardinality[many-to-one],Filters direction)
- Data Modeling (star schema approach)

## Key Focus Areas:
- Data Sourcing
- Data Transformation/Cleaning/ETL
- Data Integration(Integrated data from Microsoft SQL Server into Microsoft Power BI)
- Data Modeling
- Data Analysis And Visuals
- Dashboard Automation (Power BI Services, Scheduled Refreshes & Gateways)

### Business Context
---
Created a centralized dashboard connecting Nessus vulnerability data with company asset inventory (Critical Servers, DMZ zones, BYOD devices). Enabled risk managers to identify, prioritize, and remediate vulnerabilities faster.


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
   - Data was effectively cleaned using Microsoft Excel, e.g. **CSV Parsing:** converting a comma-separated file into proper columns.
   - Making first rows as headers in the Dim_BYOD_Devices table.
  
2. Database creation, data loading and inspection
   - A database named **CybersecurityRiskDashboard** was created in ssms, and csv files were loaded into the database
  
  - Query 1: Database creation.
 ```sql
CREATE DATABASE CybersecurityRiskDashboard;
```
- Inspections were conducted using sql queries

  - Query 2: LEFT JOIN - Find vulnerabilities on unknown/unmanaged IPs
```sql
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

```

### Data Integration & Data Modelling
- Connected Power BI to SQL Server to extract vulnerability and asset inventory data for analysis
- I followed a star-schema approach, separating fact table from dimension tables, The Fact_Vulnerabilities table is the Fact table and other tables are dimension tables.

![bar plot](https://github.com/Thoriso-Khutswane/PrivateImagesForAllRepos/blob/main/DataModel2.png)

- I also paid a closer attension to relationships,cardinality and filter directions to avoid confusion

![bar plot](https://github.com/Thoriso-Khutswane/PrivateImagesForAllRepos/blob/main/Relationships%26Cardinality.png)

  

### Data Analysis and Visuals
![bar plot](https://github.com/Thoriso-Khutswane/PrivateImagesForAllRepos/blob/main/ExecutiveOverview.png)

In the dashboard above it is clear that , I created measures to calculate the number of critical vulnerabilities, number of remediated vulnerabilities, remediation rate,  mean time to remediate, created a new table(Assets) and an an extra column in all dimension table called (Asset Type).
   
 - Number of critical vulnerabilities: 
```dax
Critical Vulnerabilities = 
CALCULATE(
COUNT(Fact_Vulnerabilities[VulnerabilityName]),
Fact_Vulnerabilities[Severity] = "Critical"
)
```
![bar plot](https://github.com/Thoriso-Khutswane/PrivateImagesForAllRepos/blob/main/Total%26CriticalVulns.png)

In the dashboard it is observed that the total number of critical vulnerabilities is equal to 1000, and the total number of critical vulnerabilities is equals to 300. 

- Number of remediated vulnerabilities:
    
```dax
Remediated =
CALCULATE(
COUNT(Fact_Vulnerabilities[VulnerabilityName]),
Fact_Vulnerabilities[Status] = "Remediated"
)
```
![bar plot](https://github.com/Thoriso-Khutswane/PrivateImagesForAllRepos/blob/main/Remediated.png)

In the dashboard it is observed that the total number of remediated vulnerabilities is 884, leaving 116 systems still exposed.

**Note**: You can check weather a vulnerability has been remediated or not by comparing your current nessus scans data with your previous scans data. If a vulnerability does not appear in the current data, it has been resolved/remediated. All of this can be accomplished by using ssms.
- Remediation rate:
  
```dax
Remediation Rate % =
DIVIDE([Remediated],
COUNT(Fact_Vulnerabilities[VulnerabilityName])
)
```
![bar plot](https://github.com/Thoriso-Khutswane/PrivateImagesForAllRepos/blob/main/RemediationRate.png)

In the dashboard it is observed that the remiation rate of the Nessus vulnerabilities is 88.4%.
- Mean Time to Remediate (MTTR):

  Shows average time taken to fix vulnerabilities.
```dax
MTTR =
AVERAGE(Fact_Vulnerabilities[RemediationDays])
```
![bar plot](https://github.com/Thoriso-Khutswane/PrivateImagesForAllRepos/blob/main/MeanTimeToRemediate.png)

It took cyber security professionals average of 3 days to remediate 884 vulnerablities


 - Assets Table:
   -Created a column in all dimensional tables called Asset Type, used to create the asset table
```dax
Asset Type = "e.g BYOD Device, DMZ System"
```


```dax
Assets = 
UNION(
SELECTCOLUMNS(Dim_Critical_Servers,"Asset Type","Critical Server","IP",Dim_Critical_Servers[IPAddress]),
SELECTCOLUMNS(Dim_DMZ,"Asset Type","DMZ System","IP",Dim_DMZ[IPAddress]),
SELECTCOLUMNS(Dim_BYOD_Devices,"Asset Type","BYOD Device","IP",Dim_BYOD_Devices[IPAddress])
)
```
![bar plot](https://github.com/Thoriso-Khutswane/PrivateImagesForAllRepos/blob/main/VulnsByAssetType.png)

In the dashboard it is observed that all IP Addresses in the asset inventory have been scanned.  


- Vulnerabilities By Severity:

![bar plot](https://github.com/Thoriso-Khutswane/PrivateImagesForAllRepos/blob/main/VulnerabilityBySeverity.png)
 In the dashboard it clear that vulnerabilities are categorized into low, medium, high, and critical severity. It is observed that there are 80 low vulns, 220 medium, 300 high, and 400 critical.

- Filter by Business Unit:

![bar plot](https://github.com/Thoriso-Khutswane/PrivateImagesForAllRepos/blob/main/FilterBuBusinessUnit.png)


You can filter based on the business units which are finance, health, marketing and IT.

N.B Keep in mind that this model can always be enhanced based on user feedback, meaning it can be designed in way that suits the stakeholders and non-techinical individuals.

 
### Dashboard Automation (Power BI Services, Scheduled Refreshes & Gateways)

- Creating a workspace:

To create a workspace I follwed the folliwng steps:


![bar plot](https://github.com/Thoriso-Khutswane/PrivateImagesForAllRepos/blob/main/WorkSpaceCreationSteps.png)





### Results & Recommendations

The analysis results are summarized as follows:
1. The company's sales have been steadily increasing over the past year, with a noticeable peak during the holiday season.
2. Product Category A is the best-performing category in terms of sales and revenue.
3. Customer segments with high lifetime value (LTV) should be targeted for marketing efforts.



