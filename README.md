# Forecasting Government Expenditure in Ghana Using ARIMAX

> **Author:** Gershon Tetteh Amanor
> **Date:** April 02, 2026
> **Connect:** [GitHub](https://github.com/your-username) | [LinkedIn](https://www.linkedin.com/in/your-profile)

---

## 📜 Abstract

This study develops and evaluates an Autoregressive Integrated Moving Average with Exogenous Variables (ARIMAX) model for forecasting Ghana's government expenditure over the period 1984–2024. The dependent variable is log-transformed total government expenditure (USD equivalent), and three exogenous variables are examined — population, log government revenue, and log GDP — both jointly and in isolation.

The ARIMAX framework is compared against a univariate ARIMA baseline using information criteria (AIC, BIC) and standard accuracy metrics (RMSE, MAE, MAPE, MASE). Results demonstrate that incorporating macroeconomic exogenous variables into the time series framework meaningfully improves forecasting performance. Policy implications for Ghana's medium-term fiscal planning are discussed.

---

## 📂 Repository Contents

| File | Description |
|------|-------------|
| `ARIMAX_GHANA_FULL_THESIS.Rmd` | Main R Markdown file containing all analysis, from data preprocessing to forecasting |
| `ARIMAX_GHANA_FULL_THESIS.pdf` | Compiled PDF version of the thesis report with all chapters, figures, and analysis |
| `ARIMAX_GHANA_FULL_THESIS.html` | HTML version of the thesis report |
| `Data2.xlsx` | Raw dataset with annual time-series data for Ghana (1983–2024), including expenditure, population, GDP, revenue, and exchange rates |

---

## ⚙️ Methodology

The study employs a quantitative, predictive time-series design based on the Box-Jenkins methodology.

### 1. Data Preprocessing
- **Currency Conversion**: Monetary variables (Expenditure, Revenue) converted from Ghana Cedis (GHS) to US Dollars (USD) using contemporaneous annual exchange rates to normalize against severe cedi depreciation.
- **Log Transformation**: All monetary variables log-transformed to stabilize variance and linearize exponential growth trends.

### 2. Stationarity
The Augmented Dickey-Fuller (ADF) test was used to check for stationarity. The log expenditure series was found non-stationary at level and differenced twice (d=2) to achieve stationarity.

### 3. Model Building
- Univariate **ARIMA** model fitted as baseline
- Multiple **ARIMAX** models fitted with exogenous variables: **Population**, **Log(Government Revenue)**, and **Log(GDP)**

### 4. Model Evaluation
- **Information Criteria**: AIC and BIC for model fit comparison
- **Forecast Accuracy**: RMSE, MAE, MAPE, MASE for in-sample performance
- **Diagnostics**: Residuals analyzed via ACF/PACF plots and Ljung-Box test

---

## 🔑 Key Findings

- **Best Performing Model**: ARIMAX with **all three exogenous variables (Population, Log Revenue, Log GDP)** consistently outperformed the univariate ARIMA baseline across all major accuracy metrics (AIC, BIC, RMSE, MAE, MAPE, MASE).
- **Improved Forecasting**: Incorporating macroeconomic factors like revenue and GDP significantly improves forecasting accuracy, confirming that Ghana's expenditure is influenced by the broader economic environment.
- **Multicollinearity**: Significant multicollinearity identified among exogenous variables; individual ARIMAX models fitted to isolate each variable's effect.
- **Projected Growth**: 10-year forecast projects continued steady increase in government expenditure, reflecting demographic pressures and economic growth assumptions.
- **Policy Implications**: Provides a data-driven framework for the Ministry of Finance to generate reliable fiscal forecasts and better budget planning.

---

## 🛠️ Technologies & Packages

**Language:** R

**Key Packages:**
- `forecast` — Time series modelling and forecasting
- `tseries` — Augmented Dickey-Fuller and KPSS unit root tests
- `car` — Variance Inflation Factor diagnostics
- `readxl` — Reading Excel data files
- `tidyverse` — Data manipulation and visualization
- `knitr` / `rmarkdown` — Report generation

---

## 🚀 How to Reproduce

1. **Clone the repository:**
   ```bash
   git clone https://github.com/your-username/your-repo-name.git
