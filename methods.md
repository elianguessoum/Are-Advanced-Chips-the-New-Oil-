# Methodology and Statistical Methods

## 1. Overview

This project analyzes financial time series from multiple companies across different sectors.  
The goal is to identify statistical relationships and potential causal dynamics between sectors over time.

Each sector’s aggregated performance was computed using **weighted averages by market capitalization**, followed by **cross-sector comparison** through time-series analysis methods including correlation, stationarity testing, and Granger causality.

---

## 2. Data Preparation

### 2.1 Source
Each Excel file contains one sheet per company, including variables such as stock price, returns, and market capitalization.

### 2.2 Grouping by Sector
Companies were grouped by their economic sector (e.g., Technology, Energy, Finance, etc.).

For each sector \( s \) at time \( t \), a **weighted average** was computed as:

\[
S_{s,t} = \frac{\sum_{i=1}^{n_s} P_{i,t} \times M_{i,t}}{\sum_{i=1}^{n_s} M_{i,t}}
\]

Where:  
- \( P_{i,t} \) = price (or return) of company *i* at time *t*  
- \( M_{i,t} \) = market capitalization of company *i* at time *t*  
- \( n_s \) = number of companies in sector *s*  

This gives the sector-level time series \( S_{s,t} \).

---

## 3. Time Series Visualization

The time series of each sector \( S_{s,t} \) was plotted to inspect overall trends, volatility, and potential co-movements.

\[
t \mapsto S_{s,t}
\]

This visual inspection helps identify temporal alignment and possible correlations between sectors.

---

## 4. Cross-Correlation Analysis

For each pair of sectors \( (s_1, s_2) \), the **cross-correlation function (CCF)** was computed to measure the similarity of two time series as a function of lag \( k \):

\[
\rho_{s_1,s_2}(k) = \frac{\sum_t (S_{s_1,t} - \bar{S}_{s_1}) (S_{s_2,t-k} - \bar{S}_{s_2})}{\sqrt{\sum_t (S_{s_1,t} - \bar{S}_{s_1})^2 \sum_t (S_{s_2,t-k} - \bar{S}_{s_2})^2}}
\]

Where \( \rho(k) \) measures how sector \( s_1 \) leads or lags sector \( s_2 \).

---

## 5. Stationarity Testing

Because many financial series are non-stationary, each sector’s series was tested for **unit roots** using the **Augmented Dickey–Fuller (ADF) test**.

**Null hypothesis:** the series has a unit root (non-stationary).  
**Alternative:** the series is stationary.

If the p-value > 0.05, the null was not rejected → the series was **differenced**:

\[
\Delta S_{s,t} = S_{s,t} - S_{s,t-1}
\]

The differencing was repeated until the series became stationary.

---

## 6. Granger Causality Testing

To examine whether one sector helps predict another, **pairwise Granger causality tests** were conducted on the stationary series.

For two stationary series \( X_t \) and \( Y_t \):

Model 1 (restricted):  
\[
Y_t = a_0 + a_1 Y_{t-1} + \cdots + a_p Y_{t-p} + \varepsilon_t
\]

Model 2 (unrestricted):  
\[
Y_t = a_0 + a_1 Y_{t-1} + \cdots + a_p Y_{t-p} + b_1 X_{t-1} + \cdots + b_p X_{t-p} + \varepsilon_t
\]

If including past values of \( X_t \) significantly improves prediction of \( Y_t \) (based on F-test p-value < 0.05), then **X “Granger-causes” Y**.

---

## 7. Summary of Statistical Workflow

1. Aggregate company data by sector using weighted averages.
2. Visualize sectoral time series.
3. Compute cross-correlations to explore interdependencies.
4. Test for stationarity using ADF.
5. Difference non-stationary series until stationary.
6. Apply Granger causality tests to detect predictive relationships between sectors.

---

## 8. Tools and Libraries

- **Python:** `pandas`, `numpy`, `statsmodels`, `matplotlib`
- **Statistical tests:**  
  - ADF: `statsmodels.tsa.stattools.adfuller`  
  - Granger causality: `statsmodels.tsa.stattools.grangercausalitytests`
  - Cross-correlation: `numpy.correlate` or `statsmodels.tsa.stattools.ccf`

---

## 9. References

- Granger, C. W. J. (1969). *Investigating Causal Relations by Econometric Models and Cross-spectral Methods*. Econometrica, 37(3), 424–438.  
- Dickey, D. A., & Fuller, W. A. (1979). *Distribution of the Estimators for Autoregressive Time Series with a Unit Root*. JASA, 74(366), 427–431.
