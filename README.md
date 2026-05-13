# 🛒 Supermarket Pulse: A Data-Driven Operations Analysis of Q1 2019

> *Cleaning the noise. Finding the signal. Telling the story behind every transaction.*

---

## 📌 Project Overview

This capstone project simulates the role of a **Lead Data Analyst** for a global supermarket chain operating across three Egyptian branches — **Alexandria, Cairo, and Giza**. Given a raw, messy extract of **~1,000 transactions** from Q1 2019, the objective was to transform noisy data into actionable business intelligence — within a single analytical workflow.

The project spans five analytical stages: data cleaning, descriptive statistics, visual dashboarding, probability modeling, and hypothesis formulation. Every figure computed has a business question behind it; every chart answers something management actually needs to know.

---

## 🗂️ Dataset

| Attribute | Detail |
|---|---|
| **Period** | Q1 2019 (January – March) |
| **Branches** | Alexandria (A), Cairo (B), Giza (C) |
| **Total Rows** | ~974 (after cleaning) |
| **Tool Used** | Microsoft Excel (Power Query + Formulas + Charts) |

**Key columns:** Invoice ID, Branch, City, Customer Type, Gender, Product Line, Unit Price, Quantity, Tax, Total, Date, Time, Payment Method, COGS, Gross Margin, Gross Income, Customer Rating.

---

## 🔧 Part 1 — Data Cleaning (Power Query)

The raw dataset arrived with several structural issues that would corrupt any downstream analysis if left uncorrected:

- **Split columns:** `City_CustType` was a merged column containing two distinct attributes. Separated into `City` and `Customer Type` using the correct delimiter.
- **Text standardization:** The `Product Line` column had inconsistent capitalization. Applied *Capitalize Each Word* transformation to normalize all entries.
- **Null handling:** Transactions with blank `Rating` values were filtered out — these could not contribute to satisfaction analysis.
- **Data types:** `Unit Price` and `Total` were cast to Currency; `Quantity` was cast to Whole Number.

The cleaned result was loaded into a dedicated `Clean_Data` sheet — the single source of truth for all subsequent analysis.

---

## 📊 Part 2 — Baseline Metrics & Statistical Summary

With clean data in hand, the focus shifted to establishing the financial and behavioral baseline.

| Metric | Value |
|---|---|
| **Mean Total Bill** | $322.26 |
| **Median Total Bill** | $253.39 |
| **Standard Deviation (Rating)** | ~1.72 |
| **Margin of Error (95% CI, n=60)** | Calculated via Z = 1.96 |

### 🔍 Observation 1 — Skewness in Sales Revenue

![Observation 1 — Skewness Analysis](images/observation_1_skewness.png)

The mean ($322.26) sits noticeably above the median ($253.39) — a gap of $68.87. This is the classic fingerprint of a **right-skewed distribution**: a small number of exceptionally large purchases pulling the mean upward while most transactions cluster at lower values. With a standard deviation of $245.65, transaction values are highly dispersed.

**Implication:** The median is a more reliable measure of a *typical* transaction. Management should not anchor operational expectations to the mean.

---

## 📈 Part 3 — Visual Dashboard

Four visualizations were built to give management an at-a-glance picture of operations.

### Scatter Plot — Total Bill vs. Quantity

![Scatter Plot: Total Bill vs Quantity](images/scatter_total_bill_vs_quantity.png)

A positive linear relationship exists between quantity purchased and total bill (R² = 0.4957). Roughly **49.57%** of the variation in total bill can be explained by quantity alone — a moderate but meaningful correlation. The relationship is real, not random.

---

### Bar Chart — Revenue by Branch Location

![Bar Chart: Revenue by Location](images/bar_revenue_by_location.png)

All three branches performed within a tight revenue band in Q1 2019:

| Branch | Total Revenue |
|---|---|
| Alexandria | $103,013 |
| Cairo | $102,875 |
| **Giza** | **$107,993** |

Giza leads by a slim but consistent margin.

---

### Pie Chart — Payment Method Breakdown

![Pie Chart: Payment Type](images/pie_payment_type.png)

Customer payment preferences were nearly evenly split:

- **Ewallet:** 35%
- **Cash:** 34%
- **Credit Card:** 31%

The near-parity of all three channels signals a diverse, digitally-engaged customer base — and an opportunity for targeted payment promotions.

---

### Histogram — Distribution of Customer Ratings

![Histogram: Customer Rating Distribution](images/histogram_customer_rating.png)

