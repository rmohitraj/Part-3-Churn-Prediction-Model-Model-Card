# Model Card: Customer Churn Prediction Model

## 1. Model Details
*   **Model Name**: Customer Churn Prediction Model
*   **Version**: 1.0
*   **Developer**: [Your Name/Team Name]
*   **Date**: 2024-06-18
*   **Algorithm**: LightGBM Classifier (LGBMClassifier)
*   **Source Code/Repository**: [If applicable, link to code repository]

## 2. Intended Use
*   **Purpose**: To predict whether a customer will churn within the next 60 days (`churn_next_60d`).
*   **Primary Users**: Customer retention teams, marketing departments, business strategists.
*   **Use Cases**: 
    *   Identify at-risk customers for proactive retention campaigns.
    *   Allocate marketing and retention resources more efficiently.
    *   Personalize customer engagement strategies.
    *   Analyze churn drivers to inform product and service improvements.

## 3. Data
*   **Dataset Used**: `rfm_modeling_snapshot.csv`
*   **Source**: Google Drive: `/content/drive/MyDrive/Capstone Project/d2c churn data package/rfm_modeling_snapshot.csv`
*   **Snapshot Date**: Data represents customer states on `2025-09-30` (as indicated by `snapshot_date`).
*   **Data Splitting**: A time-based split was used to create:
    *   **Training Set**: 1728 samples
    *   **Validation Set**: 336 samples
    *   **Test Set**: 336 samples
*   **Preprocessing**: 
    *   Missing values in `loyalty_tier` were imputed with 'Unknown'.
    *   `snapshot_date` was converted to datetime objects.
    *   Categorical features were one-hot encoded (`pd.get_dummies`), ensuring consistent feature sets across all data splits.
*   **Target Variable**: `churn_next_60d` (binary: 1 for churn, 0 for no churn).
*   **Features**: Includes RFM (Recency, Frequency, Monetary) metrics, customer demographics (city_tier, age_group), acquisition channel, loyalty status, preferred product categories, marketing consent, customer service interactions, and recent engagement behaviors.

## 4. Model Approach
*   **Algorithm**: Light Gradient Boosting Machine (LightGBM) Classifier.
*   **Key Hyperparameters (Default)**:
    *   `objective='binary'`
    *   `metric='auc'`
    *   `random_state=42`
*   **Feature Engineering**: One-hot encoding for categorical variables. No complex feature engineering beyond what was present in the raw data or implied by RFM calculations.

## 5. Performance (on Validation Set)
*   **Chosen Model**: LightGBM Classifier
*   **Key Metrics**:
    *   **Accuracy**: 0.7887
    *   **Precision**: 0.7568
    *   **Recall**: 0.7619
    *   **F1-Score**: 0.7593
    *   **ROC-AUC**: 0.8744
*   **Optimal Decision Threshold**: 0.25 (selected to prioritize Recall)
    *   **Precision at 0.25 threshold**: ~0.69
    *   **Recall at 0.25 threshold**: ~0.88

## 6. Limitations
*   **Data Bias**: The model is trained on historical data up to `2025-09-30`. Customer behavior and market conditions may change, potentially reducing model accuracy over time.
*   **Generalizability**: Performance may degrade if applied to customer segments or markets significantly different from the training data.
*   **Interpretability**: While LightGBM provides feature importances, individual predictions can be less transparent than simpler models.
*   **False Positives (57 on validation set)**: Customers predicted to churn but who do not. These lead to unnecessary retention costs and potential customer annoyance if targeted with irrelevant offers.
*   **False Negatives (19 on validation set)**: Customers predicted not to churn but who do. These represent missed opportunities for intervention and result in lost revenue.
*   **Imputation Strategy**: Imputing `loyalty_tier` with 'Unknown' assumes a distinct category for missing values, which might not always reflect reality.

## 7. Ethical Considerations
*   **Fairness/Bias**: 
    *   Potential for algorithmic bias if certain demographic groups (e.g., age_group, city_tier) are underrepresented or behave differently in ways not captured by the model, leading to unfair targeting or neglect.
    *   Retention offers could inadvertently exacerbate inequalities if certain customer segments consistently receive more favorable treatment based on model predictions.
*   **Transparency**: Explaining model decisions to customers (e.g., why they received a retention offer) could be challenging due to the complexity of the LightGBM model.
*   **Privacy**: Ensuring customer data used for prediction is handled securely and in compliance with privacy regulations (e.g., GDPR, CCPA).

## 8. Monitoring and Maintenance
*   **Performance Monitoring**: Regularly track key metrics (Precision, Recall, F1-score, ROC-AUC) on fresh data to detect model degradation (concept drift or data drift).
*   **Data Drift Monitoring**: Monitor feature distributions and target variable distribution for changes over time.
*   **A/B Testing**: Continuously test retention strategies informed by model predictions to measure their effectiveness and refine the model or business interventions.
*   **Retraining**: Establish a schedule for retraining the model (e.g., quarterly, semi-annually) using the most recent data to ensure its continued relevance and accuracy.
*   **Feedback Loop**: Implement mechanisms to collect feedback from retention campaigns to improve model features or strategy.

## 9. When Not to Use
*   **Without Human Oversight**: The model should not be used for fully automated decision-making without human review, especially for critical customer interventions.
*   **For Guiding Discriminatory Practices**: The model's predictions should not be used to justify or perpetuate unfair or discriminatory practices against any customer group.
*   **On Outdated Data**: Do not use the model if the underlying customer behavior or market conditions have drastically changed since the last training, without retraining.
*   **For Predicting Churn Outside 60-Day Window**: The model is specifically trained for `churn_next_60d`; applying it to predict churn over different timeframes may lead to inaccurate results.
