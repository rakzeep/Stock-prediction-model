# Stock Market Prediction Model

A machine learning-based stock market prediction project that uses historical S&P 500 data to predict whether the market will move upward or downward the following day.

The project uses financial time-series data, feature engineering techniques, and a Random Forest Classification model to analyze historical market trends and generate predictions.

## Overview

Predicting stock market movements is a challenging problem due to the complexity and volatility of financial markets. This project explores how machine learning models can identify patterns from historical market data and use them to classify future market direction.

The model predicts:

- `1` → The S&P 500 closing price is expected to increase the next day
- `0` → The S&P 500 closing price is expected to decrease the next day

## Technologies Used

- Python
- Jupyter Notebook
- Pandas
- NumPy
- Scikit-learn
- Matplotlib
- yFinance

## Dataset

The project uses historical S&P 500 index data obtained using the Yahoo Finance API through the `yfinance` library.

The dataset contains:

| Feature | Description |
|---|---|
| Open | Opening price of the index |
| High | Highest price during the trading day |
| Low | Lowest price during the trading day |
| Close | Closing price of the index |
| Volume | Total trading volume |
| Date | Trading date |

The dataset contains historical market data from 1927 onwards, with model training focused on data after 1990.

## Data Collection

The project automatically downloads S&P 500 historical data using:

```python
yfinance.Ticker("^GSPC")
```

The data is stored locally as:

```
sp500.csv
```

to avoid repeatedly downloading the dataset.

## Data Processing

The dataset is prepared by:

- Converting dates into proper datetime format
- Removing unnecessary columns such as dividends and stock splits
- Creating prediction targets
- Generating additional technical indicators

## Target Creation

The model predicts the next day's market movement.

A new column called `Tomorrow` is created:

```python
sp500["Tomorrow"] = sp500["Close"].shift(-1)
```

The target variable is generated:

```python
sp500["Target"] = (sp500["Tomorrow"] > sp500["Close"]).astype(int)
```

If tomorrow's closing price is higher than today's:

```
Target = 1
```

Otherwise:

```
Target = 0
```

## Machine Learning Model

The project uses a Random Forest Classifier.

Random Forest is an ensemble learning algorithm that combines multiple decision trees to improve prediction accuracy and reduce overfitting.

Initial model:

```python
RandomForestClassifier(
    n_estimators=100,
    min_samples_split=100,
    random_state=1
)
```

Final model:

```python
RandomForestClassifier(
    n_estimators=200,
    min_samples_split=50,
    random_state=1
)
```

## Features Used

### Basic Market Features

The first model uses:

- Opening price
- Highest price
- Lowest price
- Closing price
- Trading volume

### Engineered Features

Additional predictors were created using rolling averages:

#### Close Price Ratios

Measures the current closing price compared to historical averages.

Examples:

- 2-day average
- 5-day average
- 60-day average
- 250-day average
- 1000-day average


#### Market Trends

Tracks how often the market increased over previous periods.

Example:

```
Trend_60
```

represents the number of positive market movements over the previous 60 trading days.

## Model Evaluation

The model was evaluated using precision score.

Precision measures:

"Out of all the upward movements predicted by the model, how many were actually correct?"

Formula:

```
Precision = True Positive / (True Positive + False Positive)
```

## Results

### Initial Model

Using basic market features:

```
Precision: ~53%
```

### Improved Model

After adding engineered features:

```
Precision: ~57.7%
```

The improved model showed better performance by incorporating historical trends and moving average relationships.

## Backtesting

Instead of testing on only one section of data, the project uses a backtesting approach.

The model:

1. Trains on historical data
2. Predicts future unseen data
3. Moves forward in time
4. Retrains using additional historical information

This better represents how a model would perform in a real-world scenario.

## Example Prediction Output

The model produces:

| Date | Actual Movement | Prediction |
|---|---|---|
| 2003-11-14 | Down | Down |
| 2003-11-18 | Up | Up |
| 2026-07-08 | Up | Down |

Where:

```
1 = Market goes up
0 = Market goes down
```

## Project Structure

```
Stock-prediction-model/

│
├── stock-analyzer.ipynb
├── sp500.csv
└── README.md
```

## Installation

Clone the repository:

```bash
git clone https://github.com/rakzeep/Stock-prediction-model.git
```

Install dependencies:

```bash
pip install pandas numpy matplotlib scikit-learn yfinance
```

Run the notebook:

```bash
jupyter notebook stock-analyzer.ipynb
```

## How To Use

1. Open the Jupyter Notebook.
2. Run the data collection section.
3. Train the Random Forest model.
4. View predictions and evaluation metrics.
5. Experiment with different features and model parameters.

## Limitations

- Stock markets are influenced by many external factors such as news, economic conditions, and investor sentiment.
- Historical patterns do not guarantee future performance.
- The model predicts market direction, not exact future prices.
- This project is for educational purposes and should not be used as financial advice.

## Future Improvements

Possible extensions:

- Add technical indicators such as RSI, MACD, and Bollinger Bands
- Include economic indicators and news sentiment analysis
- Compare multiple ML models:
  - Logistic Regression
  - XGBoost
  - Neural Networks
  - LSTM models
- Create a dashboard for real-time predictions
- Add portfolio optimization features
