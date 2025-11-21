# Machine Learning Based Pairs Trading

## 1. Problem Statement

This project focuses on predicting and taking advantage of short-term price differences between two related stocks in the S&P 500. The idea is based on **pairs trading**, a market-neutral strategy widely used by traders and financial analysts. In pairs trading, two stocks that usually move in similar ways are monitored over time. When their price difference temporarily becomes unusually large, traders expect the prices to eventually return to their normal relationship.

For example, if Pepsi’s stock drops while Coca-Cola’s stays stable, a trader might buy Pepsi and sell Coca-Cola, expecting them to come back together. Profit is earned when the price relationship normalizes.

The challenge is identifying **which pairs** remain dependable over time and **when** the price gap will revert. Many pairs stop behaving similarly due to market shifts, company events, or broader economic changes. This means relying only on historical relationships is not enough.

Traditional methods like price spreads, Z-scores, and cointegration help find pairs with linear, stable relationships. However, real stock data often contains nonlinear patterns and structural changes. Modern machine learning can uncover complex relationships that classical tools miss.

This project combines traditional statistical methods with machine learning to better predict spread reversion and improve the accuracy of pairs trading decisions.

---

## 2. Motivation

Our team is interested in learning finance and understanding how data-driven methods can improve trading strategies. Classic pairs trading depends on simple correlations and historical relationships that often break down when stock behavior becomes nonlinear or changes due to market events.

By combining traditional econometric methods with modern ML models, we aim to:

* Identify more reliable stock pairs
* Detect complex, nonlinear relationships
* Improve short-term price reversion predictions
* Make trading decisions more accurate and data-driven

Our motivation is to explore how finance and machine learning can work together to build more robust trading strategies.

---

## 3. Challenges

* Stock market data is noisy and changes quickly, making stable pairs difficult to find.
* Classical cointegration methods may fail when stock relationships shift over time.
* Machine learning models risk overfitting and detecting patterns that are not meaningful.
* Ensuring clean, synchronized OHLCV data across all selected tickers is essential.
* Understanding financial strategies like pairs trading can be challenging without a finance background.
* Defining good labels for ML introduces class imbalance and tuning difficulties.
* Converting ML predictions into actionable trading signals requires careful parameter choices.

---

## 4. Dataset

We will use the **S&P 500 Stocks Daily Prices** dataset from Kaggle.
It contains daily OHLCV data including:

* Date
* Open
* High
* Low
* Close
* Volume
* Ticker

No new data will be collected, which keeps the project manageable within the timeline.

---

## 5. Proposed Method

### Overview

We combine classical econometric tests for linear relationships with machine learning models for nonlinear pattern discovery. The project includes data preprocessing, feature engineering, cointegration-based baseline modeling, ML-based pair selection, supervised classification, and backtesting.

---

## 6. Workflow and Stages

### **Stage 1 – Data Preprocessing**

* Load and clean the raw CSV dataset
* Handle missing values and remove duplicates
* Subset 30–50 stocks for manageable computation
* Save as `/data/processed/prices.parquet`

### **Stage 2 – Feature Engineering**

* Compute returns, log returns
* Rolling mean and standard deviation
* RSI and 5–8 additional technical indicators
* Save the feature matrix
* Plot to confirm features look reasonable

### **Stage 3 – Baseline Cointegration Strategy**

* Apply the Engle–Granger cointegration test to all stock pairs
* Identify cointegrated pairs
* Compute spread and Z-score
* Implement basic mean reversion rules:

  * Enter short if Z > 2
  * Exit if Z < 0
* Backtest and record:

  * Profit and loss
  * Sharpe ratio
  * Drawdowns

### **Stage 4 – ML Discovery (Autoencoder + Clustering)**

* Normalize log returns
* Train an autoencoder to learn nonlinear embeddings
* Use KMeans clustering to group similar stocks
* Visualize with PCA or t-SNE
* Select candidate pairs from the same cluster

### **Stage 5 – Labeling and Supervised Learning**

* Create the target variable:

  * Label = 1 if spread returns to mean within 5–10 days
  * Label = 0 otherwise
* Handle class imbalance if needed
* Train Random Forest or XGBoost classifier
* Optimize using Particle Swarm Optimization (PSO)

### **Stage 6 – Evaluation and Backtesting**

* Evaluate classification metrics
* Backtest trading signals based on predicted probabilities
* Compare ML model results with the baseline strategy
* Use SHAP values for model interpretability
