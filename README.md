# Customer Churn Executive Dashboard | Power BI

## 📊 Project Overview

This project is a **Power BI Customer Churn Executive Dashboard** developed as part of a Senior BI Developer practical assessment.

The objective is to analyze customer churn, identify the key customer characteristics associated with higher observed churn, and translate the analysis into actionable business insights for management.

The project demonstrates an end-to-end Business Intelligence workflow:

**Data Preparation → Data Quality Assessment → Data Modeling → DAX → Data Visualization → Business Analysis → Recommendations**

---

## 🎯 Business Problem

Management wants to understand the overall customer churn situation and identify the key factors associated with customer attrition.

The dashboard answers questions such as:

* How many customers does the business have?
* How many customers have churned?
* What is the overall churn rate?
* Which contract types have higher observed churn?
* Which internet-service groups have higher observed churn?
* Which payment methods are associated with higher churn?
* How does churn vary by customer tenure?
* Which customer segments have higher observed churn?
* Where should management focus retention efforts?

---

# 🧩 Selected Use Case

## Option 1 — Customer Churn Executive Dashboard

Three use cases were provided in the assessment. **Option 1 was selected** because it provides the strongest alignment between the available customer-level dataset and the assessment requirements.

The dataset directly supports analysis of:

* Churn
* Contract Type
* Internet Service
* Payment Method
* Tenure
* Monthly Charges
* Total Charges
* Customer characteristics

### Why Option 1?

Option 1 allows the project to demonstrate the complete BI workflow, including:

* Data preparation
* Data-quality assessment
* Power BI data modeling
* DAX calculations
* KPI development
* Interactive dashboard design
* Dimensional churn analysis
* Business insight generation
* Management recommendations

### Why not Option 2?

**Customer Value Segmentation** focuses primarily on identifying valuable customer groups using tenure, monthly charges and total charges.

Although the dataset supports this analysis, defining customer value would require additional assumptions regarding how these variables should be combined or interpreted.

Option 1 provides a broader opportunity to analyze the overall churn problem.

### Why not Option 3?

**Retention Opportunity Analysis** requires additional methodology for concepts such as:

* Revenue at risk
* High business impact
* Priority retention segments

The dataset does not provide a predefined revenue-at-risk or business-impact measure. Developing these would require additional assumptions.

Therefore, Option 1 was selected because it provides a more direct, transparent and defensible analysis using the available data.

---

# 📁 Dataset

The project uses a telecommunications customer churn dataset containing **7,043 customer records and 21 fields**.

Key fields include:

| Field             | Description                |
| ----------------- | -------------------------- |
| `customerID`      | Unique customer identifier |
| `gender`          | Customer gender            |
| `SeniorCitizen`   | Senior citizen indicator   |
| `Partner`         | Partner status             |
| `Dependents`      | Dependent status           |
| `tenure`          | Customer tenure in months  |
| `InternetService` | Internet service type      |
| `Contract`        | Contract type              |
| `PaymentMethod`   | Payment method             |
| `MonthlyCharges`  | Monthly customer charges   |
| `TotalCharges`    | Customer total charges     |
| `Churn`           | Customer churn status      |

---

# 🧹 Data Preparation & Quality Assessment

Data preparation was performed using **Power Query in Power BI**.

### Key transformations

* Removed unnecessary formatting inconsistencies
* Standardized data types
* Converted `tenure` to Whole Number
* Converted `MonthlyCharges` to Decimal Number
* Converted `TotalCharges` to Decimal Number
* Trimmed text fields
* Checked customer ID uniqueness
* Reviewed missing values
* Created analytical fields for tenure and customer segmentation

### Data-quality finding

`TotalCharges` contained **11 blank values**.

These records correspond to customers with zero-month tenure.

The blank values were retained as null rather than replacing them with an artificial average or estimated value.

This treatment avoids introducing fabricated financial information into the analysis.

---

# 🧮 Derived Fields

## Tenure Group

Customers were grouped into four lifecycle categories:

