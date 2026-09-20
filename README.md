<div align="center">

# 🏦 Berka Bank — Power BI Analysis Dashboard

### End-to-end Business Intelligence dashboard analyzing 6 years of Czech banking data (1993–1998)

![Power BI](https://img.shields.io/badge/Power%20BI-F2C811?style=for-the-badge&logo=powerbi&logoColor=black)
![Power Query](https://img.shields.io/badge/Power%20Query-217346?style=for-the-badge&logo=powerbi&logoColor=white)
![DAX](https://img.shields.io/badge/DAX-217346?style=for-the-badge&logo=powerbi&logoColor=white)
![Excel](https://img.shields.io/badge/Excel-217346?style=for-the-badge&logo=microsoft-excel&logoColor=white)

</div>

---

## 📖 Overview

This project analyzes the **Berka Bank dataset** (a well-known real-world Czech banking dataset, sourced from **Kaggle**), covering clients, accounts, loans, cards, transactions, and orders across the years **1993–1998**.

The goal was to build a full, multi-page **Power BI dashboard** that turns raw relational banking data into clear, decision-ready KPIs — helping stakeholders understand client behavior, loan performance, transaction volume, and regional trends across the bank's operations.

- **Dataset source:** Berka Bank Dataset — Kaggle
- **Tools used:** Power BI Desktop, Power Query (M), DAX, Microsoft Excel

---

## 🧱 Data Model

The dashboard is built on a **star-schema data model** with multiple fact and dimension tables, connected through relationships in Power BI:

- **Fact tables:** `loan fact`, `order fact`, `trans fact`
- **Dimension tables:** `client dim`, `account dim`, `district dim`, `disp dim`, `card dim`, `Date Dim`
- Data was cleaned and transformed using **Power Query** (handling missing values, renaming/typing columns, building relationships) before modeling and visualization.

---

## 📊 Dashboard Pages & KPIs

The report contains **7 pages**, each focused on a specific business area:

### 1️⃣ Overview
A summary landing page combining the top KPIs from every section of the bank's operations.

![Overview](Images/01-overview%20.jpg)

- **Total Clients:** 5.4K
- **Total Accounts:** 4.5K
- **Total Active Loans (Status C + D):** 448
- **Total Transaction Count:** 1M
- **Total Loan Amount:** 103M
- **Total Transaction Amount:** 6bn
- **Total Orders:** 6K
- **Total Cards:** 892
- **Visuals:** Top 10 cities by loan amount, total accounts by region, cards by type, transaction trend by year, loans by status
- **Filters:** Region, Date, Card Type, Status, Type trans, City, Gender

### 2️⃣ Transaction
Deep dive into transaction volume, type, and regional distribution.

![Transaction](Images/02-transaction%20.jpg)

- **Total Transaction Count:** 1M
- **Total Transaction Amount:** 6bn
- **Average Transaction Amount:** 6K
- **Unique Accounts:** 5K
- **Visuals:** Total transaction amount by year, total transactions count by type, total transaction count by year, transaction by operation, transaction amount by type, total transaction by region and city
- **Filters:** Region, Date, Type trans, Operation, Bank, Purpose

### 3️⃣ Clients
Client demographics and growth over time.

![Clients](Images/03-clients%20.jpg)

- **Total Clients:** 5.4K
- **Average Client Age:** 45
- **Total Female Clients:** 2.6K
- **Total Male Clients:** 2.7K
- **Visuals:** Total clients by gender (51% M / 49% F), number of clients by city, clients by age group, total clients by birth year, total clients by year
- **Filters:** Region, Date, Gender

### 4️⃣ Loan
Loan performance, risk status, and geographic breakdown.

![Loan](Images/04-loan%20.jpg)

- **Total Loans:** 682
- **Total Loan Amount:** 103M
- **Average Loan Amount:** 151.41K
- **Active Loan Count (Status C + D):** 448
- **Visuals:** Loans amount by status (67% C, 18% A, 11% D, 4% B), top 10 city by loans count, total loans by status, total loans count by year, top 10 city by loans amount, total loans amount by year
- **Filters:** Status, Date, City

### 5️⃣ Accounts
Account distribution and activity frequency.

![Accounts](Images/05-accounts%20.jpg)

- **Total Accounts:** 4.5K
- **Visuals:** Top 10 city by accounts, accounts by frequency, active accounts by year
- **Filters:** Frequency, Date, City

### 6️⃣ Card
Card issuance and type distribution.

![Card](Images/06-card%20.jpg)

- **Total Cards:** 892
- **Card Types:** 3
- **Visuals:** Top 10 city by cards, total cards by type (74% Classic, 16% Junior, 10% Gold), total cards by issued year
- **Filters:** Type Cards, Date, City

### 7️⃣ Order
Order volume, value, and regional performance.

![Order](Images/07-order%20.jpg)

- **Total Orders:** 6K
- **Average Order Amount:** 3.28K
- **Active Accounts Order:** 4K
- **Total Order Amount:** 21.23M
- **Visuals:** Top 10 city by orders, orders by region, total orders by year, total orders by type
- **Filters:** Bank, Date, City, Region, Order Type

---

## 🔑 Key Insights

- **Prague (Hl.m. Praha)** leads in raw totals — clients (12.5%), accounts (12.3%) and loan value (12.5%) — but this closely tracks its 11.7% share of the national population. Deeper analysis (see `Business_Questions_Insights.md`) shows Prague's exposure is **proportionate, not disproportionately concentrated**; north and south Moravia are the true regional hub by account and order volume.
- Total transaction volume grew steadily year over year, from 0.20bn in 1993 to 1.89bn in 1998, though this growth tracks the expanding account base rather than deeper per-account engagement.
- By value, **67% of the loan book is status C** (running, no issues) and **18% is status A** (finished OK), indicating a broadly healthy portfolio. Problem loans (B + D combined) make up 15% of loan value despite being only 11% of loan count — risk is concentrated in fewer, larger loans.
- Card adoption is dominated by the **Classic** card type (74%), followed by **Junior** (16%) and **Gold** (10%).
- Client base is nearly evenly split by gender (51% male / 49% female), with an average client age of 45.

---

## 🛠️ Skills & Tools Demonstrated

- **Power BI Desktop:** Star-schema data modeling, interactive filtering, multi-page report architecture
- **Power Query (M):** ETL processes, data cleaning, translation, and custom transformation
- **DAX:** Custom measures for KPIs, totals, averages, and dynamic analysis
- **Data Storytelling:** Structured executive dashboard layout focused on business decision-making, including verifying insights against raw figures before presenting them (e.g. distinguishing proportionate concentration from true regional dominance)

---

## 📁 Project Structure

```text
BerkaBankProject/
│
├── Documentation/
│   └── Business_Questions_Insights.md   # Detailed business insights, Q&A, and recommendations
│
├── Images/                              # Dashboard preview screenshots (7 pages)
│   ├── 01-overview .jpg
│   ├── 02-transaction .jpg
│   ├── 03-clients .jpg
│   ├── 04-loan .jpg
│   ├── 05-accounts .jpg
│   ├── 06-card .jpg
│   └── 07-order .jpg
│
├── Birka Analysis111.pbix               # Main Power BI dashboard file
└── README.md                            # Project documentation & overview
```
