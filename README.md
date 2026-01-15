# 💧 Global Portfolio Engine (Liquid Alpha Edition v48.3)

An advanced **research-grade quantitative portfolio backtesting engine** designed for systematic portfolio construction and robustness analysis.  
This project runs entirely **client-side in the browser** and is deployable via GitHub Pages with no backend required.

> ⚠️ Educational and research purposes only. Not financial advice.

---

## 🔗 Live Demo
https://<your-username>.github.io/global-portfolio-engine/

---

## 1. System Overview

Global Portfolio Engine (GPE) is a **static web-based quant engine** that allows users to:

- Fetch historical **price + volume** data
- Apply **liquidity filtering**
- Run multiple **portfolio strategies simultaneously**
- Perform **grid search** across parameters
- Evaluate **robustness** and **distributional behavior**
- Visualize results using interactive charts

All calculations are performed locally in the browser.

---

## 2. End-to-End Workflow

```text
[1] Load Market Data (Price + Volume)
    ↓
[2] Liquidity Filtering (ADV)
    ↓
[3] Signal & Risk Calculation
    ↓
[4] Strategy Allocation
    ↓
[5] Rebalance / Fees / DCA
    ↓
[6] Performance Metrics
    ↓
[7] Surface, Distribution & Monte Carlo Analysis
````

---

## 3. Step 1 — Market Data Preparation

### Data Sources

| Asset Class | Source        |
| ----------- | ------------- |
| ETFs        | Yahoo Finance |
| Crypto      | Binance       |

### Data Used

* Daily Close Price
* Daily Trading Volume

All assets are **date-aligned** to a common timeline using forward-fill logic.

---

## 4. Step 2 — Liquidity Filter (Liquid Alpha Core)

### Purpose

To eliminate illiquid assets and reduce:

* Unrealistic fills
* Backtest over-optimism
* Excessive turnover noise

### Formula — Average Daily Value (ADV)

[
ADV_i = \frac{1}{W} \sum_{t=1}^{W} (Price_{i,t} \times Volume_{i,t})
]

| Symbol         | Description                             |
| -------------- | --------------------------------------- |
| (ADV_i)        | Average daily traded value of asset *i* |
| (W)            | Liquidity lookback window (days)        |
| (Price_{i,t})  | Closing price on day *t*                |
| (Volume_{i,t}) | Trading volume on day *t*               |

### Numerical Example

Liquidity window = 3 days

**Asset A**

* Day 1: 100 × 1,000 = 100,000
* Day 2: 102 × 1,200 = 122,400
* Day 3: 101 × 800 = 80,800

[
ADV_A = (100,000 + 122,400 + 80,800) / 3 = 101,066.67
]

**Asset B**

* Day 1: 50 × 5,000 = 250,000
* Day 2: 52 × 4,000 = 208,000
* Day 3: 51 × 4,500 = 229,500

[
ADV_B = 229,166.67
]

➡️ Asset B is selected due to higher liquidity.

---

## 5. Step 3 — Return & Volatility Calculation

### Daily Return

[
r_t = \frac{P_t}{P_{t-1}} - 1
]

### Volatility (Rolling Standard Deviation)

[
\sigma = \sqrt{\frac{1}{N} \sum (r_t - \bar{r})^2}
]

Annualized volatility:
[
\sigma_{annual} = \sigma \times \sqrt{TradingDays}
]

---

## 6. Step 4 — Portfolio Allocation Strategies

### 6.1 Equal Weight (1/N)

[
w_i = \frac{1}{N}
]

---

### 6.2 Rank-Based Allocation

[
Score_i = N - Rank_i + 1
]
[
w_i = \frac{Score_i}{\sum Score}
]

**Example (3 assets)**

| Asset | Rank | Weight |
| ----- | ---- | ------ |
| A     | 1    | 50%    |
| B     | 2    | 33.3%  |
| C     | 3    | 16.7%  |

---

### 6.3 Top 3 Leader (Equal Weight)

[
w_i = \frac{1}{3}
]

---

### 6.4 Top 3 Leader (Ranked)

[
w_i = \frac{K - Rank_i + 1}{\sum_{j=1}^{K} j}, \quad K = 3
]

---

### 6.5 Top 50% Filter

[
K = \lceil N/2 \rceil, \quad w_i = \frac{1}{K}
]

---

### 6.6 Absolute Momentum

Invest only if:
[
Return_{LB} > 0
]

---

### 6.7 Dual Momentum

Conditions:

1. Positive absolute return
2. Top-ranked relative momentum

---

### 6.8 Inverse Volatility (Risk Parity)

[
w_i = \frac{1/\sigma_i}{\sum (1/\sigma_j)}
]

**Example**

* Asset A: Vol = 10% → Weight ≈ 66.6%
* Asset B: Vol = 20% → Weight ≈ 33.3%

---

### 6.9 Adaptive Aggressive (AAA)

1. Filter assets with positive momentum
2. Apply inverse volatility weighting

---

## 7. Step 5 — Rebalancing, Fees & DCA

### Transaction Fee

[
NetAmount = Amount \times (1 - Fee%)
]

### Daily Management Fee

[
Fee_{daily} = \frac{Fee_{annual}}{TradingDays}
]

### DCA Allocation Logic

New capital is allocated preferentially to assets with the **largest weight deficit** versus target allocation.

---

## 8. Step 6 — Performance Metrics

### CAGR

[
CAGR = \left(\frac{Final}{Initial}\right)^{1/Years} - 1
]

---

### Sharpe Ratio

[
Sharpe = \frac{E[R - R_f]}{\sigma}
]

---

### Sortino Ratio

Uses downside deviation only.

---

### Maximum Drawdown (MDD)

[
MDD = \min \left(\frac{NAV_t - Peak}{Peak}\right)
]

---

### Calmar Ratio

[
Calmar = \frac{CAGR}{|MDD|}
]

---

## 9. Step 7 — Robustness & Surface Analysis

### Parameter Surface

* X-axis: Lookback window
* Y-axis: Rebalance interval
* Z-axis: Performance metric
* Time dimension: rolling horizon

### Robustness Score

Computed using a **Gaussian-weighted neighborhood average** to avoid over-fitting to local extrema.

---

## 10. Monte Carlo Projection

Simulates future portfolio paths using:

* Empirical mean return
* Empirical volatility
* Geometric Brownian Motion

Used to evaluate forward-looking risk distributions.

---

## Summary

Global Portfolio Engine is designed to:

* Prevent liquidity bias
* Reduce overfitting
* Enable robust strategy comparison
* Provide research-grade portfolio diagnostics
* Operate entirely in the browser

---

## License

MIT License — for educational and research use.
