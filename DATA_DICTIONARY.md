# Data Dictionary - Cybersecurity Risk Dashboard

## Overview
This document describes all tables, columns, and their purposes in the Cybersecurity Risk Dashboard database.

---

## Table 1: Dim_Critical_Servers

**Purpose:** Dimension table storing critical server inventory across all business units

| Column | Data Type | Description | Example |
|--------|-----------|-------------|---------|
| ServerID | INT | Primary key, auto-incrementing | 1 |
| IPAddress | VARCHAR(15) | Server IP address (unique, joins to vulnerabilities) | 10.0.1.1 |
| ServerName | VARCHAR(100) | Human-readable server name | PROD-DB-01 |
| BusinessUnit | VARCHAR(50) | Which business unit owns the server | IT, Finance, Marketing, Health |
| Owner | VARCHAR(100) | Name of person responsible for server | John Davis |
| OS | VARCHAR(50) | Operating system | Windows Server 2019, Linux CentOS |
| ServerFunction | VARCHAR(100) | What the server does | Database Server, Web Server, Application Server |
| Criticality | VARCHAR(20) | How critical the server is | Critical, High, Medium |
| CreatedDate | DATETIME | When record was created | 2023-03-01 |
| LastUpdated | DATETIME | When record was last modified | 2024-02-28 |

**Business Unit Distribution:**
- IT: 35 servers
- Finance: 30 servers
- Marketing: 20 servers
- Health: 15 servers
- **Total: 100 servers**

---

## Table 2: Dim_DMZ

**Purpose:** Dimension table storing network zones (DMZ) across all business units

| Column | Data Type | Description | Example |
|--------|-----------|-------------|---------|
| DMZID | INT | Primary key, auto-incrementing | 1 |
| DMZCIDR | VARCHAR(18) | CIDR notation for network range | 10.0.1.0/24 |
| DMZName | VARCHAR(100) | Descriptive name for DMZ zone | External-DMZ, Internal-DMZ |
| BusinessUnit | VARCHAR(50) | Which business unit | IT, Finance, Marketing, Health |
| Location | VARCHAR(100) | Physical/cloud location | Data Center A, Data Center B, Cloud AWS |
| SecurityLevel | VARCHAR(20) | Security classification | High, Medium, Low |
| Owner | VARCHAR(100) | DMZ administrator | IT Security Team |
| CreatedDate | DATETIME | When record was created | 2023-03-01 |
| LastUpdated | DATETIME | When record was last modified | 2024-02-28 |

**Business Unit Distribution:**
- IT: 40 DMZ zones
- Finance: 35 DMZ zones
- Marketing: 30 DMZ zones
- Health: 29 DMZ zones
- **Total: 134 DMZ zones**

---

## Table 3: Dim_BYOD_Devices

**Purpose:** Dimension table storing Bring Your Own Device inventory

| Column | Data Type | Description | Example |
|--------|-----------|-------------|---------|
| DeviceID | INT | Primary key, auto-incrementing | 1 |
| IPAddress | VARCHAR(15) | Device IP address (unique, joins to vulnerabilities) | 10.20.0.1 |
| DeviceType | VARCHAR(50) | Type of device | Laptop, Desktop, Smartphone, Tablet |
| OS | VARCHAR(50) | Device operating system | Windows 10, iOS, macOS, Android |
| UserName | VARCHAR(100) | User who owns/uses device | Alice Thompson |
| UserDepartment | VARCHAR(50) | Department of user | IT, Finance, Marketing, Health |
| RiskLevel | VARCHAR(20) | Risk assessment of device | Critical, High, Medium, Low |
| EnrollmentDate | DATETIME | When device was enrolled in MDM | 2023-06-15 |
| CreatedDate | DATETIME | When record was created | 2023-06-15 |
| LastUpdated | DATETIME | When record was last modified | 2024-02-28 |

**Department Distribution:**
- IT: 150 devices
- Finance: 125 devices
- Marketing: 125 devices
- Health: 100 devices
- **Total: 500 BYOD devices**

---

## Table 4: Fact_Vulnerabilities

**Purpose:** Fact table storing all vulnerability findings from Nessus scans

**Grain:** One row per vulnerability finding per IP address

| Column | Data Type | Description | Example |
|--------|-----------|-------------|---------|
| VulnerabilityID | INT | Primary key, auto-incrementing | 1 |
| IPAddress | VARCHAR(15) | Target IP where vulnerability was found | 10.0.1.1 |
| PluginID | INT | Nessus plugin identifier | 45001 |
| VulnerabilityName | VARCHAR(255) | Name of vulnerability | OpenSSH Weak Cipher Use |
| Description | VARCHAR(MAX) | Detailed description | Server running OpenSSH with weak cipher algorithms |
| Severity | VARCHAR(20) | Risk severity level | Critical, High, Medium, Low |
| CVSSScore | DECIMAL(3,1) | CVSS v3.0 score (0.0 - 10.0) | 7.5 |
| OWASPCategory | VARCHAR(100) | OWASP Top 10 category | A01:2025 - Broken Access Control |
| DetectionDate | DATETIME | When vulnerability was first detected | 2023-03-15 |
| ResolvedDate | DATETIME | When vulnerability was fixed (NULL if open) | 2023-03-17 |
| Status | VARCHAR(20) | Current status | Open, Resolved, False Positive |
| DaysToRemediation | INT | Days from detection to resolution | 2 |
| CreatedDate | DATETIME | When record was created | 2023-03-15 |
| LastUpdated | DATETIME | When record was last modified | 2024-02-28 |

**Severity Distribution:**
- Critical: 300 (30%)
- High: 400 (40%)
- Medium: 220 (22%)
- Low: 80 (8%)
- **Total: 1,000 vulnerabilities**

**Status Distribution:**
- Resolved: 880 (88%)
- Open: 120 (12%)

**Remediation Timeline (of 880 resolved):**
- 73% resolved in 72 hours (642 vulnerabilities)
- 27% resolved in 6 days (238 vulnerabilities)

**Date Range:** 2023-03-01 to 2024-02-28

---

## OWASP Top 10 Categories Included

| Code | Category | Description |
|------|----------|-------------|
| A01 | Broken Access Control | Restrictions on user actions not properly enforced |
| A02 | Security Misconfiguration | Unsecured defaults, incomplete configs, verbose errors |
| A03 | Software & Data Integrity Failures | Insecure CI/CD pipelines, vulnerable dependencies |
| A04 | Cryptographic Failures | Weak encryption, missing encryption, poor algorithms |
| A06 | Using Components with Known Vulnerabilities | Outdated libraries, frameworks with CVEs |
| A08 | Insecure Deserialization | Unsafe deserialization of untrusted data |

---

## Key Relationships
