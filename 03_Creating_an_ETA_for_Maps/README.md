### Summary

This video presents a detailed **machine learning system design mock interview** focused on building an **Estimated Time of Arrival (ETA) prediction system** for a Maps application. The discussion involves a candidate, Stefon, who is a research engineer with expertise in computer vision and autonomous vehicles, and an interviewer who guides the conversation through problem framing, assumptions, and design decisions.

The system aims to predict ETA for users driving between specified start and end locations worldwide, leveraging map data, historical travel data, and shortest path algorithms. The video covers the entire design workflow: data setup, model design, training, validation, and deployment, emphasizing a practical and iterative approach starting from a simple baseline.

---

### Core Concepts and System Overview

- **Inputs and Data Sources:**
  - Map data segmented into road segments, each with:
    - Distance
    - Speed limit
    - Free flow speed (typical fastest speed)
    - Priority class (e.g., major highway vs minor local road)
  - Historical travel data:
    - Number of cars observed per 2-minute interval
    - Average speed per 2-minute interval
  - Shortest path functionality to compute routes on the weighted graph of road segments.

- **Product Scope:**
  - The system is designed to work **globally**, covering all roads worldwide.
  - High-performance compute clusters and key-value stores are assumed available for low-latency processing.

---

### High-Level Design Approach

The candidate breaks down the design into five key components:

1. **Data Setup**
   - Organize map and travel data into lookup tables keyed by segment ID.
   - Map table columns: segment ID, distance, speed limit, free flow speed, priority.
   - Travel data table columns: year, 2-minute interval within year, number of cars, average speed.
   - Data cleanliness ensured by automated filtering (null, invalid values) and potential human spot checks.
   - Data extends back multiple years, enabling statistical learning.

2. **Model Interface and Parameterization**
   - Define a function taking as input:
     - Road segment ID
     - Time interval within a standard week (e.g., Tuesday 5:00 p.m. to 5:02 p.m.)
   - Output: predicted ETA for the segment at that time window.
   - The chosen V1 baseline model:
     - Learnable parameter $m$ for each (segment, time interval) pair representing mean speed.
     - ETA computed as $$ \text{ETA} = \frac{\text{distance}}{m} $$

3. **Training**
   - Aggregate historical data by converting raw travel data (year + interval) into "interval within week" via modulo operation.
   - Compute weighted averages of speeds using the number of cars as weights to produce stable mean speed estimates.
   - Pipeline involves offline batch processing to update parameters periodically, not on every user query.

4. **Validation**
   - Employ an **80/20 train/validation split**, splitting data by randomly selecting 20% of months for validation.
   - This monthly split prevents information leakage and simulates future prediction.
   - Validation involves sampling road segments and time intervals, predicting ETA using only historical training data, then comparing against the true observed travel times.
   - Primary metric: absolute error $| \text{predicted ETA} - \text{actual ETA} |$.
   - Aggregated statistics include mean, variance, and interquartile range of errors.

5. **Deployment**
   - Production model uses all available historical data to maximize accuracy.
   - Model stored in a high-performance key-value store keyed by (segment ID, interval).
   - User application queries an ETA backend:
     - ETA function returns ETAs for road segments.
     - Shortest path algorithm computes route and sums segment ETAs.
   - The deployment is straightforward due to the simple lookup-based model.

---

### Important Tables

| Table Name    | Key Columns                 | Description                                       |
|---------------|-----------------------------|-------------------------------------------------|
| Map Table     | segment ID, distance, speed limit, free flow speed, priority | Static road segment attributes                   |
| Travel Data Table | year, 2-minute interval, num cars, avg speed             | Historical travel observations per time window  |
| Aggregated Travel Table | segment ID, interval within week, weighted avg speed, total num cars | Derived table for model training and inference  |

---

### Key Insights

- **Starting with a simple baseline model** that uses historical averages by segment and weekly time intervals is practical and effective for V1.
- **Data cleanliness and format standardization** are crucial first steps to ensure reliable model training.
- The **offline batch pipeline** for data aggregation and parameter computation supports scalability and low-latency inference.
- **Validation strategy using month-based splits** reduces leakage and tests model robustness to temporal changes.
- The **absolute error metric** is a reasonable starting point but can be extended to more sophisticated metrics emphasizing outliers.
- Deployment leverages **existing map and shortest path systems**, integrating ML smoothly into the product pipeline.
- The design is **iterative**, allowing refinement of models and data pipelines after establishing a working baseline.

---

### Discussion: Strengths and Limitations

| Strengths                                  | Limitations and Areas for Improvement                     |
|--------------------------------------------|-----------------------------------------------------------|
| Clear modular breakdown of design stages   | Model is simplistic and does not adapt to recent traffic changes or anomalies |
| Thorough data management and validation    | Lack of real-time updating or anomaly detection            |
| Practical validation approach with temporal splits | No advanced time series or contextual features used        |
| Integration plan with existing shortest path and map systems | No explicit data pipeline or flow diagram presented (noted as desirable) |
| Emphasis on production readiness and scalability | Outlier handling and richer metrics not yet incorporated   |

---

### Conclusion

This interview walkthrough provides a **comprehensive framework** for designing an ETA prediction system using machine learning. It illustrates the importance of **systematic data preparation, simple but effective modeling, thorough validation, and smooth deployment integration**. Starting with a baseline statistical approach grounded in historical data enables rapid iteration and sets the foundation for future improvements such as incorporating real-time traffic, anomaly detection, and more sophisticated forecasting models.

---

### Keywords

- ETA (Estimated Time of Arrival)
- Machine Learning System Design
- Map Segments
- Historical Travel Data
- Shortest Path Algorithm
- Data Cleanliness
- Weighted Average Speed
- Train/Validation Split
- Offline Data Pipeline
- Model Deployment