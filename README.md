# E-Commerce Customer Spending and National GDP: A Cross-Country Analysis with an Interactive Dashboard

A data-science project linking online-retail transaction data to national economic indicators, with an interactive Looker Studio dashboard. Graduate coursework, National Chengchi University (2025).

## Dashboard

Interactive Looker Studio report: https://lookerstudio.google.com/reporting/800a0432-9416-4f8a-b2f2-8637f28d6a91/page/1nqKF

## Project Description

This project asks whether a country's **GDP per capita** is associated with how much its customers spend on a UK-based online retailer, and how much of that spending is explained instead by **purchasing behaviour**. Transaction records from the UCI *Online Retail* dataset (2011) are cleaned and aggregated to the country level, then merged with World Bank GDP-per-capita figures for the same year. The relationship is examined through descriptive comparisons, a linear regression, and a set of summary tables feeding an interactive dashboard.

The substantive finding is that national GDP per capita shows no statistically significant association with average customer spending across the 19 countries analysed. Spending does scale with purchase frequency, but that relationship is largely mechanical rather than explanatory, so it is reported below with that caveat rather than treated as a headline result.

## Getting Started

### Prerequisites

- **Python 3.10+** (written in Google Colab)

```bash
pip install pandas numpy matplotlib seaborn statsmodels scipy openpyxl xlrd
```

> **Note on file paths.** The notebook was written in Google Colab and mounts Google Drive. Update the `retail_path`, `gdp_path`, and `output_path` variables near the top to point to wherever you store the input files (or adapt them to local relative paths).

## File Structure

```
├── README.md
├── Final_project.ipynb        # Python: cleaning, analysis, regression, dashboard tables
├── Online Retail.xlsx         # raw transaction data (UCI Online Retail)
├── GDP.xls                    # World Bank GDP per capita (World Development Indicators)
└── dashboard_tables.xlsx      # generated country-level tables (Looker Studio input)
```
> Rename `Final_project.ipynb` to match your actual notebook filename.

## Analysis

### Methods

**Data cleaning.** The *Online Retail* data (541,909 raw transactions) are filtered to 2011 and cleaned: rows with missing customer, country, invoice, quantity, or price are dropped; cancelled invoices (invoice numbers beginning with "C") and non-positive quantities are removed. Line-level revenue is computed as quantity × unit price. The cleaned data cover 4,220 customers and 17,136 transactions, totalling approximately £8.34 million.

**Aggregation and merge.** Transactions are aggregated to the country level, retaining only countries with at least five customers. Country-level measures include average spending per customer, number of customers, average number of purchases per customer, total revenue, average quantity per order, and number of unique products. These are merged with World Bank GDP-per-capita figures for 2011, yielding a final analytical dataset of 19 countries.

**Modelling.** The analysis combines univariate summaries (top countries by customers and by average spending), a bivariate scatter of GDP per capita against average spending with a fitted regression line, and an ordinary least squares regression of average spending per customer on GDP per capita and average number of purchases (estimated with `statsmodels`). Multicollinearity is checked using the variance inflation factor. Countries are grouped into GDP quartile tiers to compare observed and model-predicted spending. Summary tables are exported to Excel as the data source for the Looker Studio dashboard.

- **No GDP effect.** National GDP per capita is not a statistically significant predictor of average customer spending (p ≈ 0.39). Within this sample of 19 countries, national wealth does not explain how much customers spend.
- **The high R² is mostly mechanical.** The two-predictor model reports R² ≈ 0.86 (n = 19), but this is driven by average number of purchases, which is a component of the outcome: a customer's total spending is, by construction, their number of purchases multiplied by their average order value. Frequency therefore predicts total spending almost by definition, and its large coefficient should not be read as a behavioural insight.
- **Limitations.** With only 19 country-level observations, the regression is sensitive to a few high-volume (wholesale-type) countries. A cleaner specification would use an outcome not mechanically tied to frequency — for example, average order value or spending per transaction — before drawing conclusions about what drives spending.

## Author

**Radek Matloch** — data cleaning, aggregation, analysis, regression, dashboard design.
