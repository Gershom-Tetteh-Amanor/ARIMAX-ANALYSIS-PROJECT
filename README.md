
where:
- `Y_t` = log government expenditure (USD) at time `t`
- `X_{k,t}` = exogenous variables (population, log revenue, log GDP)
- `β_k` = coefficients for exogenous variables
- `ε_t ~ WN(0, σ²)`

### Workflow

1. **Data Loading & Preprocessing** — Currency conversion (GHS → USD), log transformation
2. **Descriptive Analysis** — Trend exploration of expenditure and exogenous variables
3. **Multicollinearity Check** — Variance Inflation Factor (VIF) diagnostics
4. **Stationarity Testing** — Augmented Dickey-Fuller (ADF) test; second-order differencing
5. **Model Identification** — ACF/PACF analysis; `auto.arima()` AIC-minimising search
6. **Model Estimation** — Five specifications via Maximum Likelihood Estimation
7. **Residual Diagnostics** — Ljung-Box test for white noise residuals
8. **Forecasting** — 10-year out-of-sample projections with 80% and 95% prediction intervals
9. **Model Comparison** — Full metric-by-metric evaluation across all specifications

### Model Selection Criteria

| Criterion | Formula | Preference |
|-----------|---------|------------|
| AIC | -2log(L) + 2k | Lower is better |
| BIC | -2log(L) + klog(n) | Lower is better |
| RMSE | √(1/n Σ e_t²) | Lower is better |
| MAE | 1/n Σ |e_t| | Lower is better |
| MAPE | 100/n Σ |e_t/Y_t| | Lower is better |
| MASE | Scaled by naïve forecast MAE | < 1 is better |

---

## Data

### Source

The dataset (`Data2.xlsx`) spans **1983–2024** and is compiled from:

| Variable | Description | Unit | Source |
|----------|-------------|------|--------|
| **GE (Y)** | Total Government Expenditure (Net Lending) | Million GHS → log(USD) | Bank of Ghana — Gov. Fiscal Operations |
| **POPULATION (X1)** | Ghana's Annual Population | Persons | World Bank / Ghana Statistical Service |
| **REVENUE (X2)** | Total Government Revenue | Million GHS → log(USD) | Bank of Ghana — Gov. Fiscal Operations |
| **GDP (X3)** | Nominal GDP | USD Billion → log(USD) | World Bank National Accounts |

### Preprocessing Steps

1. **Currency conversion** — All GHS-denominated variables converted to USD using contemporaneous annual average exchange rates (to normalise against severe cedi depreciation, especially post-2014)
2. **Log transformation** — Applied to all monetary variables to stabilise variance and linearise exponential growth trends
3. **Filtering** — Rows with `Inf` values (from division by zero exchange rate in 1983) removed, yielding 38 usable observations (1984–2021 for estimation)

### Sample

- **Estimation period:** 1984–2021 (38 observations)
- **Forecast horizon:** 2022–2031 (10 years)
- **Frequency:** Annual

---

## Models Estimated

| # | Model | Specification | Exogenous Variables |
|---|-------|---------------|---------------------|
| 1 | **ARIMA Baseline** | ARIMA(0,1,0) with drift | None |
| 2 | **ARIMAX_ALL** | Regression with ARIMA(0,0,0) errors | Population + Log Revenue + Log GDP |
| 3 | **ARIMAX_POP** | Regression with ARIMA(0,0,0) errors | Population only |
| 4 | **ARIMAX_REV** | Regression with ARIMA(1,0,0) errors | Log Revenue only |
| 5 | **ARIMAX_GDP** | Regression with ARIMA(0,1,1) errors | Log GDP only |

---

## Results Summary

### Full Model Comparison

| Metric | ARIMA | ARIMAX_ALL | ARIMAX_POP | ARIMAX_REV | ARIMAX_GDP | **Best** |
|--------|-------|------------|------------|------------|------------|----------|
| **AIC** | -7.5107 | **-46.5252** | 13.4633 | -42.9495 | -21.7580 | ARIMAX_ALL |
| **BIC** | -4.2889 | **-38.3372** | 18.3760 | -38.0367 | -15.3143 | ARIMAX_ALL |
| **ME** | 0.0005 | **0.0000** | 0.0000 | -0.0016 | 0.0075 | ARIMAX_ALL |
| **RMSE** | 0.2044 | **0.1150** | 0.2669 | 0.1266 | 0.1593 | ARIMAX_ALL |
| **MAE** | 0.1684 | **0.0998** | 0.2195 | 0.1023 | 0.1270 | ARIMAX_ALL |
| **MPE** | 0.0043 | **-0.0030** | -0.0165 | -0.0189 | 0.0471 | ARIMAX_ALL |
| **MAPE** | 0.7819 | **0.4593** | 1.0088 | 0.4702 | 0.5882 | ARIMAX_ALL |
| **MASE** | 0.8428 | **0.4992** | 1.0986 | 0.5118 | 0.6357 | ARIMAX_ALL |

### Key Takeaways

- **ARIMAX_ALL is the dominant specification**, winning all 8 evaluation metrics
- Adding exogenous variables reduces RMSE by **~44%** compared to the univariate ARIMA baseline
- The revenue-only model is the strongest single-variable specification
- The population-only model performs worse than the ARIMA baseline (population alone does not capture fiscal dynamics)
- Joint estimation is feasible despite multicollinearity because `auto.arima` handles the regression + ARIMA error structure robustly

### Forecast Highlights (2022–2031)

- **ARIMA baseline** projects log expenditure rising from 23.77 (2022) to 24.80 (2031)
- **ARIMAX_ALL** projects a flat trajectory at ~21.79 (conditioned on mean exogenous projections)
- **ARIMAX_REV** projects a gently declining trajectory from 21.91 to 21.80
- **ARIMAX_GDP** projects steady growth from 22.51 to 22.99

> **Note:** ARIMAX forecasts use a conservative **mean-projection** of exogenous variables. For policy-grade forecasting, scenario-based projections (IMF WEO, UN population forecasts) are recommended.

---

## Getting Started

### Prerequisites

- **R** (≥ 4.5.0) — [Download](https://cran.r-project.org/)
- **RStudio** (recommended) — [Download](https://posit.co/download/rstudio-desktop/)

### Required R Packages

```r
install.packages(c(
  "forecast",    # Time series modelling and forecasting
  "tseries",     # ADF and KPSS unit root tests
  "car",         # VIF for multicollinearity diagnostics
  "readxl",      # Reading Excel data files
  "tidyverse",   # Data manipulation and visualisation
  "knitr",       # Dynamic report generation
  "rmarkdown"    # R Markdown rendering
))
