# Part-3-Churn-Prediction-Model-Model-Card
# Customer Churn Prediction Project

## Overview
This project focuses on building and evaluating a customer churn prediction model. The goal is to identify customers at risk of churning within the next 60 days, enabling proactive retention efforts and optimized resource allocation.

## Problem Statement
Customer churn is a significant challenge for businesses, leading to lost revenue and increased acquisition costs. By accurately predicting which customers are likely to churn, businesses can implement targeted interventions to improve customer retention and lifetime value.

## Data
*   **Dataset**: `rfm_modeling_snapshot.csv`
*   **Source**: Provided for the project, assumed to be collected up to `2025-09-30`.
*   **Features**: Includes RFM (Recency, Frequency, Monetary) metrics, customer demographics (city tier, age group), acquisition channel, loyalty status, preferred product categories, marketing consent, customer service interactions, and recent engagement behaviors.
*   **Target Variable**: `churn_next_60d` (binary: 1 for churn, 0 for no churn).

## Methodology
1.  **Data Loading and Initial Inspection**: Loaded `rfm_modeling_snapshot.csv` and performed initial checks for missing values and data types. Handled missing `loyalty_tier` values by imputing with 'Unknown'.
2.  **Data Leakage Prevention**: Employed a time-based split strategy using the `split` column (train, validation, test) to ensure the model learns from historical data and predicts future churn, mimicking real-world scenarios.
3.  **Feature Preparation**: Identified target, exclusion, and feature columns. Categorical features were one-hot encoded.
4.  **Model Training**: 
    *   **Baseline Model**: Logistic Regression was trained as a simple benchmark.
    *   **Stronger Model**: LightGBM Classifier was trained to achieve higher predictive performance.
5.  **Model Evaluation**: Both models were evaluated on the validation set using key churn classification metrics: Accuracy, Precision, Recall, F1-score, ROC-AUC, and Confusion Matrices.
6.  **Decision Threshold Selection**: An optimal decision threshold of `0.25` was selected for the LightGBM model, prioritizing Recall to minimize False Negatives (missed churners), based on business implications.
7.  **Error Analysis**: Performed detailed analysis of False Positives and False Negatives, identifying specific customer examples and discussing their business risks.
8.  **Feature Importance Explanation**: Used LightGBM's feature importances to identify and explain the top drivers of churn, providing actionable insights.

## Key Findings and Results
*   **Data Split**: Successfully separated data into train (1728), validation (336), and test (336) sets using the predefined 'split' column.
*   **Model Performance (LightGBM on Validation Set)**:
    *   Accuracy: `0.7887`
    *   Precision: `0.7568`
    *   Recall: `0.7619`
    *   F1-Score: `0.7593`
    *   ROC-AUC: `0.8744`
*   **Optimal Threshold Impact (`0.25`)**:
    *   Precision: `~0.69` (69% of predicted churners are actual churners)
    *   Recall: `~0.88` (88% of actual churners are identified)
*   **Error Analysis**: 
    *   Identified 57 False Positives (predicted churn, actual no churn) – risk of unnecessary marketing spend.
    *   Identified 19 False Negatives (predicted no churn, actual churn) – risk of lost customers and revenue.
*   **Top Features Driving Churn**: `recency_days`, `monetary_180d`, and `days_since_signup` were found to be the most influential features, with `recency_days` being the strongest predictor.

## Model Card Summary
*   **Intended Use**: Proactive churn prediction for retention campaigns, resource allocation, and strategy formulation.
*   **Data Used**: `rfm_modeling_snapshot.csv` with time-based train/validation/test splits.
*   **Model Approach**: LightGBM Classifier with one-hot encoded categorical features.
*   **Performance**: Achieved good balance between precision and recall on the validation set, with high ROC-AUC.
*   **Limitations**: Potential for data bias, limited generalizability to different markets, interpretability challenges, and inherent trade-offs between False Positives and False Negatives.
*   **Ethical Risks**: Potential for algorithmic bias, fairness concerns in targeting, and data privacy.
*   **Monitoring Needs**: Continuous monitoring of performance metrics, data drift, and regular retraining.
*   **When Not to Use**: Without human oversight, for discriminatory practices, on outdated data, or for predicting churn outside the 60-day window.

## Business Recommendations
Based on the analysis, the following recommendations are made:
1.  **Targeted Re-engagement**: Prioritize customers with high `recency_days` for re-engagement campaigns.
2.  **Value-based Offers**: For customers with low `monetary_180d` showing churn risk, offer personalized incentives to increase engagement and spending.
3.  **Long-Term Loyalty Programs**: Develop specific programs for long-tenure customers (`days_since_signup`) to ensure continued satisfaction.
4.  **Active Monitoring**: Continuously monitor recent engagement metrics (`last_visit_days_ago`, `product_views_30d`, `email_opens_30d`, etc.) to detect early signs of disengagement.
5.  **Strategic Discounting**: Re-evaluate discounting strategies for price-sensitive customers (`avg_discount_pct_180d`).
6.  **Customer Feedback**: Engage customers with low `avg_rating_180d` to address dissatisfaction.
7.  **Feedback Loop**: Implement a system to collect feedback from retention campaigns to refine the model and intervention strategies.
