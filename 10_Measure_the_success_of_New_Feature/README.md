### Summary

This video presents a detailed mock data science interview focused on designing and analyzing an experiment to test a new reaction feature in a social media app’s stories. The candidate, Aova, a data scientist with experience at Amazon and Instacart, walks through the problem using a structured experimentation framework, emphasizing the importance of clear problem definition, metric selection, experiment design, hypothesis formulation, and analysis.

---

### Key Insights and Framework

- **Problem Clarification and Objective**  
  - The new reaction feature aims to **increase user engagement** with stories and the app overall.  
  - Expected outcomes include higher interaction among connections, more story creation, and enhanced user connectivity aligning with typical social media goals.

- **Feature Implementation Details**  
  - The feature appears in the existing reaction menu.  
  - Users receive a one-time notification when first opening the app after the feature rollout.  
  - No restrictions on user types, platforms, or app versions.  
  - User segmentation: differentiate between **regular users** (mutual friends/followers) and **celebrity users** (one-way followers). Focus is primarily on regular users for impact assessment.

---

### Metrics Definition

Four categories of metrics are proposed:

| Metric Category            | Description                                                                                       | Example Metrics                                           |
|---------------------------|-------------------------------------------------------------------------------------------------|-----------------------------------------------------------|
| **North Star Metric**       | Long-term, company-aligned success metric                                                       | - Daily minutes spent on app<br>- Daily active users (DAU) |
| **Primary Success Metric**  | Immediate, direct measure of feature impact                                                     | - Reaction usage rate: $$\frac{\text{Stories with reactions}}{\text{Stories viewed}}$$<br>- New reaction adoption rate relative to existing reactions |
| **Guardrail Metrics**       | Metrics to ensure no negative impact on existing features or technical stability                 | - Overall reaction rate<br>- Crash rate<br>- Bounce rate<br>- App latency |
| **Secondary Metrics**       | Additional insights for learning and optimization                                               | - Repeated usage of the new reaction<br>- Notification engagement rate (users interacting with the notification) |

Additional considerations:  
- Metrics should be analyzed both at the **system level** and **user level** (e.g., average number of users using the new reaction when viewing stories).  
- Notification engagement helps differentiate effects of the notification versus the feature itself.

---

### Experimental Design

- **Randomization Unit**  
  - Due to network effects and social connections, **randomizing at the user level violates the Stable Unit Treatment Value Assumption (SUTVA)**.  
  - Two alternative randomization approaches:  
    1. **Geographical market-level randomization** — limited by market differences, travel, and reduced sample size.  
    2. **Cluster-based randomization (preferred)** — group users by network clusters so all network-connected users share the same treatment or control status, minimizing contamination and bias.

- **Triggering Criteria**  
  - Assign users to treatment/control upon first app open or first login in their cluster.  
  - Consider triggering only when stories are available on the homepage to reduce noise, balancing engineering complexity.

- **Assignment Timing**  
  - Assignment occurs when the first user in a network cluster logs in; subsequent users in that cluster inherit the same assignment to maintain consistency.

---

### Hypotheses

| Hypothesis Type       | Statement                                                                                     |
|----------------------|------------------------------------------------------------------------------------------------|
| **Null Hypothesis**    | The new reaction feature has **no effect** on the primary success metrics (reaction usage rate, story creation). |
| **Alternative Hypothesis** | The new reaction feature **increases user engagement**, improving primary success metrics without degrading guardrail metrics or North Star metrics. |

---

### Statistical Testing and Power Analysis

- **Statistical Test Selection**  
  - Use a **Z-test** for proportion metrics (e.g., reaction usage rate) and a **T-test** for continuous metrics (e.g., number of stories created).  
  - Z-test and T-test are similar with large samples (thousands of observations).  
  - Because of cluster randomization, the effective sample size is smaller, requiring techniques such as **clustering standard errors** for variance reduction.

- **Sample Size and Duration**  
  - Power analysis inputs:  
    - **Effect size (Minimum Detectable Effect, MDE):** Needs practical significance, informed by historical data and stakeholder input.  
    - **Power:** Typically 0.8 (80% probability to detect a true effect).  
    - **Significance level (Alpha):** Usually 0.05 (5% false positive rate).  
    - **Variance:** Estimated from historical data, influenced by metrics’ natural variability.  

- **Duration Considerations:**  
  - Account for **seasonality** (weekday vs. weekend behavior).  
  - Account for **app adoption delays** if the feature requires app updates.

---

### Analysis Approach

- Start by analyzing **average treatment effects** on all key metrics.  
- Explore **user segments** to understand heterogeneity in responses:  
  - Regular vs. celebrity users  
  - New vs. tenured users  
  - Frequent vs. infrequent users  
- Consider trade-offs and involve stakeholders in deciding next steps:  
  - Rollout, follow-up experiments, or feature tweaking (e.g., improve notification awareness).

---

### Handling Novelty Effects

- Use a **holdback group** (1-5% of users) who remain in control even after rollout.  
- Monitor long-term engagement differences between holdback and treatment groups to assess whether increased usage persists beyond initial novelty.

---

### Qualitative Analysis

- Complement quantitative data with **user research studies** to understand behavioral differences.  
- Example: Conduct UX interviews with users exhibiting high repeated usage to uncover motivations and pain points.

---

### Summary of Structured Approach

- The candidate demonstrated a **structured, comprehensive framework** for defining success metrics, designing experiments, formulating hypotheses, and analyzing results.  
- The approach balances **quantitative rigor** with practical considerations like engineering constraints and user experience.  
- Emphasizes the importance of **cluster-based randomization** due to social network effects.  
- Highlights the need for **multi-dimensional metrics** (engagement, guardrails, adoption).  
- Includes a plan for **longitudinal analysis** and **qualitative feedback** to complement initial experimental findings.

---

### Final Notes

- The interviewer praised the candidate’s clarity, structured thought process, and comprehensive metric coverage.  
- Suggested improvements include clearer early explanations of randomization timing and metric aggregation levels.  
- Overall, the interview demonstrates a realistic data science workflow for product experimentation in social media environments.

---

This video serves as an excellent case study on applying rigorous data science principles to product feature testing, especially under complex social network influences.