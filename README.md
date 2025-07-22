# Credit Card Clustering Analysis

This project analyzes credit card customer data to uncover spending patterns, credit utilization, payment behaviors, and customer segmentation opportunities. The analysis leverages SQL queries on the provided dataset (`CC GENERAL.csv`) to extract actionable insights for financial institutions.

## Project Structure

- `CC GENERAL.csv`: Main dataset containing anonymized credit card customer records.
- `credit_clustering.session.sql`: SQL session file with queries for data exploration and feature engineering.

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

*Awal Alier ; Readind between the data!*
