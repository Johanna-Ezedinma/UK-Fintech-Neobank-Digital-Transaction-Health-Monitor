# Neobank Digital Transaction Health Monitor
## How a compliance badge became a backdoor

**Author:** Johanna Ezedinma    
**Date:** June 2026

[![LinkedIn](https://img.shields.io/badge/LinkedIn-0077B5?style=for-the-badge&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/johanna-ezedinma/)[![Medium](https://img.shields.io/badge/Medium-12100E?style=for-the-badge&logo=medium&logoColor=white)](https://medium.com/@johannaezedinma)
[![GitHub](https://img.shields.io/badge/GitHub-100000?style=for-the-badge&logo=github&logoColor=white)](https://github.com/Johanna-Ezedinma)

---

A UK digital neobank processed 1,500 transactions across H1 2026. Only 225 completed. 
£1.19M of the £1.90M total transaction value is fraud-exposed. And every single flagged 
fraud transaction in the dataset belongs to KYC-verified customers. Unverified customers 
show a 0% fraud rate.

The compliance framework built to protect the bank is, by the numbers, doing the opposite.

---

## The Business Problem

The bank's Risk and Product team had transactional data covering six months of activity 
across four payment channels, 18 merchant categories, and 20 customers spanning four 
segments. They had no visibility into where fraud was actually concentrating, why so few 
transactions were completing, or which parts of the operation were generating revenue and 
which were leaking it.

The stakeholders needed answers to three questions:

- Where is the transaction operation breaking down?
- Where is fraud actually concentrating, and is the risk framework pointing at the right targets?
- Which customer segments and channels are generating value, and what needs to change before H2?

This dashboard was built to answer all three, with evidence drawn from 1,500 transactions 
across H1 2026.

---

## The Approach

Every page was structured around what a Risk or Finance stakeholder would need to know, in the order 
they would need to know it. The guiding question throughout was: if I had to walk into a 
board meeting with this data, what would I say first, second, and third?

That produced a single connected story: the transaction operation is stalled at the network 
level, flows into where the risk and failures concentrate, which flows into what the revenue 
picture looks like and what to do about it.

---

## Project Overview

| Metric | Detail |
|---|---|
| Transactions | 1,500 |
| Customers | 20 |
| Customer segments | 4 (Starter, Standard, Premium, Business) |
| Payment channels | 4 (Mobile App, Web Browser, Automated, ATM Network) |
| Merchant categories | 18 |
| Transaction types | 15 |
| Period | January to May 2026 (H1) |
| Currency | GBP (£) |
| Fact tables | 1 |
| Dimension tables | 4 + 1 date table |

---

## Report Structure

The dashboard is organised into three pages that tell one connected story. Each page 
raises the question the next page answers.

---

### Page 1: Overview
**Question answered:** How healthy is the bank's transaction operation, and what are the headline numbers?



![Overview](Dashboards/Neobank%20Digital%20Transaction%20Health%20Monitor_Overview.jpg)


**Key insight:** Volume is there. Outcomes are not. The bank is generating activity without 
generating results. Page 2 explains where the failure concentrates.

---

### Page 2: Risk
**Question answered:** Where is fraud concentrating, and is the risk framework pointing at the right targets?



![Risk](Dashboards/Neobank%20Digital%20Transaction%20Health%20Monitor_Risk.jpg)


**Key insight:** The risk framework is misfiring. The categories it treats as dangerous are 
not the ones driving actual fraud. Page 3 shows what the revenue picture looks like and 
what needs to change.

---

### Page 3: Recommendations
**Question answered:** Where is revenue coming from, where is it leaking, and what should the bank do before H2?



![Recommendations](Dashboards/Neobank%20Digital%20Transaction%20Health%20Monitor_Recommendations.jpg)


**Key insight:** The bank is over-reliant on one segment that is both its biggest revenue 
driver and its biggest risk. 50% of H1 fee revenue is tied to fraud-flagged transactions. 
If nothing changes before H2, the bank risks losing both.

---

## Key Findings

- 1,500 transactions processed. Only 225 completed (15% success rate), against an industry standard of 80%
- £1.19M of £1.90M total value is fraud-exposed (63% exposure rate)
- Business and Standard segments have never completed a single transaction across all five months
- Fuel and Online Retail carry higher fraud rates (31%) than the bank's own "High Risk" merchant flags (Gambling 30%, Crypto Exchange 10%)
- KYC-verified customers account for 100% of flagged fraud. Unverified customers show zero
- Business customers on the Automated channel carry a 50% fraud rate, the highest segment-channel combination in the dataset
- 50% of H1 fee revenue originates from fraud-flagged transactions
- Fee revenue peaked at £163 in March and declined to £149 by May despite transaction volume remaining stable
- Card Refunds processed £156K in value and generated £0 in fee revenue

---

## Data Model

**Star schema:** one fact table connected to four dimension tables and a date table.

| Table | Description |
|---|---|
| `fact_transactions_updated` | 1,500 rows. Core transaction log including amount, fee, status, fraud flag, device, and failed reason |
| `dim_customer` | 20 customers across 4 segments, 4 UK regions, KYC status, account open date |
| `dim_merchant_category` | 18 categories across 4 sectors with Low/Medium/High risk flags |
| `dim_transaction_type` | 15 types across 4 channels, domestic and international split, typical fee |
| `dim_date` | Complete calendar Jan 1 to May 31 2026, marked as date table |

**Calculated columns added in Power Query (not DAX):** transaction month, month number, 
transaction hour, time of day, is completed, is failed, is international, fee revenue band, 
amount band, failed reason clean, device type clean (fact table); KYC status label, account 
tenure years, tenure band (dim_customer); risk score (dim_merchant_category); transaction 
direction (dim_transaction_type).

---

## What I Learned

**Counterintuitive findings are worth following.** The KYC finding (verified customers 
driving all fraud, unverified customers showing zero) looked wrong the first time the 
numbers appeared. Checking it three different ways confirmed it was right. The instinct 
to smooth over something that challenges the expected narrative is the exact moment 
to dig in instead.

**A zero is a finding.** Business and Standard segments completing zero transactions 
initially looked like a data or DAX error. Three separate verification methods confirmed 
it was real. Two full customer segments generating no completed transactions is a 
structural failure, not a rounding issue.

**Visual design serves the story.** Replacing the donut chart with a horizontal bar chart 
and adding an industry benchmark line to the success rate trend were both suggested in 
post-build feedback. Both changes made the same data land harder. A flat line at 80% 
sitting above a 15% actual rate communicates urgency that a subtitle alone cannot.

**The risk label and the actual risk are not the same thing.** Gambling and Crypto Exchange 
are flagged High Risk in the merchant table. The data shows Fuel stations and Online 
Retail running higher actual fraud rates. A framework built on assumptions rather than 
evidence is not a risk control. It is a false sense of security.

---

## Tools Used

| Tool | Purpose |
|---|---|
| Power BI Desktop | Report building, visualisation, data modelling |
| DAX | KPI measures, MoM time intelligence, conditional formatting, color measures |
| Power Query (M) | Calculated columns, custom date table, data type corrections |

---
```
## Repository Contents
📁 Neobank-Digital-Transaction-Health-Monitor
│
├── 📄 README.md
└── 📁 Dashboards/
├── Neobank digital transaction health monitor_overview.jpg
├── Neobank digital transaction health monitor_risk.jpg
└── Neobank digital transaction health monitor_recommendations.jpg
```
---

## Links

**Challenge:** [Onyx Data DataDNA June 2026](https://datadna.onyxdata.co.uk)

---


## Author

**Johanna Ezedinma**

[![LinkedIn](https://img.shields.io/badge/LinkedIn-0077B5?style=for-the-badge&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/johanna-ezedinma/)[![Medium](https://img.shields.io/badge/Medium-12100E?style=for-the-badge&logo=medium&logoColor=white)](https://medium.com/@johannaezedinma)
[![GitHub](https://img.shields.io/badge/GitHub-100000?style=for-the-badge&logo=github&logoColor=white)](https://github.com/Johanna-Ezedinma)  
