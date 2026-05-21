### Summary

This video presents a detailed walkthrough of designing a machine learning system aimed at predicting **user app deletion within the next $n$ weeks**. The discussion covers problem formulation, feature engineering, data handling, model architecture, training methodology, evaluation metrics, and practical considerations for deployment and improvement. The expert, Satya, draws on extensive experience in large-scale ML systems, particularly in ads ranking and product recommendation.

---

### Problem Definition & Data Overview

- **Objective:** Predict whether a user will delete the app in the next $n$ weeks.
- **Problem Type:** Binary classification with label:
  - $1$ = user deleted the app within the next $n$ weeks.
  - $0$ = user did not delete the app (active user).
- **Labeling Window:** The time frame $n$ (e.g., 2 days, 1 week) is configurable and critical to defining positive and negative samples.
- **Class Imbalance:** App deletion events are rare (e.g., ~1%), implying strong class imbalance.

---

### Features

Features are organized into **four main categories**:

| Category                | Feature Examples                                   | Type              | Description                                                                                   |
|-------------------------|---------------------------------------------------|-------------------|-----------------------------------------------------------------------------------------------|
| 1. Static User Features | User country ($F_1$), gender ($F_2$), age ($F_3$) | Categorical, Numeric | Demographic static info about the user.                                                      |
| 2. Dynamic User Behavior | Number of interactions in last 7, 14, 30 days ($F_5$, $F_6$, ...), time spent daily ($F_7$) | Numeric           | Behavioral metrics capturing app usage patterns and recency.                                 |
| 3. App-related Features  | App version ($F_{13}$), app store ($F_{12}$)      | Categorical       | Information about the app itself, like version and platform (iOS/Android).                    |
| 4. Contextual Features   | Time of day ($F_{10}$), day of week ($F_{11}$)    | Categorical       | Environmental context potentially impacting deletion behavior (e.g., weekends, end of year). |

- **Behavioral features** are critical, as users tend to delete apps after reduced engagement.
- Numerical features are accompanied by **missingness indicators** to handle incomplete data gracefully.
- Categorical features are handled via **one-hot encoding**.
- The dataset includes **14 features ($F_1$ to $F_{14}$)** plus the binary label column.

---

### Data Splitting & Leakage Considerations

- **Time-based split:** Train on $n$ weeks, validate on week $n+1$, test on week $n+2$ to avoid temporal leakage.
- **Minimal leakage:** Some features (e.g., interactions over last 30 days) may overlap training and test periods, but this reflects real-world inference scenarios.
- **User overlap:** Users may appear in both train and test sets; fully user-disjoint splits are not mandatory and may yield overly pessimistic results.
- **Stratified sampling:** Cap the number of samples per highly active user to prevent bias.

---

### Model Architecture

- **Model Type:** Deep Neural Network (DNN) with multiple fully connected layers.
- **Activation:** ReLU in hidden layers, sigmoid in the output layer to produce deletion probability.
- **Loss Function:** Binary Cross-Entropy (log loss).
- **Regularization:** Dropout and batch normalization layers to prevent overfitting.
- **Input Processing:**
  - Categorical features are one-hot encoded.
  - Numeric features are scaled or normalized.
  - Missing numeric values use an additional binary mask feature.
- **Output:** Well-calibrated probability scores to enable downstream product interventions (e.g., notifications).

---

### Training & Evaluation

- **Training Loop:**
  - Uses PyTorch with custom dataset and dataloader implementations.
  - Handles feature encoding, missing data, and label extraction.
  - Optimizer: Adam with tunable learning rate.
- **Evaluation Metrics:**
  - **Area Under the ROC Curve (AUC):** Measures ranking quality under class imbalance.
  - **Calibration metric:** Ratio of mean predicted deletion probability to observed deletion rate to ensure output probabilities are meaningful.
- **Model validation:** Monitor training and validation loss for overfitting/underfitting.
- **Hyperparameter tuning:** Random/grid search for layer sizes, learning rate, dropout, etc.
- **Data batching:** Batch size tuning (minimum 64 suggested for GPU optimization).

---

### Practical Considerations & Challenges

- **Feature privacy:** Some features (e.g., gender, demographics) may be restricted due to privacy/legal concerns, requiring upstream processing or exclusion.
- **Handling missing data:** Use default indices for missing categorical values and missingness flags for numeric data.
- **Scalability:** One global model across geographies and devices is preferred for maintainability, unless a specific region requires separate modeling.
- **Real-world inference:** The model is designed to mimic production usage where features cover recent user activity.
- **Code efficiency:** Consider caching transformed features to avoid redundant processing during multiple epochs.
- **Baseline models:** Suggested to compare against simpler models (logistic regression or gradient-boosted trees) to justify DNN complexity. However, DNN with sigmoid output offers better calibration.

---

### Summary Table: Feature Encoding and Handling

| Feature Type    | Encoding Method          | Missing Data Handling                          |
|-----------------|-------------------------|-----------------------------------------------|
| Categorical     | One-hot encoding via vocabularies | Use a reserved index/category for missing values |
| Numerical       | Float casting + scaling | Binary missingness indicator (1 = missing, 0 = present) |

---

### Key Insights

- **User behavior metrics, especially recent interaction counts and time spent, are pivotal predictors of app deletion.**
- **Well-calibrated output probabilities are crucial** to enable effective product interventions such as notifications.
- **Time-based train-test splits avoid temporal leakage** and better simulate production usage.
- **Handling missing features explicitly improves model robustness** in real-world noisy data.
- **A simple deep neural network with batch norm, dropout, and sigmoid output is an effective starting architecture.**
- **Privacy constraints may limit feature inclusion, requiring careful product collaboration.**
- **Evaluation must consider class imbalance and calibration, not just accuracy.**

---

### Potential Improvements & Future Work

- Explore **different model architectures** or embedding layers for categorical variables.
- Perform **feature importance analysis** to refine input features.
- Implement **feature caching** to improve training efficiency.
- Incorporate **user graph or social features** if app context allows (e.g., social media).
- Consider **multi-task learning** if related prediction tasks exist.
- Evaluate model performance **across user segments** (country, device type, demographics).
- Explore **ensemble models** combining simpler baselines with DNN for better calibration and interpretability.

---

### Conclusion

The video provides a comprehensive, practical framework for building a machine learning system to predict app deletion. It emphasizes thoughtful feature engineering, rigorous data handling, model calibration, and evaluation metrics aligned with business goals. The approach balances real-world constraints with scalable, maintainable design choices, offering a strong blueprint for similar predictive modeling problems in product analytics.

