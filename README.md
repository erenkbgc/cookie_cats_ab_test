# A/B Testing Player Retention in Cookie Cats

This project analyzes whether a small change in game design — moving the first progression gate from level 30 to level 40 — significantly impacts player retention in the mobile puzzle game Cookie Cats.

Using a real A/B test dataset with over 90,000 players, we explore this question through a mix of statistical hypothesis testing, bootstrapped confidence intervals, and logistic regression modeling.

---

## Dataset Overview

The dataset includes anonymized data for 90,189 players randomly split into two groups:

- `gate_30`: Players encountered the gate at level 30  
- `gate_40`: Players encountered the gate at level 40

For each player, the following fields are provided:

- `userid`: Unique identifier  
- `version`: A/B test group (`gate_30` or `gate_40`)  
- `sum_gamerounds`: Number of rounds played in the first 7 days  
- `retention_1`: Boolean indicator for Day 1 retention  
- `retention_7`: Boolean indicator for Day 7 retention

---

## Objective

The key business question is:

**Does delaying the progression gate from level 30 to level 40 increase short-term or long-term player retention?**

We aim to answer this using statistical and machine learning techniques while supporting design decisions with data.

---

## Analysis Structure

1. **Exploratory Data Analysis (EDA)**  
   - Visual inspection of group balance  
   - Distribution of rounds played  
   - Outlier detection with boxplots and histograms  

2. **Outlier Handling**  
   - Capped the 99th percentile to reduce skew  
   - Applied log transformation:

     $$
     \text{gamerounds\_log} = \log(1 + \text{capped\_rounds})
     $$

3. **Hypothesis Testing: Chi-Squared**  
   - Compared Day 1 and Day 7 retention rates between groups  
   - Null hypothesis: $H_0$: No difference in retention between groups  
   - Alternative hypothesis: $H_1$: There is a difference  
   - Day 7 results showed statistical significance ($p = 0.0016$)

4. **Bootstrapping**  
   - Simulated thousands of retention rate samples  
   - Constructed 95% confidence intervals  
   - Used to validate findings from Chi-Squared test

5. **Predictive Modeling: Logistic Regression**  
   - Modeled likelihood of Day 7 return using:
     - Day 1 retention
     - Number of rounds played (log)
     - Test group assignment  
   - Evaluated with precision, recall, and F1-score  
   - Extracted and visualized feature importance

   The model predicts the probability of Day 7 retention using the logistic function:

   $$
   P(y = 1 \mid \mathbf{x}) = \frac{1}{1 + e^{-(\beta_0 + \beta_1 x_1 + \beta_2 x_2 + \dots + \beta_n x_n)}}
   $$

---

## Key Results

- Day 1 retention difference was **not** statistically significant ($p = 0.0755$)  
- Day 7 retention was **significantly higher** in the `gate_30` group ($p = 0.0016$)  
- Players who returned on Day 1 or played more rounds were more likely to stay engaged  
- Being in the `gate_40` group slightly decreased long-term retention

---

## Recommendation

**Retain the gate at level 30**, or consider placing it even earlier. The data shows that players in this group are more likely to remain engaged after 7 days.

This insight allows game designers to make informed decisions about progression pacing without negatively affecting short-term behavior.

---

## Tools Used

- Python (pandas, NumPy, seaborn, matplotlib, scikit-learn, scipy)
- Logistic regression for classification
- Bootstrap resampling
- Markdown and LaTeX for documentation clarity

---

## License & Disclaimer

This project is for educational and non-commercial purposes. All rights to game assets and data belong to the original creators of Cookie Cats.
