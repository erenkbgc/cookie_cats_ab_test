# A/B Testing Player Retention in Cookie Cats

This project analyzes whether a small change in game design — moving the first progression gate from level 30 to level 40 — significantly impacts player retention in the mobile puzzle game Cookie Cats.

Using a real A/B test dataset with over 90,000 players, we explore this question through a mix of statistical hypothesis testing, bootstrapped confidence intervals, and logistic regression modeling.

---

## Dataset Overview

The dataset used in this project is publicly available on Kaggle:

**[Mobile Games A/B Testing - Cookie Cats Dataset](https://www.kaggle.com/datasets/mursideyarkin/mobile-games-ab-testing-cookie-cats/data)**

It includes anonymized data for 90,189 players randomly split into two groups:

- `gate_30`: Players encountered the gate at level 30  
- `gate_40`: Players encountered the gate at level 40

Each record contains the following fields:

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
   - Group balance and retention comparison  
   - Round distribution and outlier detection  

2. **Outlier Handling**  
   - Applied a 99th percentile cap  
   - Created a log-transformed feature:  
    `gamerounds_log = log(1 + capped_rounds)`

3. **Hypothesis Testing: Chi-Squared**  
   - $H_0$: No difference in retention between groups  
   - $H_1$: A difference exists  
   - Day 7 p-value: $p = 0.0016$ (statistically significant)

4. **Bootstrapping**  
   - Created empirical confidence intervals for retention rates  
   - Confirmed statistical findings through simulation

5. **Predictive Modeling: Logistic Regression**  
   - Features: `retention_1`, `gamerounds_log`, `version_bin`  
   - Target: `retention_7`  
   - Model:  
     $P(y = 1 \mid \mathbf{x}) = \frac{1}{1 + e^{-(\beta_0 + \beta_1 x_1 + \beta_2 x_2 + \dots + \beta_n x_n)}}$  
   - Evaluation: Precision, recall, F1-score  
   - Feature importance visualized

---

## Key Results

- Day 1 retention: no statistically significant difference ($p = 0.0755$)  
- Day 7 retention: significantly higher in `gate_30` group ($p = 0.0016$)  
- Early engagement (Day 1 return and game rounds) strongly predicts long-term retention  
- `gate_40` assignment slightly reduced retention probability

---

## Recommendation

**Keep the gate at level 30** — or consider testing even earlier placements.

Players shown the gate earlier tend to return more reliably after 7 days, without harming short-term engagement. This supports a game design strategy that optimizes long-term retention through carefully timed progression blocks.

---

## Tools Used

- Python (pandas, NumPy, seaborn, matplotlib, scikit-learn, scipy)
- Logistic regression
- Bootstrap resampling
- Inline LaTeX for documentation

---

## License & Dataset Credit

Dataset by **Mürşide Yarkın**, hosted on Kaggle:  
[https://www.kaggle.com/datasets/mursideyarkin/mobile-games-ab-testing-cookie-cats](https://www.kaggle.com/datasets/mursideyarkin/mobile-games-ab-testing-cookie-cats)

This project is for educational purposes. All game content is owned by the original developers of Cookie Cats.
