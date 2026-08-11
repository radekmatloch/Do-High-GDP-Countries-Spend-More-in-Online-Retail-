# E-Commerce Customer Spending and National GDP: A Cross-Country Analysis with an Interactive Dashboard

A data-science project linking online-retail transaction data to national economic indicators, with an interactive Looker Studio dashboard. Graduate coursework, National Chengchi University (2025).

## Dashboard

Interactive Looker Studio report: https://lookerstudio.google.com/reporting/800a0432-9416-4f8a-b2f2-8637f28d6a91/page/1nqKF

## Project Description

This project asks whether a country's **GDP per capita** is associated with how much its customers spend on a UK-based online retailer, and how much of that spending is explained instead by **purchasing behaviour**. Transaction records from the UCI *Online Retail* dataset (2011) are cleaned and aggregated to the country level, then merged with World Bank GDP-per-capita figures for the same year. The relationship is examined through descriptive comparisons, a linear regression, and a set of summary tables feeding an interactive dashboard.

The main finding is that average spending per customer is explained almost entirely by how frequently customers purchase, not by national wealth. In a regression of average spending on GDP per capita and average number of purchases (n = 19 countries), purchase frequency is a strong, highly significant predictor while GDP per capita is not statistically significant. The model explains roughly 86% of the cross-country variation in average spending.

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

### Results

- **Purchase frequency dominates.** Average number of purchases per customer is a strong, highly significant predictor of average spending per customer (p < 0.001).
- **National wealth is not decisive.** GDP per capita is not a statistically significant predictor of average spending once purchase frequency is accounted for (p ≈ 0.39).
- **Model fit.** The two-predictor model explains about 86% of the cross-country variation in average spending (R² ≈ 0.86, n = 19), with the explanatory power concentrated in purchasing behaviour rather than economic level.

## Author

**Radek Matloch** — data cleaning, aggregation, analysis, regression, dashboard design.
