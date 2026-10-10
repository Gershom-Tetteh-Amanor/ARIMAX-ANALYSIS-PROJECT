# Forecasting Government Expenditure in Ghana Using ARIMAX

A time-series econometric study applying ARIMAX modelling to forecast Ghana's government expenditure (1984-2024) using population, government revenue, and GDP as exogenous variables.

---

## Abstract

This study develops and evaluates an Autoregressive Integrated Moving Average with Exogenous Variables (ARIMAX) model for forecasting Ghana's government expenditure over the period 1984-2024. The dependent variable is log-transformed total government expenditure (USD equivalent), and three exogenous variables are examined - population, log government revenue, and log GDP - both jointly and in isolation. The ARIMAX framework is compared against a univariate ARIMA baseline using information criteria (AIC, BIC) and standard accuracy metrics (RMSE, MAE, MAPE, MASE). Results demonstrate that incorporating macroeconomic exogenous variables into the time series framework meaningfully improves forecasting performance. Policy implications for Ghana's medium-term fiscal planning are discussed.

---

## Key Findings

| Finding | Detail |
|---------|--------|
| Integration Order | Government expenditure is non-stationary at level and integrated of order I(2) - second-order differencing required |
| Multicollinearity | Severe multicollinearity among exogenous variables (VIF > 15 for all three), motivating individual ARIMAX specifications |
| Best Model | ARIMAX_ALL (population + revenue + GDP jointly) won 8 out of 8 evaluation metrics |
| Improvement over ARIMA | RMSE reduced from 0.2044 to 0.1150; MAE from 0.1684 to 0.0998; MAPE from 0.7819 to 0.4593 |
| Forecast Horizon | 10-year out-of-sample projection (2022-2031) using mean-projection of exogenous variables |
| Theoretical Support | Keynesian fiscal theory, Wagner's Law, Peacock-Wiseman displacement hypothesis, and commodity-price fiscal linkage |

---

## Repository Structure

```
ARIMAX-Ghana-Government-Expenditure/
│
├── ARIMAX_GHANA_FULL_THESIS.Rmd     # Main R Markdown source (full thesis)
├── ARIMAX_GHANA_FULL_THESIS.html    # Compiled HTML thesis output
├── ARIMAX_Ghana_Thesis_final.pdf    # Compiled PDF thesis output
├── Data2.xlsx                        # Raw annual fiscal dataset (1983-2024)
├── README.md                         # This file
├── LICENSE                           # MIT License
│
└── figures/                          # Generated figures (from Rmd)
    ├── fig01_expenditure_trend.png
    ├── fig02_exogenous_variables.png
    ├── fig03_stationarity.png
    ├── fig04_acf_pacf_level.png
    ├── fig05_acf_pacf_diff.png
    ├── fig06_arima_residuals.png
    ├── fig07_arima_forecast.png
    ├── fig08_arimax_all_residuals.png
    ├── fig09_arimax_all_forecast.png
    ├── fig10_arimax_pop_residuals.png
    ├── fig11_arimax_pop_forecast.png
    ├── fig12_arimax_rev_residuals.png
    ├── fig13_arimax_rev_forecast.png
    ├── fig14_arimax_gdp_residuals.png
    └── fig15_arimax_gdp_forecast.png
```

---

## Methodology

### Analytical Framework

The ARIMAX(p, d, q) model extends the Box-Jenkins ARIMA framework with exogenous regressors:

```
Y_t = μ + Σ(φ_i * Y_{t-i}) + Σ(θ_j * ε_{t-j}) + Σ(β_k * X_{k,t}) + ε_t
```

where:
- Y_t = log government expenditure (USD) at time t
- X_{k,t} = exogenous variables (population, log revenue, log GDP)
- β_k = coefficients for exogenous variables
- ε_t ~ WN(0, σ²)

### Workflow

1. Data Loading and Preprocessing - Currency conversion (GHS to USD), log transformation
2. Descriptive Analysis - Trend exploration of expenditure and exogenous variables
3. Multicollinearity Check - Variance Inflation Factor (VIF) diagnostics
4. Stationarity Testing - Augmented Dickey-Fuller (ADF) test; second-order differencing
5. Model Identification - ACF/PACF analysis; auto.arima() AIC-minimising search
6. Model Estimation - Five specifications via Maximum Likelihood Estimation
7. Residual Diagnostics - Ljung-Box test for white noise residuals
8. Forecasting - 10-year out-of-sample projections with 80% and 95% prediction intervals
9. Model Comparison - Full metric-by-metric evaluation across all specifications

### Model Selection Criteria

