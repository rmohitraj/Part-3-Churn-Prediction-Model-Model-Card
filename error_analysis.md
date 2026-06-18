# Error Analysis: Business Risks of False Positives and False Negatives

## Business Risks of False Positives and False Negatives

**False Positives (FP): Predicted Churn, Actual No Churn (57 customers)**

These are customers who were predicted to churn but ultimately did not. Targeting these customers with retention campaigns carries several business risks:

*   **Unnecessary Costs**: Offering discounts, special promotions, or dedicated support to customers who would have remained loyal anyway incurs direct costs without a corresponding benefit in churn prevention. For example, `CUST00027` (a Tier 1 customer, 45+ age group, Instagram acquisition) or `CUST00048` (Silver loyalty tier, Makeup preferred category) were predicted to churn but did not. Any retention efforts directed at them would have been wasted. The first 10 FP customers generally show moderate to high activity levels (e.g., `sessions_30d`, `product_views_30d`), which might have contributed to the model's high churn probability, despite their eventual non-churn.
*   **Customer Annoyance/Negative Experience**: Customers who are not considering churning might find unsolicited retention offers intrusive or confusing, potentially leading to a negative perception of the brand. They might question why they are being targeted with such offers if they are satisfied customers.
*   **Dilution of Brand Value**: Overuse of retention incentives for loyal customers can devalue the product or service in their eyes, making them expect discounts or special treatment in the future.

**False Negatives (FN): Predicted No Churn, Actual Churn (19 customers)**

These are customers who were predicted *not* to churn but eventually did. This error type represents a direct failure in churn prevention and carries significant business risks:

*   **Lost Revenue and Customer Lifetime Value (CLTV)**: Each false negative represents a lost customer and, consequently, a loss of their future spending. For example, `CUST00093` (Gold loyalty tier, Baby Care preferred category, 16 sessions in 30 days) and `CUST00145` (Unknown loyalty tier, Wellness preferred category, high recency days of 30) were predicted not to churn but did. These represent missed opportunities to intervene and retain valuable customers.
*   **Negative Word-of-Mouth**: Churned customers, especially those who felt neglected, are more likely to share negative experiences, damaging the brand's reputation and potentially deterring new customers.
*   **Lack of Proactive Intervention**: The business misses the opportunity to understand the underlying reasons for churn for these specific customers, preventing the implementation of targeted strategies that could have saved them.
*   **Competitive Advantage Loss**: Losing customers to competitors means not only lost revenue but also an increased market share for rivals.

**Summary of Risk Prioritization**:

Given that the optimal threshold was chosen to prioritize recall, we intentionally accept a higher number of False Positives to minimize False Negatives. This strategy is driven by the understanding that the cost of losing a genuinely churning customer (FN) is typically higher than the cost of an unnecessary retention offer to a loyal customer (FP). While 57 FPs are a concern for cost efficiency, 19 FNs represent significant missed opportunities for customer retention and revenue protection. A deeper dive into the characteristics of these FN customers would be crucial to refine future retention strategies.
