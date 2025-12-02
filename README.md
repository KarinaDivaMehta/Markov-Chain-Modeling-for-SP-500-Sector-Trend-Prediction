# Market-Cap-Weighted Markov Chain Modeling: S\&P 500 Trend Prediction and Data Provenance

### Project Overview

This project provides an **end-to-end statistical analysis** of the S\&P 500, utilizing over a decade of historical price data. It models and predicts daily market movements (**Bull, Bear, or Stable**) using **Discrete-Time Markov Chains**. A key feature is the **rigorous data quality assessment and provenance documentation** to ensure the reliability of the underlying data. The final output is a Market-Cap-Weighted sector-level model used for state probability and price path forecasting.

-----

### Data Provenance & Quality Assurance

#### Data Source and Context

The project uses three related datasets describing the S\&P 500 index and its constituent companies.

  * **Source:** Aggregated S\&P 500 dataset from **Kaggle**, with underlying data collected from **Yahoo Finance** and the **Federal Reserve Bank of St. Louis (FRED)**.
  * **Index Structure:** The S\&P 500 is a **market capitalization-weighted** index, meaning a stock's influence on the index is proportional to its relative market capitalization. The index experiences annual turnover ($\sim 5\%$), leading to structural changes in the constituent list over time.
  * **Version Control:** The project critically assessed and utilized a verified dataset version **(v1022)**, as the most recent version was found to have over $67\%$ missing data, ensuring data integrity.

#### Missing Value Analysis

A deep dive was performed on the `sp500_stocks.csv` file ($1.89$ million rows) to understand data gaps:

  * **Structural Missingness:** The $\sim 5.3\%$ missing values were confirmed to be **structural** rather than data collection errors. These gaps correlate to periods before specific companies (e.g., ABNB, CRWD, UBER) were officially added to the S\&P 500 index, reflecting real-world index composition changes.
  * **Conclusion:** The missing data pattern is expected due to index composition changes and does not introduce bias for the modeling period.

-----

### Phenomenon and Background

Under the **Efficient Market Hypothesis (EMH)**, future price changes should be unpredictable. This project uses **non-parametric Markov Chains** to test whether short-term price movements and **volatility clustering**—properties often observed in real markets—can be captured by simple, data-driven transitions.

  * **Model Choice Rationale:** Markov chains are used because stock price movements depend more on recent price behavior than on the full historical distribution of prices. This model is more appropriate than descriptive statistical tools (like KDE or ECDF) because it focuses on the **dynamics of how prices evolve** rather than static price level distributions.
  * **Modeling Focus:** Modeling these dynamics helps us understand short-term market behavior and assess whether simple statistical models can reproduce actual market trends.

-----

### Technical Workflow & Methodology

#### 1\. Data Classification and State Definition

  * **Daily Return Calculation:** Computed the daily percentage change in the `Adj Close` price.
  * **State Classification:** Defined a 3-state system for daily returns based on a $\pm 0.002$ threshold:
      * **Bull:** Daily return $>$ $+0.2\%$
      * **Bear:** Daily return $<$ $-0.2\%$
      * **Stable:** Daily return within $\pm 0.2\%$

#### 2\. Markov Chain Training (Transition Matrices)

  * **Individual Stock Matrices:** Generated a separate $3 \times 3$ transition matrix for each stock using the training dataset (2010–2019).
  * **Sector Aggregation:** Consolidated individual stock matrices into a single, comprehensive **Sector Transition Matrix** using **Market Capitalization** as a weighting factor.

#### 3\. Simulation and Validation

  * **Simulation Phase:** Used the transition matrices to generate simulated price movements and state sequences over the test period (2020–2024).
  * **Price Path Forecasting:** Conducted **Monte Carlo Simulation** to project future price paths and generate a **95% Confidence Interval**.

-----

### Conclusion of Findings and Model Limitations

The **Markov Chain model successfully captured the broad directional properties and proportions** of market states observed in the historical data, with simulated price paths broadly mirroring the long-run upward trend. However, the simulation **should not be interpreted as a credible forecast** due to the model's fundamental limitations:

1.  **Memory-lessness:** The primary drawback is the **Markov property**, which assumes that only the most recent state influences the next movement. This prevents the model from capturing persistent real-world dynamics like momentum, volatility clustering, or mean reversion that extend beyond a single period.
2.  **Exogenous Factors:** The model does not incorporate critical real-world inputs such as macroeconomic conditions, firm-specific fundamental data (e.g., earnings transcripts, EBITDA), market sentiment, or structural drivers of returns.

**Future Work:** To improve forecasting, the project would benefit from transitioning to a parametric model (e.g., using **Transformers** for time-series) that can incorporate multiple inputs (e.g., trading volume, moving averages, sentiment from news headlines) and allow for greater memory in the sequence modeling. Alternatively, improving the current methodology would involve increasing the Markov chain order from one to $k$.

-----

### Dependencies and Setup

This project requires a standard Python data science environment.

| Package | Purpose |
| :--- | :--- |
| **`pandas`** | Data manipulation, transformation, and state sequence generation. |
| **`numpy`** | Numerical operations and Monte Carlo simulations. |
| **`matplotlib`** | General plotting, trend visualization, and CI plotting. |
| **`seaborn`** | Heatmap visualization of transition matrices. |

You can install the dependencies using the following:

```bash
pip install pandas numpy matplotlib seaborn
```
