# Credit Card Financial Power BI Dashboard

## Dashboards

### Credit Card Transcation Dashboard
![WhatsApp Image 2024-07-17 at 18 37 35 (1)](https://github.com/user-attachments/assets/f461d0df-cca7-4a38-a5a2-b32967f6c77e)

The Credit Card Transaction Report Dashboard provides an overview of key performance metrics including total revenue ($57M), transaction amount ($45.5M), and interest earned ($8M). It breaks down these metrics by card category, expenditure type, customer job, and education level. The dashboard also visualizes quarterly trends in revenue and transaction count, highlighting significant changes. Additionally, it includes customer acquisition costs and revenue distribution based on transaction methods (Swipe, Chip, Online). Filters for week start date, gender, and card category allow for detailed data analysis.

### Credit Card Customer Dashboard
![WhatsApp Image 2024-07-17 at 18 37 35](https://github.com/user-attachments/assets/e6c36d2c-0691-4fda-8bee-bdd890c2fe3f)

The Credit Card Customer Report Dashboard provides key insights into customer demographics and financial performance. It highlights total revenue ($57M), total interest ($8M), and income ($588M), along with a Customer Satisfaction Score (CSS) of 3.19. The dashboard breaks down revenue by age group, marital status, income group, dependents, and education level. It also shows revenue trends by week and top contributing states, with detailed insights into revenue and interest earned by customer job categories. Filters for week start date, gender, and card category enable in-depth analysis.


## Project Objective

To develop a comprehensive credit card weekly dashboard that provides real-time insights into key performance metrics and trends, enabling stakeholders to monitor and analyze credit card operations effectively.

## Dataset

The dataset was sourced from two separate CSV files containing real-time credit card data of customers and transactions, each consisting of 10,293 rows.

## Data Importation

The data was imported into a PostgreSQL database using the following steps:

1. Prepared CSV files
2. Created tables in SQL
3. Imported CSV files into SQL

## Data Preprocessing

Several DAX queries were utilized for data preprocessing:

```DAX
AgeGroup = SWITCH(
    TRUE(),
    'public cust_detail'[customer_age] < 30, "20-30",
    'public cust_detail'[customer_age] >= 30 && 'public cust_detail'[customer_age] < 40, "30-40",
    'public cust_detail'[customer_age] >= 40 && 'public cust_detail'[customer_age] < 50, "40-50",
    'public cust_detail'[customer_age] >= 50 && 'public cust_detail'[customer_age] < 60, "50-60",
    'public cust_detail'[customer_age] >= 60, "60+",
    "unknown"
)

IncomeGroup = SWITCH(
    TRUE(),
    'public cust_detail'[income] < 35000, "Low",
    'public cust_detail'[income] >= 35000 && 'public cust_detail'[income] <70000, "Med",
    'public cust_detail'[income] >= 70000, "High",
    "unknown"
)

week_num2 = WEEKNUM('public cc_detail'[week_start_date])

Revenue = 'public cc_detail'[annual_fees] + 'public cc_detail'[total_trans_amt] + 'public cc_detail'[interest_earned]

Current_week_Revenue = CALCULATE(
    SUM('public cc_detail'[Revenue]),
    FILTER(
        ALL('public cc_detail'),
        'public cc_detail'[week_num2] = MAX('public cc_detail'[week_num2])
    )
)

Previous_week_Revenue = CALCULATE(
    SUM('public cc_detail'[Revenue]),
    FILTER(
        ALL('public cc_detail'),
        'public cc_detail'[week_num2] = MAX('public cc_detail'[week_num2]) - 1
    )
)
```

## Project Findings & Insights

### Week on Week Change

- Revenue increased by 28.8%
- Total Transaction Amount increased by 25.9%

### Overview Year to Date

- Overall revenue is $57M
- Total interest is $8M
- Total transaction amount is $46M
- Male customers contribute more in revenue ($31M) compared to female customers ($26M)
- Blue and Silver credit cards contribute to 93% of overall transactions
- TX, NY, and CA contribute to 68% of the overall revenue
- Overall activation rate is 57.5%
- Overall delinquent rate is 6.06%
- Self-employed individuals have the highest delinquent rate at 1.66%