| Criterion | Formula | Preference |
|-----------|---------|------------|
| AIC | -2log(L) + 2k | Lower is better |
| BIC | -2log(L) + klog(n) | Lower is better |
| RMSE | Square root of (1/n * sum of e_t squared) | Lower is better |
| MAE | 1/n * sum of absolute e_t | Lower is better |
| MAPE | 100/n * sum of absolute (e_t / Y_t) | Lower is better |
| MASE | Scaled by naive forecast MAE | Less than 1 is better |

---

## Data

### Source

The dataset (Data2.xlsx) spans 1983-2024 and is compiled from:

| Variable | Description | Unit | Source |
|----------|-------------|------|--------|
| GE (Y) | Total Government Expenditure (Net Lending) | Million GHS to log(USD) | Bank of Ghana - Gov. Fiscal Operations |
| POPULATION (X1) | Ghana's Annual Population | Persons | World Bank / Ghana Statistical Service |
| REVENUE (X2) | Total Government Revenue | Million GHS to log(USD) | Bank of Ghana - Gov. Fiscal Operations |
| GDP (X3) | Nominal GDP | USD Billion to log(USD) | World Bank National Accounts |

### Preprocessing Steps

1. Currency conversion - All GHS-denominated variables converted to USD using contemporaneous annual average exchange rates (to normalise against severe cedi depreciation, especially post-2014)
2. Log transformation - Applied to all monetary variables to stabilise variance and linearise exponential growth trends
3. Filtering - Rows with Inf values (from division by zero exchange rate in 1983) removed, yielding 38 usable observations (1984-2021 for estimation)

### Sample

- Estimation period: 1984-2021 (38 observations)
- Forecast horizon: 2022-2031 (10 years)
- Frequency: Annual

---

## Models Estimated

| Number | Model | Specification | Exogenous Variables |
|--------|-------|---------------|---------------------|
| 1 | ARIMA Baseline | ARIMA(0,1,0) with drift | None |
| 2 | ARIMAX_ALL | Regression with ARIMA(0,0,0) errors | Population + Log Revenue + Log GDP |
| 3 | ARIMAX_POP | Regression with ARIMA(0,0,0) errors | Population only |
| 4 | ARIMAX_REV | Regression with ARIMA(1,0,0) errors | Log Revenue only |
| 5 | ARIMAX_GDP | Regression with ARIMA(0,1,1) errors | Log GDP only |

---

## Results Summary

### Full Model Comparison

| Metric | ARIMA | ARIMAX_ALL | ARIMAX_POP | ARIMAX_REV | ARIMAX_GDP | Best |
|--------|-------|------------|------------|------------|------------|------|
| AIC | -7.5107 | -46.5252 | 13.4633 | -42.9495 | -21.7580 | ARIMAX_ALL |
| BIC | -4.2889 | -38.3372 | 18.3760 | -38.0367 | -15.3143 | ARIMAX_ALL |
| ME | 0.0005 | 0.0000 | 0.0000 | -0.0016 | 0.0075 | ARIMAX_ALL |
| RMSE | 0.2044 | 0.1150 | 0.2669 | 0.1266 | 0.1593 | ARIMAX_ALL |
| MAE | 0.1684 | 0.0998 | 0.2195 | 0.1023 | 0.1270 | ARIMAX_ALL |
| MPE | 0.0043 | -0.0030 | -0.0165 | -0.0189 | 0.0471 | ARIMAX_ALL |
| MAPE | 0.7819 | 0.4593 | 1.0088 | 0.4702 | 0.5882 | ARIMAX_ALL |
| MASE | 0.8428 | 0.4992 | 1.0986 | 0.5118 | 0.6357 | ARIMAX_ALL |

### Key Takeaways

- ARIMAX_ALL is the dominant specification, winning all 8 evaluation metrics
- Adding exogenous variables reduces RMSE by approximately 44% compared to the univariate ARIMA baseline
- The revenue-only model is the strongest single-variable specification
- The population-only model performs worse than the ARIMA baseline (population alone does not capture fiscal dynamics)
- Joint estimation is feasible despite multicollinearity because auto.arima handles the regression plus ARIMA error structure robustly

### Forecast Highlights (2022-2031)

- ARIMA baseline projects log expenditure rising from 23.77 (2022) to 24.80 (2031)
- ARIMAX_ALL projects a flat trajectory at approximately 21.79 (conditioned on mean exogenous projections)
- ARIMAX_REV projects a gently declining trajectory from 21.91 to 21.80
- ARIMAX_GDP projects steady growth from 22.51 to 22.99

Note: ARIMAX forecasts use a conservative mean-projection of exogenous variables. For policy-grade forecasting, scenario-based projections (IMF WEO, UN population forecasts) are recommended.

---

## Getting Started

### Prerequisites