| Tenure Group | Definition   |
| ------------ | ------------ |
| New          | 0–12 months  |
| Developing   | 13–36 months |
| Loyal        | 37–60 months |
| Champions    | >60 months   |

## Customer Segment

The dataset did not contain a predefined Customer Segment field.

Therefore, a segment was derived using `Partner` and `Dependents`:

| Partner | Dependents | Segment        |
| ------- | ---------- | -------------- |
| Yes     | Yes        | Family         |
| Yes     | No         | Partner Only   |
| No      | Yes        | Dependent Only |
| No      | No         | Single         |

This is explicitly documented as an analytical assumption.

---

# 📐 Key DAX Measures

The dashboard uses DAX measures including:

### Total Customers

```DAX
Total Customers =
DISTINCTCOUNT('Customer'[customerID])
```

### Churned Customers

```DAX
Churned Customers =
CALCULATE(
    [Total Customers],
    'Customer'[Churn] = "Yes"
)
```

### Churn Rate

```DAX
Churn Rate =
DIVIDE(
    [Churned Customers],
    [Total Customers],
    0
)
```

### Total Historical Charges

```DAX
Total Revenue =
SUM('Customer'[TotalCharges])
```

### Average Monthly Charges

```DAX
Average Monthly Charges =
AVERAGE('Customer'[MonthlyCharges])
```

Additional measures are used for revenue contribution, churn contribution and segment-level analysis.

---

# 📊 Dashboard Structure

The Power BI report consists of three analytical pages.

---

## Page 1 — Executive Overview

The executive dashboard provides a high-level view of the customer base and churn situation.

### KPIs

* Total Customers
* Churned Customers
* Churn Rate
* Average Monthly Charges
* Average Customer Tenure

### Analysis

* Churn by Contract Type
* Churn by Internet Service
* Churn by Payment Method
* Churn by Tenure Group
* Churn by Customer Segment

Interactive slicers allow management to investigate churn across customer characteristics.

---

# Page 2 — Churn Drivers & Customer Risk Patterns

This page focuses on:

> **Which customer characteristics are associated with higher observed churn?**

### Analysis

* Churn Rate by Contract Type
* Churn Rate by Internet Service
* Churn Rate by Payment Method
* Churn Rate by Tenure Group
* Churn Rate by Customer Segment

An overall churn benchmark is used to help identify categories above and below the total customer churn rate.

### Key observed patterns

* New customers show substantially higher observed churn than long-tenure customers.
* Month-to-month customers show substantially higher observed churn than longer-term contract customers.
* Electronic-check customers have the highest observed churn rate among payment methods.
* Fiber-optic customers show substantially higher observed churn than other internet-service groups.

These findings represent **observed associations in the dataset and should not be interpreted as proof of causation**.

---

# Page 3 — Customer Value & Retention

The third page provides a commercial view of customer segments.

### Matrix

| Segment        | Customers | Churned | Churn Rate |                Revenue |
| -------------- | --------: | ------: | ---------: | ---------------------: |
| Single         |     3,280 |   1,123 |     34.24% | Calculated in Power BI |
| Partner Only   |     1,653 |     420 |     25.41% | Calculated in Power BI |
| Dependent Only |       361 |      77 |     21.33% | Calculated in Power BI |
| Family         |     1,749 |     249 |     14.24% | Calculated in Power BI |

### Visuals

* Customer Count by Segment
* Revenue Contribution by Segment
* Churn Rate by Segment
* Average Monthly Charges by Segment

This page helps management compare customer scale, historical charges and retention performance.

---

# 🔎 Key Findings

## Overall Churn

The dataset contains:

* **7,043 customers**
* **1,869 churned customers**
* **26.54% overall churn rate**
* **64.76 average monthly charges**
* **32.4 months average customer tenure**

---

## Tenure

Customers in the **0–12 month** tenure group show a **47.44% observed churn rate**, compared with **6.61%** among customers with more than 60 months of tenure.

This indicates a strong observed relationship between customer tenure and churn.

---

## Contract

Month-to-month customers show a **42.71% observed churn rate**, compared with:

* 11.27% for one-year contracts
* 2.83% for two-year contracts

