<div align="center">

# Statistical Time Series Analysis: Testing Market Assumptions with Apple (AAPL) Data

**Do common beliefs about stock price behavior hold up when they are tested against five years of real trading data?**

![Python](https://img.shields.io/badge/Python-3776AB?style=flat-square&logo=python&logoColor=white)
![pandas](https://img.shields.io/badge/pandas-150458?style=flat-square&logo=pandas&logoColor=white)
![SciPy](https://img.shields.io/badge/SciPy-8CAAE6?style=flat-square&logo=scipy&logoColor=white)
![Jupyter](https://img.shields.io/badge/Jupyter-F37626?style=flat-square&logo=jupyter&logoColor=white)

</div>

![Apple price trend and yearly returns](images/price_and_yearly_returns.png)

---

## Project Overview

| | |
|---|---|
| **Domain** | Financial markets: stock price and trading volume analysis |
| **Business problem** | Widely repeated market beliefs are often used to judge risk and timing without being checked against data |
| **Who it is for** | Investment and risk analysts, portfolio teams, and anyone making decisions from time series data |
| **Data** | 1,254 trading days of Apple (AAPL) daily prices and volume, August 2021 to August 2026 |
| **Tools** | Python, pandas, NumPy, SciPy, Matplotlib, Seaborn, yfinance, Jupyter |
| **Output** | A tested verdict on four common assumptions, supported by statistical tests and charts |

## Key Findings

| **112.8%** | **1.65%** | **p = 0.267** | **6.64** |
|:---:|:---:|:---:|:---:|
| price growth, $146 to $311 (16.3% per year) | average daily volatility, with no upward trend | volume difference between up and down days (not significant) | excess kurtosis, showing extreme days are far more common than a bell curve predicts |

| Common belief | Verdict | What the data showed |
|---|---|---|
| A higher stock price means higher risk | **Not supported** | Price more than doubled, but percentage volatility did not trend upward |
| Trading volume predicts price direction | **Not supported** | Volume on up days and down days was statistically the same |
| Some months or weekdays reliably perform better | **Inconclusive** | Monthly patterns appeared but rest on only 5 years; weekday effects were negligible |
| Daily returns follow a normal (bell curve) distribution | **Not supported** | Returns failed the normality test (p < 0.001) with heavy tails |

---

## Business Problem

Many market decisions rest on rules of thumb: "a stock that has doubled is riskier," "high volume confirms a move," "September is a bad month." Each one sounds reasonable, and each one can change how risk is measured or when a decision is made.

The problem is that these beliefs are rarely tested. A chart can make a trend look real when it is actually caused by how the metric was measured. For example, a $300 stock moving $5 a day looks more volatile than a $150 stock moving $2.50, but both moved the same 1.7%.

This project tests four of these beliefs against five years of Apple's daily trading data. It uses statistical tests to separate real patterns from ones that only appear because of how the data is measured or how short the time window is.

## Business Questions

1. Does risk increase as the stock price rises?
2. Does trading volume tell us anything about which direction the price will move?
3. Are there reliable monthly or weekday patterns in returns?
4. Do daily returns behave the way standard risk models assume?

## Stakeholders

| Stakeholder | Decisions Supported |
|---|---|
| Risk analysts | Whether to measure volatility in percentage or dollar terms, and whether normal-distribution models understate extreme moves |
| Portfolio and investment analysts | Whether volume or calendar timing should factor into decisions |
| Data and business analysts | How to avoid misleading correlations when two measures both trend over time |

---

## Detailed Findings

### 1. A higher price did not mean higher risk

Apple's price rose from $146 to $311, but its 30-day volatility (measured as the typical daily percentage move) showed no upward trend.

| Year | Average daily volatility |
|---|---:|
| 2021 | 1.38% |
| 2022 | **2.20%** (most volatile) |
| 2023 | 1.32% |
| 2024 | 1.39% |
| 2025 | 1.84% (single peak of 4.40%) |
| 2026 | 1.55% |

Risk was **episodic, not trending**. Measuring in dollars would have suggested rising risk, but that is a side effect of the higher price, not a change in behavior.

![Volatility analysis](images/volatility_analysis.png)

### 2. Trading volume did not predict price direction

| | Average daily volume |
|---|---:|
| Up days | 63.9M shares |
| Down days | 65.7M shares |
| t-test p-value | **0.267** (not significant) |

The correlation between daily returns and volume was **0.01**, effectively zero. An apparent "low prices attract more volume" pattern (4.8x) shrank to **1.15x** once each day was compared with its own year's averages, showing that the original pattern came from the time trend rather than from trader behavior.

![Volume and price relationship](images/volume_price.png)

### 3. Calendar patterns were weak and unreliable

- **Strongest months on average:** July (+6.7%), November (+5.3%), May (+4.3%), October (+3.9%)
- **Weakest month:** September (-3.3%)
- **Weekdays:** average returns ranged from -0.09% (Thursday) to +0.16% (Wednesday), which is small next to a typical daily move of 1.6% to 1.9%

Each monthly average is based on only five observations, so one unusual year can change the ranking. These are patterns in this sample, not dependable signals.

![Monthly seasonality](images/monthly_seasonality.png)

### 4. Returns did not follow a bell curve

The D'Agostino normality test rejected a normal distribution (**p < 0.001**). Excess kurtosis of **6.64** means very large daily moves happened much more often than a bell curve predicts. Risk models that assume normal returns would **understate the chance of extreme days**.

![Return distribution](images/return_distribution.png)

### 5. A misleading correlation between price and volume

Closing price and volume had a correlation of **-0.40**, which looks like "volume falls as price rises." In reality, both were moving on separate long-term trends: price rose 112.8% while average daily volume fell 39% (85M to 51M shares). The day-to-day relationship between returns and volume was **0.01**.

![Correlation matrix](images/correlation_matrix.png)

### 6. Strong growth came with a large drawdown

| Year | Price return |
|---|---:|
| 2021 (Aug to Dec) | +21.5% |
| 2022 | **-28.6%** |
| 2023 | **+53.9%** |
| 2024 | +34.9% |
| 2025 | +11.5% |
| 2026 (Jan to Aug) | +14.8% |

The 112.8% total return included a year in which the stock lost more than a quarter of its value.

---

## Key Takeaways

1. **Measure volatility in percentages, not dollars.** Dollar-based measures create a false impression of rising risk as prices grow.
2. **Check correlations for shared time trends.** Two measures that both trend over time can look related when they are not.
3. **Be cautious with calendar patterns from short histories.** Five data points per month are not enough to support a timing rule.
4. **Do not rely on normal-distribution risk models alone.** Extreme days occur far more often than those models expect.

*This analysis is for educational purposes and is not investment advice.*

---

## Methodology

<details>
<summary><b>Data Preparation, Features and Statistical Tests</b></summary>

<br>

**1. Data collection**
- Downloaded 1,254 daily records (Open, High, Low, Close, Adj Close, Volume) through `yfinance`
- Confirmed 0 missing values and 0 duplicate rows

**2. Feature engineering**

| Feature | Definition |
|---|---|
| Daily return | Percentage change in closing price from the previous day |
| Daily change | Close minus open, used to classify up and down days |
| Daily range | High minus low, in dollars and as a percentage of the open |
| 30-day moving average | Rolling average of closing price |
| 30-day volatility | Rolling standard deviation of daily returns |

**3. Statistical tests**

| Question | Method |
|---|---|
| Are returns normally distributed? | D'Agostino normality test, skewness, kurtosis, Q-Q plot |
| Does volume differ on up and down days? | Two-sample t-test |
| How are the measures related? | Pearson correlation matrix |
| Is the price and volume pattern real? | Within-year comparison to remove the time trend |

**4. Time-based analysis**
- Yearly returns, monthly returns averaged across years, and weekday averages

**Additional charts**

![Trends and seasonality](images/trends_seasonality.png)
![Weekday effect](images/weekday_effect.png)

</details>

---

## Limitations

- **One stock, one time window.** Results describe Apple from August 2021 to August 2026 and may not apply to other stocks or periods.
- **No market benchmark.** Returns are not compared against the S&P 500 or NASDAQ.
- **Dividends excluded.** Returns are based on price only, so total shareholder return was slightly higher.
- **Partial years.** 2021 and 2026 cover about 5 and 7 months, so their returns are not directly comparable to full years.
- **Data refreshes on each run.** The notebook downloads the latest five years every time, so figures will change if it is run again. All figures here come from the August 2026 run.

## Data Source

Daily AAPL trading data from Yahoo Finance, accessed through the [`yfinance`](https://pypi.org/project/yfinance/) Python library. Full field definitions and formulas are in the [Data Dictionary](DATA_DICTIONARY.md).

## Repository Structure

| File / folder | Contents |
|---|---|
| [`Apple_Inc.ipynb`](Apple_Inc.ipynb) | Full analysis notebook with code and outputs |
| [`DATA_DICTIONARY.md`](DATA_DICTIONARY.md) | Field definitions, formulas, caveats and glossary |
| [`images/`](images/) | Exported charts used in this README |

---

## Author

**Thrinesh Vuribindi**, Data Analyst

[![LinkedIn](https://img.shields.io/badge/LinkedIn-0077B5?style=flat-square&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/thrineshvuribindi)
[![GitHub](https://img.shields.io/badge/GitHub-181717?style=flat-square&logo=github&logoColor=white)](https://github.com/thrinesh13)
