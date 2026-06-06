# What Shapes How Long We Sleep?

A multiple regression study testing whether age predicts weekday sleep duration, using the CDC's National Health and Nutrition Examination Survey (NHANES). Group project from graduate coursework; my contribution was the modeling and diagnostics.

## Question

We tested a deliberately skeptical null hypothesis: that age has no effect on sleep duration. Failing to reject it would itself have been informative.

## Data

- **Source:** CDC NHANES
- **Outcome:** weekday sleep hours
- **Predictors:** age, gender, income-to-poverty ratio, weekly leisure-time physical activity (LTPA)
- **Sample:** 2,991 respondents after removing incomplete records and engineering the activity variables

## Method

Two specifications, built up and validated predictor by predictor:

- **Simple model:** `Sleep ~ Age + Gender + Poverty_Index`
- **Complex model:** the same, plus log-transformed weekly physical activity

Sleep against age was clearly non-linear, so a quadratic `Age²` term was added. Each model was then assessed against the standard regression assumptions: linearity (residual-vs-fitted), homoskedasticity (Breusch-Pagan), normality (Q-Q plots and Shapiro-Wilk), and multicollinearity (variance inflation factors).

## Key findings

- `Age` and `Age²` are both highly significant (p < 1e-14): age has a genuine, curvilinear relationship with sleep duration, which rejects the null.
- The model explains only about 2% of the variance (adjusted R² ≈ 0.022). Age is statistically significant but substantively minor.
- Gender and physical activity are not significant once the other covariates are controlled for.
- Diagnostics flag pronounced heteroskedasticity (Breusch-Pagan p < 2.2e-16) and severe `Age` / `Age²` collinearity (VIF ≈ 37), so the standard errors should be read with caution.

## Recommendations

- Use robust standard errors to address the heteroskedasticity.
- Center age before squaring to reduce the `Age` / `Age²` collinearity.
- Add interaction terms and additional NHANES variables to capture more structure.

## Files

- `sleep-age-analysis.Rmd` — analysis source
- `final_data.csv` — cleaned dataset
- `sleep-age-report.pdf` — knit report

## Tools

R: `lm`, `ggplot2`, `lmtest` (Breusch-Pagan), `car` (VIF), `stargazer`
