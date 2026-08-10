# Personal-Finance-Tracker-Dashboard-Excel-Power-BI
End-to-end personal finance analytics project using Excel and Power BI to clean, model, analyze, and visualize financial transactions.

## Project Overview

This is an end-to-end financial analytics project designed to provide a clear view of personal financial activity across accounts, transactions, categories, and recipients. The project transforms transaction-level data into an interactive dashboard that allows users to monitor balances, analyze income and expenses, track monthly financial performance, identify major spending categories and recipients, and understand transaction trends over time.

The data was prepared and validated using **Microsoft Excel**, including data cleaning, standardization, and preparation for analysis. **Power BI** was then used to build a relational data model, create a dedicated calendar table, develop DAX measures for financial and time-based analysis, and deliver an interactive dashboard with dynamic filtering, conditional formatting, contribution analysis, and custom SVG-based visuals.

## Problem Statement

Managing personal finances across multiple accounts and transactions can make it difficult to understand where money is coming from, where it is being spent, and how financial activity changes over time. Transaction data on its own provides individual records but does not readily provide a consolidated view of financial performance.

The objective of this project was to transform transaction-level financial data into an interactive reporting solution that makes key financial information easier to monitor and interpret. The dashboard was designed to quickly assess balances, compare income and expenses, track monthly changes, identify major transaction categories, and understand which recipients account for a significant share of financial activity.

## Data Disclaimer

The financial data used in this project is **sample data** created for analytical and demonstration purposes. It does not represent real personal financial information or actual transactions.

## Project Objectives

- Prepare and validate transaction data for analysis using Microsoft Excel.
- Build a structured relational data model in Power BI using fact and dimension tables.
- Create time-based analysis using a dedicated calendar table and DAX time-intelligence measures.
- Monitor key financial metrics including balances, income, expenses, and net income.
- Analyze financial activity by transaction type, category, subcategory, and recipient.
- Develop an interactive dashboard that enables users to explore financial performance and trends at a glance.

## Data Preparation

The project began with multiple financial datasets containing transaction, account, category, subcategory, and recipient information. The datasets were reviewed and prepared in Microsoft Excel before being imported into Power BI.

The preparation process included:

- Reviewing the structure and purpose of each dataset.
- Identifying fact and dimension tables for the analytical model.
- Standardizing column names and data formats.
- Reviewing data types, particularly dates and numerical fields.
- Checking for missing values and inconsistencies.
- Reviewing the datasets for duplicate or invalid records.
- Cleaning and validating transaction-level data where necessary.
- Preparing recipient and category-related lookup data for analysis.
- Performing data-quality checks before loading the data into Power BI.

The cleaned and validated datasets were then used as the foundation for the Power BI data model and dashboard.

## Data Modelling

The prepared datasets were imported into Power BI and structured into a relational data model to support flexible filtering and analysis.

The model was organized around a central `Fact_Finance` transaction table supported by dimension tables for accounts, categories, recipients, subcategories, and dates.

Key modelling steps included:

- Identifying `Fact_Finance` as the central transaction table.
- Creating and structuring the required dimension tables.
- Creating a dedicated `dim_Subcategory` table to improve the organization of subcategory information.
- Merging the required subcategory information into the appropriate dimension structure.
- Establishing relationships between the fact table and dimension tables using their corresponding keys.
- Creating a dedicated `Calendar` table covering the minimum and maximum transaction dates.
- Connecting the Calendar table to `Fact_Finance` to support consistent date filtering and time-based analysis.
- Creating a dedicated measures table to organize the project's DAX measures separately from the underlying data tables.

## DAX & Analytical Calculations

DAX measures were created in Power BI to transform the transaction data into meaningful financial metrics and support dynamic analysis.

The measures covered several areas:

- **Core financial metrics:** Total Amount, Income, Expense, Net Income, and Total Balance.
- **Time intelligence:** Previous-month Income, Expense, and Net Income using the Calendar table.
- **Variance analysis:** Current-period versus previous-period variance for income, expenses, and net income.
- **Growth analysis:** Percentage growth measures to identify changes in financial performance.
- **Dynamic indicators:** Conditional color measures and percentage indicators with directional arrows to communicate positive and negative movements.
- **Contribution analysis:** Measures to determine the contribution of individual categories and recipients to overall transaction value.
- **Custom visual calculations:** SVG-based measures used to create contribution progress bars and monthly transaction sparklines.

These measures were organized in a dedicated measures table to keep the model structured and make the dashboard calculations easier to manage.

## Dashboard & Visualizations

The final Power BI dashboard was designed to provide a consolidated view of financial activity and allow users to explore their finances interactively.

![Personal Finance Tracker Dashboard](screenshots/dashboard-overview.png)

Key dashboard components include:

- **Financial overview:** KPI cards displaying account balances, income, expenses, and overall transaction value.
- **Transaction composition:** Donut charts showing the distribution of financial activity across transaction types and categories.
- **Monthly performance:** Current-month and previous-month comparisons for income, expenses, and net income, supported by variance and growth indicators.
- **Income vs. Expense analysis:** A comparative monthly view of money flowing into and out of the accounts.
- **Net income trend:** An area chart showing how net income changes over time.
- **Category analysis:** An interactive matrix showing transaction types, categories, total amounts, and each category's contribution to overall transaction value.
- **Trend analysis:** Custom SVG sparklines showing how transaction amounts change across months for individual categories.
- **Recipient analysis:** A recipient-level view showing transaction amounts and each recipient's contribution to total financial activity.
- **Interactive filtering:** Slicers and category selections allow users to dynamically explore different periods, transaction types, and categories.
- **Dynamic visual indicators:** Conditional formatting, directional arrows, contribution bars, and custom SVG elements were used to make changes and proportions easier to interpret.

## Key Insights

The dashboard provides a consolidated view of financial activity and highlights patterns across income, expenses, categories, monthly performance, and recipients.

Key findings from the analysis include:

- **Income performance:** 
- **Expense patterns:** 
- **Category contribution:** 
- **Monthly financial performance:** 
- **Recipient concentration:** 

## Tools & Technologies

- **Microsoft Excel** — Data preparation, cleaning, validation, and initial data review.
- **Power BI** — Data modelling, DAX calculations, interactive visualization, and dashboard development.
- **DAX** — Time intelligence, financial metrics, variance and growth analysis, contribution analysis, and custom SVG visuals.

## Project Files

- **Dashboard screenshots** — Static previews of the completed Power BI dashboard.
- **Data model documentation** — Overview of the fact and dimension tables and their relationships.
- **Selected DAX measures** — Key calculations used for financial metrics, time intelligence, variance, growth, and contribution analysis.
- **Project documentation** — Details of the data preparation, modelling, analysis, and findings.

## Recommendations

Based on the analytical framework developed in the dashboard, the solution can support better financial decision-making by helping users:

- Monitor income and expense movements across different periods.
- Identify categories that account for a significant share of total spending.
- Track recurring or high-value transactions and recipients.
- Compare current financial performance with previous periods.
- Investigate significant changes in income, expenses, and net income.
- Use contribution and trend analysis to identify areas where spending patterns may require attention.

