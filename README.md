# Python for Data Analytics: AnalytixLabs case studies (2023)

In January 2023 I left my operations role at ITILITE to retrain in data analytics full-time, starting from the basics. This repo holds the Python work from that year: eight notebooks, from Python fundamentals to an end-to-end e-commerce capstone, completed as part of the **AnalytixLabs Data Analytics** programme (certificate issued 2024).

The questions were set by the course; the code, analysis and charts are mine. I've kept the notebooks as I wrote them in 2023 (only local file paths and an embedded auto-profiling report were removed), and added a short **"What I'd fix now"** note for each one below. Reading your old work critically is part of learning.

**Stack:** Python · pandas · NumPy · Matplotlib · Seaborn · SciPy (t-tests, ANOVA, chi-square, Pearson) · Jupyter

---

## The notebooks

| # | Notebook | Business question | Skills |
|---|---|---|---|
| 01 | [Python basics](notebooks/01_python_basics.ipynb) | 20+ exercises: operators, loops, type checks, user-defined functions | Core Python |
| 02 | [Data manipulation & visualisation](notebooks/02_data_manipulation_and_visualisation.ipynb) | Guided drills on 10 datasets (Chipotle orders, cars, students, retail, wind, Apple stock…) | Import, clean, filter, group, merge, plot |
| 03 | [Retail](notebooks/03_retail_case_study.ipynb) | Who buys what, where, and through which store channel? | Joins, summaries, frequency tables, age bands, date filters |
| 04 | [Credit card](notebooks/04_credit_card_case_study.ipynb) | How much does a bank earn from customer spend vs repayment, and who spends the most? | Business-rule cleaning, age groups, 2.9% interest model, city/product/time charts |
| 05 | [Insurance claims](notebooks/05_insurance_claims_case_study.ipynb) | Which claims look risky, and do claim amounts differ by gender, age or segment? | Data audit, fraud-alert flag, de-duplication, imputation, hypothesis tests |
| 06 | [Sales visualisation](notebooks/06_sales_visualisation_case_study.ipynb) | How did 2016 sales compare with 2015 by region, tier, division and quarter? | Grouped bars, pies, `np.where` quarters |
| 07 | [Hypothesis testing](notebooks/07_hypothesis_testing_case_study.ipynb) | Five business problems: loan interest rates, price quotes, a re-engineering programme, job prioritisation, customer satisfaction | t-tests, ANOVA, chi-square |
| 08 | [E-commerce marketing capstone](notebooks/08_ecommerce_marketing_analytics_capstone.ipynb) | End-to-end: KPIs, customer acquisition and retention, seasonality, segmentation, payments, ratings, delivery | 8-table merge, cohort-style metrics, segmentation, 6 charts |

---

## Highlights

**08 · E-commerce capstone (the biggest one)**
- Merged 8 tables (customers, orders, items, payments, reviews, products, sellers, geolocation) into one analysis table.
- Headline KPIs: 32,951 products · 71 categories · 3,095 sellers · 5 payment methods.
- Credit card was used in about 74% of payments; UPI was second.
- Bed/Bath/Table was the most-ordered category in every month.
- Security & Services, Diapers & Hygiene and Office Furniture had the lowest average ratings.
- Average delivery took 13 days. Among orders slower than two weeks, several states averaged 35+ days, which is a clear lever for improving ratings.

**05 · Insurance claims**
- Built a 1/0 alert flag for injury claims that weren't reported to the police.
- De-duplicated to one latest claim per customer.
- Fixed two-digit birth years that were parsing as 2070s.
- Adults (30–60) filed the most fraudulent claims.
- No significant difference in claim amount between men and women (t-test, p = 0.37), or across age groups (ANOVA, p = 0.86).

**04 · Credit card**
- Worked through the brief's cleaning rules: under-18 ages replaced with the mean age, and spend above the card limit replaced with 50% of the limit.
- Modelled monthly bank profit at 2.9% interest, earned only on positive balances.
- Spend is highest in the 25–34 age group.
- Petrol, camera and food are the top spend categories.
- Both of these results still hold when I re-checked them in 2026 without the merge issue described below.

**03 · Retail**
- e-Shop is the biggest channel by both value and quantity: roughly double any other store type.
- Male and female customers buy a very similar category mix, with Books the largest category for both (about 26% of transactions).
- Both of these results still hold on the corrected join.

---

## What I'd fix now

I learned the most in the months after writing these, so here's what I'd change if I did them again.

- **03 Retail: the join key.** I merged products on category code only. Each category has several sub-categories, so every transaction was duplicated (23,053 rows became 99,293) and the ₹ totals are overstated by about 4×. The fix is to join on category **and** sub-category. Rankings such as "e-Shop leads" still hold; the absolute amounts don't. The city answer should also count customers per city rather than dividing the maximum code by a count.
- **04 Credit card: the many-to-many merge.** I joined spend to repayments on customer only, which multiplied rows (1,500 became 37,284) and inflated the profit figure. The fix is to aggregate each table to customer × month first, then join. In my second version, the two limit rules wrote to a new column instead of the amounts, so the analysis table kept the original values. The final question (a user-defined top-10 function) is unfinished.
- **05 Insurance: reading p-values.** In Q20 the p-value is 0.43, so the conclusion should be *no* relationship; I wrote the opposite. In Q18 I tested two yearly averages against $10,000, but n = 2 is far too small. The test should use the individual claims from the current year.
- **08 Capstone: time and retention.** I grouped by calendar month across 2016–18, which mixes years. My "retention" was a month-over-month difference in customer counts, which is why it went above 100% and "new customers" went negative. A proper cohort table (first-purchase month × months since) is the right tool. The cross-selling question is still open.
- **General:** use relative paths, fewer `inplace=True` calls, one clear answer per question, and a short written insight after each chart.

---

## Not included

- **Datasets and question papers.** These belong to AnalytixLabs, so they aren't redistributed here. Notebooks read from a local `data/` folder.
- **SQL, Excel and Power BI** are in their own repo: [retail-customer-analysis-sql-powerbi](https://github.com/ameer29/retail-customer-analysis-sql-powerbi), an end-to-end retail case (SQL Server → Excel pivots and Pareto → 3-page Power BI dashboard). The Python **supply-chain capstone** is in [supply-chain-inventory-analysis](https://github.com/ameer29/supply-chain-inventory-analysis).

---

[← Back to my profile](https://github.com/ameer29) · [Portfolio site](https://ameer29.github.io)
