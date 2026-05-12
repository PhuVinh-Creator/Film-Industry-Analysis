# Film-Industry-Analysis
Lights, Camera, Regression

## Dataset Download ⚠️

This project uses the TMDB Movies Dataset from Kaggle:

**[TMDB Movies Dataset 2023: 930K+ Movies (Kaggle)](https://www.kaggle.com/datasets/asaniczka/tmdb-movies-dataset-2023-930k-movies)**

We cleaned the dataset in Excel, but the file is too large to upload here. The presentation file "Lights, Camera, Regression.pdf" covers all the preprocessing steps for reference.

---

## Overview 📋

The film industry is a high-risk, high-reward business — a major studio film costs around $65 million on average, rising to nearly $100 million with marketing. This project applies OLS multiple linear regression to a curated sample of 215 English-language films released between 2015 and 2023 to quantitatively determine which factors drive box office revenue.

**Research Question:** Can a movie's revenue be predicted using factors such as production budget, runtime, popularity, language, vote metrics, and genre?

**Hypothesis:**
- **H₀ (Null):** There is no significant relationship between the selected predictors and movie revenue.
- **H₁ (Alternative):** At least one of the selected predictors has a significant effect on movie revenue.

---

## Data & Sample 🗃️

**Source:** TMDB Movies Dataset (Kaggle)

**Filters applied during cleaning:**
- Movies released between 2015 and 2023
- English-language films only
- Major production companies with ≥ 10 movies in the dataset
- Removed rows with missing or zero values for `budget` and `revenue`

**Final Sample:** 215 films from major studios

**Engineered Variables:**
- `profit` = revenue − budget
- `ROI` = profit / budget

| Variable | Role | Description |
|---|---|---|
| `revenue` | Response | US box office revenue (USD) |
| `budget` | Explanatory | Production budget (USD) |
| `vote_count` | Explanatory | Number of audience votes on TMDB |
| `vote_average` | Explanatory | Average audience rating (1–10) |
| `runtime` | Explanatory | Film length in minutes |
| `popularity` | Explanatory | TMDB popularity score |
| `genres` | Explanatory | Genre classification (dummies) |
| `production_companies` | Explanatory | Studio name (dummies) |

---

## Summary Statistics 📊

| Metric | Budget | Revenue | Profit | Runtime | Popularity |
|---|---|---|---|---|---|
| Mean | $69.1M | $271.2M | $202.8M | 111 min | 82.7 |
| Median | $30.0M | $78.9M | $46.7M | 109 min | 27.6 |
| Std Dev | $84.0M | $420.6M | $359.1M | 21 min | 263.0 |
| Min | $0 | $4,997 | −$171.1M | 40 min | 0.6 |
| Max | $365M | $2.8B | $2.44B | 189 min | 2,994.4 |

---

## Methods & Analysis 🔬

**Why Linear Regression?**
Revenue is a continuous numeric outcome. The goal is to understand how much each factor contributes and which levers have the strongest effect on box office performance — making OLS multiple linear regression the most appropriate tool.

**Pipeline:**
1. Data cleaning and type conversion in Python (Jupyter Notebook)
2. Feature engineering (profit, ROI, one-hot encoding for genres and studios)
3. Exploratory Data Analysis (distributions, genre comparisons, studio rankings, scatter plots)
4. Correlation matrix to identify multicollinearity
5. Full OLS model with all predictors (R² = 0.807)
6. Refined OLS model keeping only statistically significant predictors (Adj. R² = 0.79)

---

## Key EDA Findings 🔍

**Genre by Average Revenue:** Adventure leads at over $1.2B average, followed by Family at ~$650M. Drama, Comedy, and Documentary trail far behind.

**Top Studios by Average Budget:** Marvel Studios leads, followed by Walt Disney Pictures, Paramount, Warner Bros., and Columbia Pictures.

**Top Studios by Average Revenue:** Marvel Studios averages ~$1.1B per film, with Walt Disney Pictures second at ~$640M.

**Scatter Plots:**
- Budget vs. Revenue: slope = 3.88, R² = 0.60
- Vote Count vs. Revenue: slope = 61,370, R² = 0.64

**Correlation Matrix Highlights:**
- `vote_count` → revenue: **0.80** (strongest)
- `budget` → revenue: **0.78**
- `runtime` → revenue: **0.53**
- `vote_average` → revenue: **0.05** (weak)
- `popularity` → revenue: **0.10** (weak)

---

## Regression Results 📈

**Final Model Equation:**

```
Revenue = −$50,066,600.07
        + $2.15 × Budget
        + $36,222.74 × Vote_Count
        + $363,232,627.30 × Genres_Adventure
        + $260,200,301.27 × Genres_Family
        − $141,176,280.09 × Production_Companies_Walt Disney Pictures
```

| Predictor | Coefficient | P-value |
|---|---|---|
| Intercept | −$50,066,600.07 | 0.006 |
| `vote_count` | +$36,222.74 | 0.000 |
| `budget` | +$2.15 | 0.000 |
| `genres_Adventure` | +$363,232,627.30 | 0.000 |
| `genres_Family` | +$260,200,301.27 | 0.001 |
| `production_companies_Walt Disney Pictures` | −$141,176,280.09 | 0.014 |

**Model Performance:**
- Full model R² = **0.807**
- Refined Adjusted R² = **0.79**
- Observations: **213**

---

## Key Findings & Takeaways 💡

**Budget drives quality and marketing reach.** Each additional dollar invested in production yields ~$2.15 in revenue on average.

**Audience engagement (vote count) is a stronger revenue signal than ratings.** Each additional vote translates to ~$36,222 in revenue — hype and visibility matter more than critical score.

**Genre is a powerful predictor.** Adventure films add ~$363M and Family films add ~$260M to expected revenue compared to baseline genres, reflecting their broad global appeal.

**Studio brand alone adds no revenue premium.** Walt Disney Pictures is actually associated with a negative coefficient (−$141M) relative to the baseline studio, suggesting that studio prestige does not independently inflate revenue once budget and genre are controlled for.

**Business implications:**
- Use predictive analytics to plan budgets and genre strategy before greenlighting projects.
- Track engagement metrics (vote count / audience reach) rather than relying on ratings alone.
- Apply risk modeling and portfolio diversification across film types to manage the high-risk nature of studio investments.

---

## Limitations ⚠️

- **Correlation, not causation:** The model identifies associations but cannot establish causal relationships.
- **Missing variables:** Star power, franchise status, marketing spend, and release timing are not included and likely explain a portion of the remaining variance.
- **US revenue only:** International box office and streaming revenues are excluded.
- **Selection bias:** The dataset is user-curated on TMDB, which may over-represent well-known films.

**Future research directions:**
- Include international and streaming revenue data
- Test machine learning models (Random Forest, XGBoost) for better outlier control
- Apply time-series analysis to capture trends across release years

---

## Technologies Used 💻

- **Python** — primary language
- **pandas** — data cleaning and manipulation
- **statsmodels (OLS)** — regression modeling
- **matplotlib / seaborn** — data visualization
- **Jupyter Notebook** — interactive analysis environment
- **Excel** — summary statistics and supplementary regression output

---

## References 📚

- Asaniczka. (2023). *TMDB movies dataset [2023]: 930k+ movies* [Data set]. Kaggle. https://www.kaggle.com/datasets/asaniczka/tmdb-movies-dataset-2023-930k-movies
- Sharpe, N. D., De Veaux, R. D., & Velleman, P. F. (2024). *Business statistics* (4th ed.). Pearson.
- Investopedia. (2024). Why movies cost so much to make. https://www.investopedia.com/financial-edge/0611/why-movies-cost-so-much-to-make.aspx
- Vogel, H. L. (2020). *Entertainment industry economics* (10th ed.). Cambridge University Press.
- McKenzie, J. (2012). The economics of movies: A literature survey. *Journal of Economic Surveys, 26*(1), 42–43.

---

## Team 👥

| Member | Institution |
|---|---|
| Bharati Sahani | Temple University |
| Steven Nguyen | Temple University |
| Prince Nkomo | Temple University |
| Enestina Raradza | Temple University |
| Sheena Huang | Temple University |
