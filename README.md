# Markowitz-Optimization
# Mean-Variance Portfolio Optimization (Markowitz Model)

This project implements the Modern Portfolio Theory (MPT) introduced by Harry Markowitz in the mid-20th century. His contribution was fundamental to the field of investment evaluation. Until then, securities were evaluated individually, estimating risk and return in isolation. Markowitz advanced a revolutionary theory, considering the securities within a portfolio as interdependent through mathematical correlation relationships. Based on this acute insight, he proposed a new operational method that evaluates the portfolio through a holistic, collective vision.

The evaluation of an investment is based on two fundamental and seemingly opposing objectives: maximizing expected return and minimizing risk. Markowitz demonstrated how portfolio diversification allows for a harmonious synthesis between these two objectives, successfully moderating risk without sacrificing return.

In this project, our objective is to use Markowitz's theory to determine the optimal capital allocation across a basket of real equity assets. Specifically, we consider the following assets: AAPL, MSFT, TSLA, AMZN. From a methodological standpoint, the project translates linear algebra concepts (vectors, covariance matrices, and constrained optimization) into reproducible Python code.

## MATHEMATICAL FOUNDATIONS
Let $N$ be the number of assets in the portfolio. We define:
* The weight vector $w = [w_1, w_2, \dots, w_N]^T$, subject to the following budget constraints: the sum of the weights equals 1 ($\sum_{i=1}^N w_i = 1$) and there are no short sales ($w_i \ge 0$).
* The annualized expected return vector $\mu$, calculated from the historical log-returns of the individual securities.
* The annualized covariance matrix $\Sigma$, which captures the variance of each asset and the mutual covariance (correlation) between pairs of securities.

## KEY IMPLEMENTED FORMULAS
* **Expected Portfolio Return ($R_p$):**
  $$R_p = w^T \mu$$
* **Portfolio Volatility / Risk ($\sigma_p$):**  
  Risk is not a simple average of individual risks (as was believed prior to Markowitz's research), but accounts for the interaction among securities via the quadratic form:
  $$\sigma_p = \sqrt{w^T \Sigma w}$$
* **Sharpe Ratio ($S_p$):**  
  Measures portfolio efficiency (return per unit of risk, assuming a risk-free rate of zero):
  $$S_p = \frac{R_p}{\sigma_p}$$

## DEVELOPMENT PIPELINE (Python)
The code is structured through the following steps:
1. **Data Acquisition:** Downloading historical daily closing prices (past 3 years) via the Yahoo Finance API (`yfinance`).
2. **Feature Engineering:** Calculating daily log-returns $r_t = \ln(P_t) - \ln(P_{t-1})$, analogously to the stochastic processes used in regime detection models.
3. **Statistical Estimation:** Computing the vector $\mu$ and the matrix $\Sigma$ scaled on an annual basis (assuming 252 trading days per year).
4. **Monte Carlo Simulation:** Generating 10,000 random portfolios with randomized weights to map the space of possible risk-return combinations.
5. **Optimal Portfolio Identification:** Extracting the portfolio that maximizes the Sharpe Ratio (the optimal combination on the Efficient Frontier).

## GRAPH AND CRITICAL ANALYSIS
To graphically represent Markowitz's theory, we combine a set of assets in all possible ways on an $(x, y)$ plane representing volatility on the x-axis and expected return on the y-axis, respectively. This yields a cloud of points whose upper perimeter, called the Efficient Frontier, contains all portfolios offering the maximum possible return for a given level of risk. On the Efficient Frontier, there is one uniquely optimal portfolio corresponding to the point that guarantees the maximum slope when connecting the origin (risk-free rate) to the curve.

![Efficient Frontier Plot](https://github.com/user-attachments/assets/95f1acef-38bc-4653-b56d-179e6da916b5)

### The Optimal Portfolio (Red Star):
The portfolio with the maximum Sharpe Ratio identified by the algorithm exhibits the following results calculated on historical data:

* **Annualized Expected Return:** 28.90%
* **Annualized Volatility (Risk):** 22.46%
* **Optimal Weight Allocation:**
  * **AAPL:** 36.63%
  * **MSFT:** 23.08%
  * **TSLA:** 33.07%
  * **AMZN:** 7.22%
