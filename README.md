# Credit Card Clustering Analysis

This project analyzes credit card customer data to uncover spending patterns, credit utilization, payment behaviors, and customer segmentation opportunities. The analysis leverages SQL queries on the provided dataset (`CC GENERAL.csv`) to extract actionable insights for financial institutions.

## Project Structure

- `CC GENERAL.csv`: Main dataset containing anonymized credit card customer records.
- `credit_clustering.session.sql`: SQL session file with queries for data exploration and feature engineering.
---
```sql
CREATE TABLE credit (
customer_id VARCHAR(20),
balance FLOAT,
balance_frequency FLOAT,
purchases FLOAT,
oneoff_purchases FLOAT,
installment_purchases FLOAT,
cash_advance FLOAT,
purchases_frequency FLOAT,
oneoff_purchases_frequency FLOAT,
purchases_installment_frequency FLOAT,
cash_advance_frequency FLOAT,
cash_advance_trx INT,
purchases_trx INT,
credit_limit INT,
payments FLOAT,
minimum_payment FLOAT,
prc_full_payment FLOAT,
tenure INT
)
```

*viewing the table*
```sql
SELECT * FROM credit;
```

**1. Customer Spending Behavior**
-- a) average purchase per customer
```sql
SELECT 
customer_id,
AVG(purchases) AS avg_purchase_per_customer
FROM credit
GROUP BY 1
ORDER BY 2 DESC
LIMIT 10;
```

b) What percentage of purchases are made in installments vs. one-off?
```sql
SELECT
customer_id,
installment_purchases/ NULLIF(purchases, 0) AS installment_ratio,
oneoff_purchases/ NULLIF(purchases, 0) AS oneoff_ratio
FROM credit
WHERE purchases > 0
ORDER BY 2 DESC
LIMIT 10;
```

c) What is the ratio of cash advances to total spending?
```sql
SELECT
customer_id,
cash_advance/ (cash_advance + purchases) AS cash_vs_spending_ratio
FROM credit
WHERE (cash_advance + purchases) > 0
ORDER BY 2 DESC
LIMIT 10;
```

**2. Customer Credit Utilization**
d) What percentage of the credit limit is being used (utilization)?
```sql
SELECT
customer_id,
balance/ NULLIF(credit_limit, 0) AS credit_utilization
FROM credit
WHERE balance/ NULLIF(credit_limit, 0) IS NOT NULL
ORDER BY 2 DESC
LIMIT 10
```

e) How consistent are users with paying their minimum payments?
```sql
SELECT 
customer_id,
minimum_payment / NULLIF(payments, 0) AS min_payment_ratio
FROM credit
WHERE minimum_payment / NULLIF(payments, 0) IS NOT NULL
ORDER BY 2 DESC
LIMIT 10;
```

**3. Customer Transaction Patterns**
f) How frequently do customers make purchases?
```sql
SELECT
customer_id,
COUNT(purchases_trx) AS purchase_frequency
FROM credit
GROUP BY customer_id
ORDER BY purchase_frequency DESC
LIMIT 10;
```

g) How frequently do customers use their cards (average frequency)?
```sql
SELECT
customer_id,
(balance_frequency + purchases_frequency   + cash_advance_frequency) / 3 AS avg_frequency
FROM credit
ORDER BY 2 DESC
LIMIT 10
```

h) Which users rely heavily on cash advances (frequency & volume)?
```sql
SELECT * FROM credit
SELECT 
customer_id,
cash_advance_frequency,
cash_advance
FROM credit
WHERE cash_advance > 0
```

**4. Derived Indicators For Clustering**
i) average tenure spent
```sql
SELECT
customer_id,
ROUND((purchases + cash_advance)::integer / NULLIF(tenure, 0),0) AS avg_tenure_spent
FROM credit
WHERE tenure > 0
ORDER BY 2 DESC
```

j) average tenure payment
```sql
SELECT
customer_id,
ROUND((payments)::integer / NULLIF(tenure, 0),0) AS avg_tenure_payment
FROM credit
WHERE tenure > 0
ORDER BY 2 DESC;
```

k) What is the payment to credit limit ratio?
```sql
SELECT
customer_id,
ROUND(payments::integer / NULLIF(credit_limit, 0),0) AS payment_to_credit_limit_ratio
FROM credit
WHERE credit_limit > 0
ORDER BY 2 DESC
```

l) Who pays more than their minimum payment regularly?
```sql
WITH payment_reg
AS
(
SELECT
customer_id,
CASE
    WHEN payments > minimum_payment THEN 'Above Minimum'
    ELSE 'Below Minimum'
END AS payment_status
FROM credit
WHERE payments IS NOT NULL
)
SELECT
payment_status,
COUNT(*) AS customer_count
FROM payment_reg
GROUP BY 1
```


## Key Analyses

- **Customer Spending Behavior**: Examines average purchases, installment vs. one-off purchases, and cash advance ratios.
- **Credit Utilization**: Measures how much of the credit limit is used and payment consistency.
- **Transaction Patterns**: Analyzes purchase frequency and card usage.
- **Derived Indicators**: Calculates average tenure spending, payment-to-credit-limit ratios, and payment behaviors.

---

## Findings

### 1. Spending Patterns
- **Installment vs. One-off Purchases**: Customers show a mix of installment and one-off purchases, with some relying heavily on one type.
- **Cash Advance Usage**: A subset of users frequently uses cash advances, indicating possible short-term liquidity needs.

### 2. Credit Utilization
- **Utilization Ratios**: Some customers consistently use a high percentage of their credit limit, which may signal financial stress or high engagement.
- **Minimum Payment Consistency**: There is a significant group of customers who only pay the minimum amount due, increasing their risk of accumulating debt.

### 3. Payment Behavior
- **Above Minimum Payments**: The majority of customers pay above the minimum, but a notable portion does not, which could impact their credit health.
- **Payment-to-Credit Limit Ratio**: Variability in this ratio suggests differing repayment capacities and financial discipline across the customer base.

### 4. Transaction Frequency
- **Purchase Frequency**: High-frequency users are easily identifiable, representing valuable segments for targeted offers.
- **Average Card Usage**: Some customers use their cards infrequently, indicating potential for increased engagement.

---

## Recommendations

1. **Targeted Financial Education**
   - Educate customers who pay only the minimum about the long-term costs of revolving credit and benefits of paying more than the minimum.

2. **Personalized Offers**
   - Offer tailored rewards or credit line increases to high-frequency and high-utilization customers to enhance loyalty and engagement.
   - Provide installment plan promotions to users who prefer one-off purchases to encourage more consistent spending.

3. **Risk Monitoring**
   - Closely monitor customers with high credit utilization and frequent cash advances for early signs of financial distress.
   - Proactively reach out to these customers with financial planning resources or flexible repayment options.

4. **Engagement Campaigns**
   - Design campaigns to increase card usage among low-frequency users, such as limited-time cashback or bonus point offers.

5. **Product Development**
   - Consider developing new credit products or features (e.g., flexible payment plans, budgeting tools) for segments identified as at-risk or under-engaged.

---

## Getting Started

1. **Load the Data**: Import `CC GENERAL.csv` into your SQL environment.
2. **Run Analyses**: Execute queries from `credit_clustering.session.sql` to reproduce the findings or customize for deeper exploration.
3. **Interpret Results**: Use the output to inform business strategies, customer segmentation, and product development.

---

*Awal Alier ; Reading between the data!*