- R (version 4.5.0 or higher) - https://cran.r-project.org/
- RStudio (recommended) - https://posit.co/download/rstudio-desktop/

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
```

### Installation

```bash
git clone https://github.com/your-username/ARIMAX-Ghana-Government-Expenditure.git
cd ARIMAX-Ghana-Government-Expenditure
```

---

## Reproducing the Analysis

### Option 1: Knit the R Markdown (Recommended)

Open ARIMAX_GHANA_FULL_THESIS.Rmd in RStudio and click Knit (or press Ctrl+Shift+K). This will:

1. Load and preprocess Data2.xlsx
2. Run all diagnostics, model estimations, and forecasts
3. Generate all 15 figures
4. Produce a complete HTML/PDF thesis with embedded results

```r
rmarkdown::render("ARIMAX_GHANA_FULL_THESIS.Rmd", output_format = "all")
```

### Option 2: Run Analysis Scripts Interactively

Open the Rmd in RStudio and execute chunks sequentially, or source individual sections to explore:

- Data preprocessing: Section 4.2
- Stationarity tests: Section 4.5
- ARIMA baseline: Section 4.8
- ARIMAX models: Sections 4.9-4.12
- Full comparison: Section 4.13

### Expected Outputs

- 15 diagnostic and forecast figures
- 7 summary tables (variable descriptions, pairwise comparisons, full model comparison)
- Console output with ADF test results, Ljung-Box tests, and accuracy metrics
- 10-year point forecasts with 80% and 95% prediction intervals

---

## Technologies Used

| Tool | Purpose |
|------|---------|
| R 4.5.2 | Statistical computing environment |
| forecast 9.0.1 | auto.arima(), forecast(), checkresiduals(), accuracy() |
| tseries 0.10-58 | adf.test() - Augmented Dickey-Fuller unit root test |
| car 3.1-3 | vif() - Variance Inflation Factor |
| readxl 1.4.5 | Reading Data2.xlsx |
| tidyverse 2.0.0 | Data manipulation (dplyr, ggplot2, tidyr) |
| knitr 1.50 / rmarkdown 2.30 | Dynamic report generation |
| RStudio | Integrated development environment |

---

## Author

Mr. Gershon Tetteh Amanor

- Email: gershontettehamanor@gmail.com
- Institution: University of Ghana
- LinkedIn: [Gershon Tetteh Amanor](https://www.linkedin.com/in/gershon-tetteh-amanor-a9a302268?utm_source=share_via&utm_content=profile&utm_medium=member_android)
- GitHub: [Gershom-Tetteh-Amanor](https://github.com/Gershom-Tetteh-Amanor/ARIMAX-ANALYSIS-PROJECT)

---

## Citation

If you use this code, data, or findings in your research, please cite:

```bibtex
@thesis{amanor2026arimax,
  title     = {Forecasting Government Expenditure in Ghana Using Autoregressive
               Integrated Moving Average with Exogenous Variables (ARIMAX)},
  author    = {Amanor, Gershon Tetteh},
  year      = {2026},
  month     = {April},
  type      = {Undergraduate Thesis},
  institution = {University of Ghana},
  url       = {https://github.com/Gershom-Tetteh-Amanor/ARIMAX-ANALYSIS-PROJECT}
}
```

APA (7th ed.):

Amanor, G. T. (2026). Forecasting government expenditure in Ghana using autoregressive integrated moving average with exogenous variables (ARIMAX) [Undergraduate thesis, University of Ghana]. [GitHub](https://github.com/Gershom-Tetteh-Amanor/ARIMAX-ANALYSIS-PROJECT)

---

## License

This project is licensed under the MIT License - see the LICENSE file for details.

You are free to:
- Use, modify, and distribute the code
- Use the findings for academic and non-commercial purposes
- Cite the work in your own research

Under the condition that:
- You provide appropriate attribution to the author
- The original source is cited in derivative works

---

## Acknowledgements

- Bank of Ghana - Government Fiscal Operations data
- World Bank - National Accounts and population data
- Ghana Statistical Service - Population statistics
- International Monetary Fund - Fiscal policy context and ECF arrangement documentation
- R Core Team and the developers of the forecast, tseries, and car packages
- Academic supervisors and reviewers - for guidance on methodology and interpretation

---

## References

- Box, G. E. P., and Jenkins, G. M. (1970). Time Series Analysis: Forecasting and Control. Holden-Day.
- Keynes, J. M. (1936). The General Theory of Employment, Interest and Money. Macmillan.
- Peacock, A. T., and Wiseman, J. (1961). The Growth of Public Expenditure in the United Kingdom. Princeton University Press.
- Spatafora, N., and Samake, I. (2012). Commodity price shocks and fiscal outcomes. IMF Working Paper WP/12/183.
- Wagner, A. (1883). Finanzwissenschaft. Leipzig: C. F. Winter.
- World Bank. (2023). Ghana - Public Expenditure Review. Washington, DC: World Bank.

---

If you find this project useful, please consider giving it a star.

Made with dedication in Ghana.