Month-to-month customers also represent the majority of churned customers.

---

## Payment Method

Electronic-check customers show a **45.29% observed churn rate**, the highest among the payment-method categories.

This should be investigated further to determine whether payment experience or other correlated customer characteristics contribute to this pattern.

---

## Internet Service

Fiber-optic customers show a **41.89% observed churn rate**, compared with:

* 18.96% for DSL
* 7.40% for customers without internet service

This indicates a significant observed difference that warrants further investigation.

---

# 💡 Business Recommendations

### 1. Strengthen early-life customer retention

Develop targeted onboarding and early customer engagement initiatives for customers during their first 12 months.

Potential actions include:

* Proactive onboarding communication
* Early service-quality checks
* Customer satisfaction monitoring
* Proactive support
* First-year retention campaigns

### 2. Increase focus on month-to-month customers

Identify suitable month-to-month customers and test incentives for migration to longer-term contracts.

The effectiveness of these initiatives should be monitored through subsequent churn rates.

### 3. Investigate payment and service experience

Investigate the high observed churn among electronic-check and fiber-optic customers.

The analysis should determine whether the observed relationship is associated with:

* Billing experience
* Payment friction
* Service quality
* Customer demographics
* Contract structure
* Other underlying customer characteristics

---

# ⚠️ Analytical Assumptions & Limitations

### Customer Segment

Customer Segment was not provided directly in the dataset.

It was derived from:

* Partner
* Dependents

### Revenue

`TotalCharges` is treated as **historical customer charges** for analytical purposes.

It should not automatically be interpreted as accounting revenue or future revenue at risk.

### Causality

The dashboard identifies **observed relationships and patterns**.

It does not establish that contract type, payment method, internet service or tenure directly causes churn.

### Revenue at Risk

A formal revenue-at-risk model was not developed because the selected use case does not require it and the source data does not provide a predefined methodology for calculating future revenue at risk.

---

# 🛠️ Tools & Technologies

* **Microsoft Power BI**
* **Power Query**
* **DAX**
* **Microsoft Excel / CSV**
* **Data Visualization**
* **Business Intelligence**
* **Customer Analytics**

---

# 📂 Project Structure

```text
Customer-Churn-PowerBI/
│
├── README.md
│
├── Data/
│   └── customer_churn.csv
│
├── PowerBI/
│   └── Customer_Churn_Dashboard.pbix
│
├── Report/
│   └── Customer_Churn_Dashboard.pdf
│
├── Documentation/
│   └── Executive_Summary.pdf
│
└── Screenshots/
    ├── Executive_Overview.png
    ├── Churn_Drivers.png
    └── Customer_Segments.png
```

---

# 🎯 Project Objective

The objective of this project is not simply to create attractive visualizations.

The dashboard is designed to demonstrate how a BI professional can:

1. Understand a business problem
2. Assess data quality
3. Prepare and transform data
4. Develop an appropriate analytical model
5. Create meaningful DAX measures
6. Build an interactive Power BI dashboard
7. Identify meaningful customer patterns
8. Translate analysis into business recommendations

---

# 📌 Assessment Alignment

| Assessment Area            | Implementation                                 |
| -------------------------- | ---------------------------------------------- |
| Data Preparation & Quality | Power Query transformations and quality checks |
| Data Model Design          | Customer-level analytical model                |
| DAX Measures               | KPI, churn, revenue and segment measures       |
| Dashboard Design           | Three-page interactive Power BI report         |
| Business Insights          | Churn patterns and customer segment analysis   |
| Recommendations            | Retention and customer-experience actions      |
| Presentation               | Executive-focused dashboard and documentation  |

---

# 👤 Author

**Sehrish Saqlain**

Business Intelligence / Data Analytics

Skills demonstrated:

**Power BI • Power Query • DAX • Data Analysis • Data Visualization • Business Intelligence • Customer Analytics**

---

## Disclaimer

This project is developed for practical assessment and portfolio purposes.

The insights represent patterns observed in the supplied dataset. Recommendations should be validated against additional operational, customer-service and financial data before implementation.
