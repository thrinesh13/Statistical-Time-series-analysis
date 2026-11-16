<div align="center">

# Statistical Time Series Analysis: Apple (AAPL) Stock Performance and Risk

**How did the stock perform, how did its risk behave, and what does five years of daily data say about volume, seasonality and extreme moves?**

![Python](https://img.shields.io/badge/Python-3776AB?style=flat-square&logo=python&logoColor=white)
![pandas](https://img.shields.io/badge/pandas-150458?style=flat-square&logo=pandas&logoColor=white)
![SciPy](https://img.shields.io/badge/SciPy-8CAAE6?style=flat-square&logo=scipy&logoColor=white)
![Jupyter](https://img.shields.io/badge/Jupyter-F37626?style=flat-square&logo=jupyter&logoColor=white)

[Notebook](Apple_Inc.ipynb) · [Data Dictionary](DATA_DICTIONARY.md)

</div>

Price charts show what happened, but not whether a pattern is real. This project analyzes 1,254 trading days of Apple stock data to measure performance and risk, and uses statistical testing to separate real patterns from noise.

![Apple price trend and yearly returns](images/price_and_yearly_returns.png)

## Key Results

| **112.8%** | **1.65%** | **p = 0.267** | **6.64** |
|:---:|:---:|:---:|:---:|
| total return, $146 to $311 (16.3% CAGR) | average daily volatility, with no upward trend as price doubled | no significant volume difference between up and down days | excess kurtosis: extreme days far more common than a normal distribution predicts |

## Project Overview

| | |
|---|---|
| **Objective** | Measure five-year performance and risk, and test what drives volume, seasonality and extreme moves |
| **Data** | 1,254 trading days of AAPL daily prices and volume, Aug 2021 to Aug 2026 (0 missing, 0 duplicates) |
| **Tools** | Python, pandas, NumPy, SciPy, Matplotlib, Seaborn, Jupyter |
| **Techniques** | Feature engineering, exploratory data analysis, time series trend analysis, seasonality, correlation, distribution analysis, hypothesis testing |

## Business Questions

| # | Question | Analysis | Answer |
|:---:|---|---|---|
| 1 | How did the stock perform? | Trend analysis: total return, CAGR, yearly returns | **+112.8%**, including a **-28.6%** year in 2022 |
| 2 | Did risk rise as the price rose? | 30-day rolling volatility of % returns | **No.** Volatility moved in episodes (highest yearly average 2.20% in 2022), not upward |
| 3 | Does volume signal price direction? | Two-sample t-test, Pearson correlation | **No.** p = 0.267, return-volume r = 0.01 |
| 4 | Are there monthly or weekday patterns? | Seasonality analysis | **Not reliable.** Only 5 observations per month |
| 5 | Are daily returns normally distributed? | Normality test, skewness, kurtosis, Q-Q plot | **No.** p < 0.001, heavy tails |
| 6 | Is the price-volume correlation (r = -0.40) real? | Within-year comparison | **No.** Separate trends; shrinks to 1.15x within each year |

## Hypothesis Tests (α = 0.05)

| Test | H₀ | H₁ | p-value | Decision |
|---|---|---|---:|---|
| D'Agostino-Pearson normality test | Daily returns are normally distributed | Returns are not normally distributed | < 0.001 | **Reject H₀** |
| Two-sample t-test | Mean volume is equal on up and down days | Mean volume differs | 0.267 | **Fail to reject H₀** |

---

## Findings

**Risk was episodic, not trending.** Volatility measured on % returns showed no upward trend while the price doubled. A dollar-based measure would wrongly suggest rising risk.

![Volatility analysis](images/volatility_analysis.png)

**Volume did not predict direction.** Up days averaged 63.9M shares and down days 65.7M, a difference that was not significant.

![Volume and price relationship](images/volume_price.png)

**Returns had heavy tails.** Extreme daily moves happened far more often than a normal distribution predicts, so normal-based risk models would understate them.

![Return distribution](images/return_distribution.png)

**The price-volume correlation was misleading.** Price rose 112.8% while volume fell 39% on separate trends, which produced the -0.40 correlation. Day to day, returns and volume were unrelated (r = 0.01).

![Correlation matrix](images/correlation_matrix.png)

**Calendar patterns were weak.** July (+6.7%) and September (-3.3%) stood out, but each month rests on only five observations.

![Monthly seasonality](images/monthly_seasonality.png)

## Recommendations

1. Measure volatility in percentages, not dollars.
2. Check correlations for shared time trends before drawing conclusions.
3. Do not build timing rules on short histories.
4. Do not rely on normal-distribution risk models alone.

## Limitations

* One stock and one five-year window, with no market benchmark.
* Price returns only (dividends excluded); 2021 and 2026 are partial years.
* The notebook downloads the latest data on each run. Figures here are from the August 2026 run.

*Data from Yahoo Finance via `yfinance`. For educational purposes, not investment advice.*

---

**Thrinesh Vuribindi**, Data Analyst · [![LinkedIn](https://img.shields.io/badge/LinkedIn-0077B5?style=flat-square&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/thrineshv)
