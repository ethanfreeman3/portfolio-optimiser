# Portfolio Optimisation: Does Mean-Variance Optimisation Beat Equal Weighting?

An implementation of Markowitz mean-variance optimisation, tested out-of-sample
against a naive 1/n benchmark.

## Finding

Across 7 independent rolling windows (3-year training, 1-year test, 2018–2024),
mean-variance optimisation **underperformed equal weighting**:

| Strategy | Mean OOS Sharpe | Mean OOS Volatility |
|---|---|---|
| Max-Sharpe optimised | 1.12 | 27% |
| Equal weighted | 1.44 | 18% |

The optimiser allocated 31–66% to a single asset in every window, and produced
50% higher volatility on average.

## Diagnosis

Applying Ledoit-Wolf shrinkage to the covariance matrix had negligible effect (shrinkage
intensity 0.02–0.05), indicating the covariance estimates were not the problem.
With ~750 observations estimating 55 parameters, the sample covariance matrix is
adequately conditioned.

The failure is driven by expected-return estimation, and the optimiser treats
these estimates as certain, concentrating into whichever assets happened to
perform best in-sample.

## Method

- 10 US large-caps across sectors, daily data 2015–2024 (yfinance)
- Constrained optimisation via `scipy.optimize.minimize`, long-only, fully invested
- Rolling windows with non-overlapping test periods

## Limitations

- 7 windows is a small sample, and equity years are themselves correlated
- Survivorship bias: universe selected from firms that exist today
- No transaction costs modelled, despite ~96% annual turnover
- US large-cap equities only; conclusions may not generalise

## Files

- `01_frontier_construction.ipynb` — data, random portfolios, efficient frontier
- `02_out_of_sample.ipynb` — rolling-window testing, shrinkage comparison