Customer ratings (scale: 4–10) showed a relatively **uniform distribution** across the range, with a slight spike at the [4, 4.5] bin (99 occurrences) and a dip at the upper end ([9.5, 10] = 68). There is no strong concentration of very high ratings — indicating consistent but unremarkable satisfaction across the customer base.

---

### 🔍 Observation 2 — Ewallet Promotion Strategy

![Observation 2 — Ewallet Recommendation](images/observation_2_ewallet.png)

**Giza should be the primary target for any Ewallet promotion.** It generated the highest total revenue ($107,993), the widest Ewallet transaction spread (SD gap of $97.27), and an estimated Ewallet revenue of ~$37,797 (35% × $107,993) — the highest across all branches. A focused promotion here reinforces existing high-value Ewallet behavior and delivers the greatest revenue impact per dollar of marketing spend.

---

## 🎲 Part 4 — Probability Modeling

### Normal Distribution — Sales Revenue

![Probability Distributions](images/probability_distributions.png)

Using Mean = $322.26 and SD = $245.65:

| Scenario | Probability |
|---|---|
| Customer spends **≤ $500** | **76.53%** |
| Customer spends **≥ $800** | **2.59%** |

**Observation 3:** Over three-quarters of customers spend under $500, and high-ticket purchases above $800 are statistically rare (≈1 in 39 customers). The data strongly favors a **volume-over-premium strategy** — stock for smaller, frequent baskets rather than high-ticket inventory that sits on shelves.

---

### Binomial Distribution — Credit Card Usage (p = 0.31)

| Scenario | Probability |
|---|---|
| Exactly 20 of 50 customers pay by credit card | **4.63%** |
| Exactly 0 of 10 customers pay by credit card | **2.45%** |

Both outcomes represent statistical edge cases — reinforcing that credit card usage is consistent but not dominant. Planning staffing or POS terminals exclusively around credit card traffic would be a miscalculation.

---

### Poisson, Exponential & Uniform Distributions

Additional probability models were applied to operational scenarios:

- **Poisson (Foot Traffic):** Modeled daily transaction probability to guide shift scheduling. Extreme outlier volumes (e.g., 120 transactions/day) carry very low probability and should not drive baseline staffing decisions.
- **Exponential (Equipment Failure):** With printer jams averaging every 45 minutes, the probability of a jam within 20 minutes is non-trivial (~36%). Preventive maintenance windows should account for this.
- **Uniform (Voucher Draw):** Each of 300 receipts carries an equal 1/300 probability of winning — no receipt number has any advantage.

---

## 🧪 Part 5 — Hypothesis Formulation

### Test 1 — Member vs. Normal Customer Spend

- **H₀:** The average total spend of Member customers = the average total spend of Normal customers.
- **H₁:** The average total spend of Member customers > the average total spend of Normal customers.
- **Type:** One-tailed (directional) — the marketing team suspects Members spend *more*, not merely *differently*.

### Test 2 — Customer Rating by Gender

- **H₀:** The average customer rating for Male shoppers = the average customer rating for Female shoppers.
- **H₁:** There is a difference in average rating between Male and Female shoppers.
- **Type:** Two-tailed (non-directional) — no prior assumption is made about the direction of the difference.

---

## 🛠️ Tools & Techniques

| Category | Tool / Method |
|---|---|
| Data Cleaning | Excel Power Query |
| Descriptive Statistics | Excel (AVERAGE, MEDIAN, MODE, STDEV.S, VAR.S) |
| Visualizations | Excel Charts (Scatter, Bar, Pie, Histogram) |
| Probability Modeling | NORM.DIST, BINOM.DIST, POISSON.DIST, EXPON.DIST |
| Hypothesis Framing | One-tailed and two-tailed T-test structure |

---

## 📁 Repository Structure

```
supermarket-pulse-q1-2019/
│
├── data/
│   └── Caleb_Chisom_Chike_SuperMarket_Sales_Q1_2019.xlsx
│
├── images/
│   ├── scatter_total_bill_vs_quantity.png
│   ├── bar_revenue_by_location.png
│   ├── pie_payment_type.png
│   ├── histogram_customer_rating.png
│   ├── probability_distributions.png
│   ├── observation_1_skewness.png
│   └── observation_2_ewallet.png
│
├── report/
│   └── The_Capstone_Supermarket_Sales_Operations_Analysis.pdf
│
└── README.md
```

---

## 👤 Author

**Caleb Chisom Chike**
Structural Engineer | Data Analytics Practitioner
📍 Port Harcourt, Nigeria

*This project was completed as part of an Excel for Data Analysis course capstone, applying statistical and analytical techniques to a real-world retail operations scenario.*

---

> *"Without data, you're just another person with an opinion."* — W. Edwards Deming
