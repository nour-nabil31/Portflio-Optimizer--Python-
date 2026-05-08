# 📈 Monte Carlo Portfolio Optimizer

A dynamic Python tool that runs Monte Carlo simulation to map the efficient frontier and identify optimal portfolio allocations across any set of stocks.

Built as part of my journey to bring quantitative rigor to FP&A and financial modeling — going beyond Excel with Python automation.

---

## What It Does

Most portfolio optimization tools are black boxes. This one is transparent and fully dynamic — feed it any tickers and it simulates 5,000 random weight combinations to find the best possible allocations across three dimensions:

| Portfolio | Goal |
|---|---|
| **Max Sharpe** | Best risk-adjusted return |
| **Min Volatility** | Lowest possible risk |
| **Max Return** | Highest raw annual return |

---

## Features

- **Monte Carlo Simulation** — 5,000 random portfolios simulated per run
- **Efficient Frontier Plot** — Sharpe ratio heatmap with key portfolios highlighted
- **Beta Calculation** — Each portfolio's beta computed against the S&P 500
- **Dynamic** — Works with any number of tickers, any time period
- **Clean Output** — Scrollable styled HTML summary table in Jupyter

---

## Example Output

### Efficient Frontier
The scatter plot maps all 5,000 simulated portfolios. Color represents Sharpe ratio (red = low, green = high). Diamond markers highlight the three optimal portfolios.

### Summary Table
| | Return | Volatility | Sharpe | Beta | ... |
|---|---|---|---|---|---|
| Max Return | 30.72% | 29.59% | 0.971 | 1.294 | ... |
| Min Volatility | 17.47% | 21.43% | 0.722 | 0.990 | ... |
| Max Sharpe | 29.80% | 26.51% | 1.049 | 1.211 | ... |

---

## Installation

```bash
pip install numpy pandas yfinance matplotlib
```

---

## Usage

```python
from portfolio_optimizer import optimize_portfolio

# Any tickers, any time period
results = optimize_portfolio(
    tickers   = ["AAPL", "MSFT", "GOOGL", "NVDA"],
    start     = "2020-01-01",
    trials    = 5000,
    plot      = True,
    risk_free = 0.02        # annualized risk-free rate
)

# Access results programmatically
best_sharpe_weights = results["best_sharpe"]
all_portfolios      = results["portfolios"]  # full 5,000-row DataFrame
```

### Parameters

| Parameter | Default | Description |
|---|---|---|
| `tickers` | required | List of stock ticker strings |
| `start` | `"2020-01-01"` | Historical data start date |
| `trials` | `5000` | Number of Monte Carlo simulations |
| `plot` | `True` | Show efficient frontier chart |
| `risk_free` | `0.02` | Annualized risk-free rate for Sharpe ratio |

### Return Value

Returns a dictionary with four keys:

```python
{
    "portfolios":  pd.DataFrame,  # all simulated portfolios (Return, Volatility, Sharpe, Beta, weights)
    "best_return": pd.Series,     # highest return portfolio
    "lowest_vol":  pd.Series,     # lowest volatility portfolio
    "best_sharpe": pd.Series,     # highest Sharpe ratio portfolio
}
```

---

## Finance Logic

**Why Monte Carlo?**
Rather than solving the optimization mathematically (mean-variance optimization), Monte Carlo samples the weight space randomly at scale. With enough trials, it approximates the full efficient frontier without requiring convex optimization libraries.

**Why three portfolios?**
There is no single "best" portfolio — it depends on the investor's objective:
- A risk-averse investor targets **Min Volatility**
- A return-maximizing investor targets **Max Return**
- A rational investor balancing both targets **Max Sharpe**

**Beta vs S&P 500**
Beta measures how much the portfolio moves relative to the broader market. A beta of 1.2 means the portfolio amplifies market moves by 20%. Calculated as `Cov(portfolio, market) / Var(market)` using daily log returns.

**Annualization**
Returns and covariance are annualized using **252 trading days** — the standard used by financial professionals.

---

## Tech Stack

- `pandas` — data wrangling and return calculations
- `numpy` — matrix operations, covariance, simulation
- `yfinance` — historical price data via Yahoo Finance
- `matplotlib` — efficient frontier visualization

---

## About

**Nour Nabil** — Finance undergraduate (GPA 3.63) with experience in equity research and FP&A modeling. Building Python tools to automate financial analysis and go beyond what Excel alone can do.

[LinkedIn](https://linkedin.com/in/your-profile) · [Email](mailto:your-email)
