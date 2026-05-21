### Summary: Designing a Ranking Model for Instagram Feed

This video presents a detailed discussion on designing a **machine learning-based ranking model** for Instagram's feed, focusing on suggested posts from non-followed content creators. The main goal is to increase **user engagement** on the platform by ranking posts that users are likely to interact with (view, like, comment). The discussion covers both **functional and non-functional requirements**, **data and feature engineering**, **model architecture**, and **evaluation methods**.

---

### Key Concepts and Functional Requirements

- **Objective**: Design a ranking system that improves **individual user engagement** (views, likes, comments) with Instagram posts, primarily for suggested content from non-connected users.
- **Business Goal**: Increase metrics like **Daily Active Users (DAU)** and session length indirectly by optimizing personalized engagement.
- **Machine Learning Objective**: Optimize an **individual-level engagement probability**, which correlates with broader business KPIs but is not optimized directly.
- **Engagement Types**: Viewing content, liking, commenting, sharing.

---

### Non-Functional Requirements

- **Scalability**: The system must handle a large user base (estimated around **500 million DAUs**).
- **Availability**: High uptime and responsiveness are critical.
- **ML Ops Tooling**: Includes monitoring, alerting, debugging, and feature coverage validation.
- **Exclusions**: Monetization or ad placements are outside the current scope.
- **Analytics**: Tracking popular content creators, geographic trends, and content popularity for administrative insights.

---

### System Pipeline Overview

The ranking system is designed as a **three-phase pipeline**:

| Phase               | Description                                                                                  |
|---------------------|----------------------------------------------------------------------------------------------|
| Candidate Generation | Generate a large set (e.g., 1,000) of possible posts relevant to the user using embeddings.  |
| Ranking             | Score and order candidates based on predicted engagement likelihood.                         |
| Post-processing     | Re-rank or reshuffle posts to ensure fairness, diversity, freshness, and other business rules.|

- **Post-processing** does not add new candidates but adjusts rankings to improve content diversity or fairness.

---

### Data and Feature Engineering

- **Feature Categories**:
  - **Viewer Features**: Demographics, aggregated historical engagement (e.g., likes in past 7 days), delayed features showing interaction trends over time.
  - **Post Features**: Creator features (followers, engagement), content embeddings (video, audio, text), historical engagement metrics.
  - **Interaction History**: Viewer’s interactions with posts and social network connections.
  - **Labels**: Binary engagement indicators (1 for engagement, 0 for no engagement), with delayed feedback due to lag in user reactions.

- **Embeddings**:
  - Derived from **pre-trained models** for video, audio, and text content.
  - Embeddings are dense vectors (commonly of size ~786 floating-point numbers).
  - Can also be learned end-to-end within the ranking model architecture.

- **Data Tuple**: For each user-post pair, data is represented as a tuple $(X, A, R, P)$:
  - $X$: Feature vector
  - $A$: Action taken (recommended post)
  - $R$: Reward (engagement label)
  - $P$: Probability assigned by the model during serving (used in training and logging)

---

### Model Architectures for Ranking

Two main approaches are discussed:

| Approach                  | Description                                                                                   | Pros/Cons                                  |
|---------------------------|-----------------------------------------------------------------------------------------------|--------------------------------------------|
| Collaborative Filtering   | Factorizes the user-post interaction matrix into low-dimensional embeddings for users ($U$) and posts ($V$). | Good conceptually but struggles with scalability and sparsity in large datasets. |
| Two-Tower Neural Network  | Independent neural nets ("towers") encode user and post features into embeddings; interaction modeled by dot product followed by sigmoid activation. | Preferred contemporary method; scalable; naturally incorporates rich features. |

**Two-Tower Network Details**:

- Input: Sparse/dense features for user and post.
- Output: $d$-dimensional embedding vectors for user and post.
- Interaction score: $P = \sigma(\mathbf{u} \cdot \mathbf{v})$, where $\mathbf{u}, \mathbf{v} \in \mathbb{R}^d$ and $\sigma$ is the sigmoid function.
- Loss: Binary cross-entropy comparing predicted $P$ to true engagement labels.
- Training: Optimized via stochastic gradient descent or Adam optimizer.

---

### Training and Data Preparation

- **Positive Samples**: User interactions with posts where engagement occurred.
- **Negative Sampling**: Critical to sample non-engaged posts that were actually shown to the user to avoid bias.
- **Evaluation Metrics**:
  - **Area Under ROC Curve (AUC)**: A primary metric to evaluate classification quality of engagement prediction.
  - Typical target AUC ranges: **60% to 80%**, with >50% being better than random.
- **Validation**: Separate datasets for unbiased model evaluation before deployment.

---

### Serving & Candidate Selection

- For a given user, embed their features via the user tower.
- Use **Approximate Nearest Neighbor (ANN)** search in the embedding space to select top candidate posts.
- Score candidates with the ranking model and sort by predicted engagement probability.
- Apply post-processing rules for fairness/diversity before final display.

---

### Evaluation and Deployment

- **A/B Testing**: Deploy the new ranking model to a subset of users; compare engagement and DAU metrics against control group using the existing model.
- **Safeguard Metrics**: Monitor for unintended effects like increased user reports or blocks.
- **Continuous Training**: Incorporate online learning to handle non-stationarity in user behavior over time *Not specified in detail*.
- **Cold Start Strategy**: For new users, recommend popular posts to gather initial engagement data.

---

### Summary of Key Insights

- **Aligning ML Objectives with Business Goals**: Optimize individual engagement as a proxy for platform-wide DAU improvements.
- **Pipeline Design**: Candidate generation, ranking, and post-processing are distinct but interlinked phases.
- **Feature Engineering**: Time-aggregated and delayed features capture user behavior trends effectively.
- **Two-Tower Neural Network**: Offers scalable, expressive embeddings for both users and posts enabling effective ranking.
- **Negative Sampling & Labeling**: Crucial to avoid bias and ensure model learns meaningful engagement signals.
- **Robust Evaluation**: Use AUC and A/B testing along with safeguard metrics for real-world validation.
- **Scalability and MLOps**: Monitoring, alerting, and tooling are essential for production readiness.

---

This comprehensive approach to Instagram feed ranking balances **machine learning rigor**, **business alignment**, and **system engineering considerations** to deliver a scalable, effective personalized content recommendation system.