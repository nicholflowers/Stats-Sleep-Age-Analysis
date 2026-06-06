# 😴 What Shapes How Long We Sleep?

**Does age actually shape how long we sleep? A regression study on nearly 3,000 CDC survey respondents.**

## 📌 Overview

Sleep is among the strongest predictors of long-term health, yet what shapes how much of it people get is poorly understood. This project asks a focused version of that question: Does age affect how long people sleep? It draws on weekday sleep data from the CDC's National Health and Nutrition Examination Survey (NHANES).

## 🎯 Project Goals

- Test whether age has a measurable effect on sleep duration
- Compare a simple demographic model against a fuller one that adds physical activity
- Check the relationship for non-linearity rather than assuming a straight line
- Validate every regression assumption instead of trusting the coefficients alone
- Report the outcome honestly, including where the model falls short

## 🗂️ Dataset

| Source | Description |
| --- | --- |
| [CDC NHANES, 2021–2023 cycle](https://wwwn.cdc.gov/nchs/nhanes/continuousnhanes/default.aspx?Cycle=2021-2023) | Self-reported weekday sleep hours alongside demographic, socioeconomic, and physical-activity measures from a large U.S. health survey (collected August 2021 to August 2023). |

- **Outcome:** weekday sleep hours
- **Predictors:** age, gender, income-to-poverty ratio, weekly leisure-time physical activity (LTPA)
- **Sample:** 2,991 respondents after removing incomplete records and engineering the activity variables

## 🚀 Implementation Details

We approached the question with a **null hypothesis**: we assumed age has *no* effect at all and required the data to provide strong evidence before accepting otherwise. Starting from "nothing is going on" is a deliberately conservative stance that guards against mistaking random noise for a real pattern.

### 🔹 Data Preparation

- Coded gender and the income-to-poverty ratio as binary variables
- Engineered weekly totals for moderate and vigorous leisure-time activity
- Log-transformed the highly skewed activity measure
- Dropped records with missing values, leaving 2,991 respondents

### 🔹 Model Specification

- **Simple model:** `Sleep ~ Age + Gender + Poverty_Index`
- **Complex model:** the same, plus log-transformed weekly physical activity
- Added a quadratic `Age²` term once the age-to-sleep relationship proved non-linear

### 🔹 Diagnostic Testing

- **Linearity:** residual-vs-fitted plots
- **Homoskedasticity:** Breusch-Pagan test
- **Normality:** Q-Q plots and Shapiro-Wilk
- **Multicollinearity:** variance inflation factors (VIF)

## 🏆 Results

A genuine but modest finding:

- `Age` and `Age²` are both highly significant (p < 1e-14): age has a curvilinear relationship with sleep duration, overturning the starting assumption of no effect.
- The model explains only about 2% of the variance (adjusted R² ≈ 0.022). Age is statistically significant but substantively minor.
- Gender and physical activity are not significant once the other covariates are controlled for.
- Diagnostics flag pronounced heteroskedasticity (Breusch-Pagan p < 2.2e-16) and severe `Age` / `Age²` collinearity (VIF ≈ 37), so the standard errors should be read with caution.

## 🧭 Recommendations

- Use robust standard errors to address the heteroskedasticity
- Center age before squaring to reduce the `Age` / `Age²` collinearity
- Add interaction terms and additional NHANES variables to capture more structure

## 🧠 Skills Demonstrated

- **Regression modeling:** specifying, estimating, and comparing nested linear models, from a parsimonious baseline to a multivariable specification
- **Functional form:** identifying a non-linear relationship and capturing it through a quadratic term and a log transformation of a skewed predictor
- **Diagnostic evaluation:** formally testing the core OLS assumptions of homoskedasticity, normality of residuals, and absence of multicollinearity (Breusch-Pagan, Shapiro-Wilk, variance inflation factors)
- **Statistical interpretation:** distinguishing statistical significance from explanatory power, and reporting a weak-but-significant result honestly, with its limitations made explicit

## 🧰 Tools

Includes but is not limited to R, `ggplot2`, `lmtest`, `car`, and `stargazer`.
