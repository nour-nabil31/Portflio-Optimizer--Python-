# 📈 Monte Carlo Portfolio Optimizer

A dynamic Python tool that runs Monte Carlo simulation to map the efficient frontier and identify optimal portfolio allocations across any set of stocks.

Built as part of my journey to bring quantitative rigor to FP&A and financial modeling — going beyond Excel with Python automation.

---

## What It Does

Most portfolio optimization tools are black boxes. This one is transparent and fully dynamic — feed it any tickers and it simulates 100,000 random weight combinations to find the best possible allocations across three dimensions:

| Portfolio | Goal |
|---|---|
| **Max Sharpe** | Best risk-adjusted return |
| **Min Volatility** | Lowest possible risk |
| **Max Return** | Highest raw annual return |

---

## Features

- **Monte Carlo Simulation** — 100,000 random portfolios simulated per run for high-resolution frontier mapping
- **Efficient Frontier Plot** — Sharpe ratio heatmap with key portfolios highlighted
- **Beta Calculation** — Each portfolio's beta computed against the S&P 500
- **Dynamic** — Works with any number of tickers, any time period
- **Clean Output** — Scrollable styled HTML summary table in Jupyter

---

## Example Output

### Efficient Frontier
The scatter plot maps all 100,000 simulated portfolios. Color represents Sharpe ratio (red = low, green = high). Diamond markers highlight the three optimal portfolios.

### Summary Table
| | Return | Volatility | Sharpe | Beta | ... |
|---|---|---|---|---|---|
| Max Return | 33.84% | 32.41% | 0.982 | 1.408 | ... |
| Min Volatility | 15.66% | 20.87% | 0.654 | 0.958 | ... |
| Max Sharpe | 30.44% | 26.39% | 1.077 | 1.195 | ... |

---

## Installation

```bash
pip install numpy pandas yfinance matplotlib
```

---

## Usage

```python
from portfolio_optimizer import optimize_portfolio

# Example — 15 tickers, custom start date
optimize_portfolio(
    ["CMI","TM","NKE","GM","SBUX","AAPL","FSLY","AVGO","NVDA","MSFT","AMZN","GOOGL","TSLA","WMT","META"],
    start = "2000-05-04"
)

# Fewer tickers, default settings
optimize_portfolio(["AAPL", "MSFT", "GOOGL"])

# Adjust trials or turn off plot
optimize_portfolio(["JPM", "BAC", "GS"], trials=10000, plot=False, risk_free=0.04)
```

### Parameters

| Parameter | Default | Description |
|---|---|---|
| `tickers` | required | List of stock ticker strings — any number, any sector |
| `start` | `"2020-01-01"` | Historical data start date (`"YYYY-MM-DD"`) |
| `trials` | `100000` | Number of Monte Carlo simulations — more trials = smoother frontier |
| `plot` | `True` | Show efficient frontier chart — set `False` for table only |
| `risk_free` | `0.02` | Annualized risk-free rate used in Sharpe ratio calculation |

> **Tip:** 100,000 trials gives a dense, smooth frontier. Drop to `10000` if you want faster results during exploration.

---

## Finance Logic

**Why Monte Carlo?**
Rather than solving the optimization mathematically (mean-variance optimization), Monte Carlo samples the weight space randomly at scale. At 100,000 trials, the simulation produces a dense, smooth frontier that closely approximates the theoretical optimum — without requiring convex optimization libraries.

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
