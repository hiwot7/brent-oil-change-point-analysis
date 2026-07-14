# Birhan Energies: Brent Crude Oil Price Change Point Analysis
**Interim Methodology & EDA Report**  
*Author: Hiwot Fikadu*  
*Date: July 14, 2026*  

---

## 1. Project Overview & Business Need
Birhan Energies provides data-driven strategic advice to global energy market stakeholders. The objective of this project is to model how significant geopolitical events, OPEC policy changes, and macroeconomic shocks impact Brent crude oil prices. By detecting structural breaks and change points, we aim to deliver clear, quantitative insights to guide investment strategies and operational planning[cite: 1].

---

## 2. Planned Data Analysis Workflow
To achieve a robust analysis, we have structured our pipeline into five distinct stages[cite: 1]:

1. **Data Loading & Preprocessing:** Load the historical daily Brent crude oil dataset (1987–2022), handle mixed date-formatting dynamically, and sort data chronologically[cite: 1].
2. **Exploratory Data Analysis (EDA) & Stationarity Testing:** Plot the historical price and daily log returns[cite: 1]. Perform the Augmented Dickey-Fuller (ADF) test to evaluate stationarity[cite: 1].
3. **Bayesian Change Point Modeling:** Implement a Bayesian single change point model in `PyMC` using Markov Chain Monte Carlo (MCMC) sampling[cite: 1]. Define priors for the transition point ($\tau$) and price parameters before ($\mu_1$) and after ($\mu_2$) the break[cite: 1].
4. **Impact Quantification & Event Correlation:** Match detected structural changes with researched historical events and calculate the percentage price shift[cite: 1].
5. **Interactive Dashboard Development:** Package the model results into a Flask API backend and construct a dynamic React frontend visualization dashboard[cite: 1].

---

## 3. Assumptions and Limitations
While change point models are powerful, our analysis operates under key assumptions and structural limitations[cite: 1]:

* **Correlation vs. Causality:** A core limitation is that detecting a change point precisely aligned with a historical event (e.g., a policy announcement or military conflict) proves **statistical correlation in time, not direct mathematical causality**[cite: 1]. Global oil markets are complex; price shifts can be co-influenced by microeconomic factors, speculative trading, or lagging market signals[cite: 1].
* **Single Change Point Constraint:** Our core PyMC model assumes a single abrupt transition within a specified time slice to preserve computational efficiency and model interpretability[cite: 1].
* **Data Slicing:** To run the intensive MCMC sampling on a standard development environment, we slice the data into focused high-volatility windows (e.g., the year 2020)[cite: 1].

---

## 4. Initial EDA & Stationarity Findings
We investigated the properties of Brent crude prices over time to guide our modeling[cite: 1].

### Trend & Volatility Analysis
The price series shows high, non-constant volatility with clear structural jumps following major global shocks (e.g., the 2008 Financial Crisis and the 2020 COVID-19 pandemic)[cite: 1]. Calculating daily log returns reveals strong "volatility clustering"—periods of high volatility are followed by high volatility, and calm periods are followed by calm[cite: 1].

### Augmented Dickey-Fuller (ADF) Test Results
To check for unit roots, we ran the ADF test on both raw prices and computed daily log returns[cite: 1].

| Series | ADF Statistic | p-value | Interpretation |
| :--- | :---: | :---: | :--- |
| **Raw Prices** | $-1.9939$ | $0.28927$ | **Non-Stationary** (Strong Trend) |
| **Log Returns** | $-16.4271$ | $2.49858 \times 10^{-29}$ | **Stationary** (Constant Mean/Variance) |

The test confirms raw prices are non-stationary, meaning modeling raw prices directly requires a change-point structure to capture changes in mean over time[cite: 1]. Conversely, log returns are highly stationary, making them excellent inputs for standard statistical modeling[cite: 1].
