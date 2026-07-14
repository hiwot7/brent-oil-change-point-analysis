# Birhan Energies: Brent Crude Oil Price Change Point Analysis
**Interim Methodology & EDA Report**  
*Author: Hiwot Fikadu*  
*Date: July 14, 2026*  

---

## 1. Project Overview & Business Need
Birhan Energies provides data-driven strategic advice to global energy market stakeholders. The objective of this project is to model how significant geopolitical events, OPEC policy changes, and macroeconomic shocks impact Brent crude oil prices. By detecting structural breaks and change points, we aim to deliver clear, quantitative insights to guide investment strategies and operational planning.

---

## 2. Planned Data Analysis Workflow
To achieve a robust analysis, we have structured our pipeline into five distinct stages:

1. **Data Loading & Preprocessing:** Load the historical daily Brent crude oil dataset (1987–2022), handle mixed date-formatting dynamically, and sort data chronologically.
2. **Exploratory Data Analysis (EDA) & Stationarity Testing:** Plot the historical price and daily log returns. Perform the Augmented Dickey-Fuller (ADF) test to evaluate stationarity.
3. **Bayesian Change Point Modeling:** Implement a Bayesian single change point model in `PyMC` using Markov Chain Monte Carlo (MCMC) sampling. Define priors for the transition point ($\tau$) and price parameters before ($\mu_1$) and after ($\mu_2$) the break.
4. **Impact Quantification & Event Correlation:** Match detected structural changes with researched historical events and calculate the percentage price shift.
5. **Interactive Dashboard Development:** Package the model results into a Flask API backend and construct a dynamic React frontend visualization dashboard.

---

## 3. Assumptions and Limitations
While change point models are powerful, our analysis operates under key assumptions and structural limitations:

* **Correlation vs. Causality:** A core limitation is that detecting a change point precisely aligned with a historical event (e.g., a policy announcement or military conflict) proves **statistical correlation in time, not direct mathematical causality**. Global oil markets are complex; price shifts can be co-influenced by microeconomic factors, speculative trading, or lagging market signals.
* **Single Change Point Constraint:** Our core PyMC model assumes a single abrupt transition within a specified time slice to preserve computational efficiency and model interpretability.
* **Data Slicing:** To run the intensive MCMC sampling on a standard development environment, we slice the data into focused high-volatility windows (e.g., the year 2020).

---

## 4. Initial EDA & Stationarity Findings
We investigated the properties of Brent crude prices over time to guide our modeling.

### Visual Analysis of Historical Trends & Volatility
Visual exploration of our 35-year historical price dataset highlights distinct pricing regimes and exceptional shock windows:
* **Structural Market Regimes:** Historically, raw Brent prices remained consistently constrained below $40/barrel throughout the late 1980s and 1990s. This was followed by a massive macroeconomic commodity supercycle starting in the early 2000s, peaking at an all-time high of over $140/barrel in mid-2008 immediately preceding the Global Financial Crisis (GFC) crash.
* **The Extreme 2020 Outlier:** The daily log returns plot clearly exposes a massive, unprecedented spike in **early 2020**. During the onset of the COVID-19 pandemic, log returns plunged below $-0.6$ before surging above $+0.4$ in a highly concentrated period, marking the most severe single volatility shock in modern oil market history.
* **Clustering Dynamics:** Distinct dense spikes in the daily returns chart (notably circa 1991, 2008, and 2020) visually validate **volatility clustering** (periods of high volatility followed by more high volatility). This confirms the presence of temporary regimes with high variance.

### Augmented Dickey-Fuller (ADF) Test Results
To check for unit roots, we ran the ADF test on both raw prices and computed daily log returns.

| Series | ADF Statistic | p-value | Interpretation |
| :--- | :---: | :---: | :--- |
| **Raw Prices** | $-1.9939$ | $0.28927$ | **Non-Stationary** (Strong Trend) |
| **Log Returns** | $-16.4271$ | $2.49858 \times 10^{-29}$ | **Stationary** (Constant Mean/Variance) |

The test confirms raw prices are non-stationary, meaning modeling raw prices directly requires a change-point structure to capture changes in mean over time. Conversely, log returns are highly stationary, making them excellent inputs for standard statistical modeling.
