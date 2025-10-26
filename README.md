# 📈 Stock Price Prediction using Linear Regression

This project builds a Linear Regression model to predict the next day's closing price for a stock based on its historical daily data. The project serves as a practical introduction to time series forecasting, feature engineering, and model evaluation, highlighting the critical differences between a naive model and a proper forecasting model.

For this analysis, we use 5 years of historical data for **Apple Inc. (AAPL)**.

## Actual vs Predicted Plot
<img width="1131" height="602" alt="forecast_plot" src="https://github.com/user-attachments/assets/370d4c63-8456-4155-8cef-cff84f5923cd" />

---

## 📋 Table of Contents
1.  [Project Overview](#-project-overview)
2.  [Dataset](#-dataset)
3.  [Methodology](#-methodology)
4.  [Results and Evaluation](#-results-and-evaluation)
5.  [Future Improvements](#-future-improvements)
6.  [License](#-license)

---

## 📝 Project Overview

The primary goal of this project is to forecast the closing stock price for the next day. The process involves:
*   **Data Acquisition:** Fetching historical stock data using the `yfinance` library.
*   **Exploratory Data Analysis (EDA):** Visualizing trends, correlations, and distributions to understand the data's characteristics.
*   **Feature Engineering:** Creating a "lagged" target variable to frame the problem as a true forecast, avoiding data leakage.
*   **Modeling:** Building and training a Linear Regression model.
*   **Evaluation:** Assessing the model's performance using metrics like RMSE and R-squared, and visualizing the results.

A key part of this project is the comparison between an initial **naive model** (which suffers from data leakage) and the final, **realistic forecasting model**.

---

## 📊 Dataset

The dataset consists of 5 years of daily stock price information for Apple Inc. (ticker: `AAPL`), sourced from Yahoo Finance.

*   **Source:** Yahoo Finance, accessed via the `yfinance` Python library.
*   **Ticker:** AAPL
*   **Time Period:** Last 5 years
*   **Features:**
    *   `Open`: The price at which the stock first traded upon the opening of an exchange on a given day.
    *   `High`: The highest price at which the stock traded during a day.
    *   `Low`: The lowest price at which the stock traded during a day.
    *   `Close`: The last price at which the stock traded during a day.
    *   `Volume`: The number of shares traded during a day.

---

## ⚙️ Methodology

The project follows these key steps:

#### 1. Data Acquisition
Historical data for AAPL was downloaded using the `yfinance` library.

#### 2. Exploratory Data Analysis (EDA)
*   **Data Integrity:** Checked for and confirmed no null values.
*   **Trend Analysis:** Plotted the `Close` price and `Volume` over time to observe long-term trends and volatility.
*   **Moving Averages:** Visualized the 50-day and 200-day moving averages to better identify market momentum.
*   **Correlation Analysis:** A heatmap of feature correlations revealed an extremely high correlation (nearly 1.0) between `Open`, `High`, `Low`, and `Close` for the same day. This finding was critical in identifying the data leakage issue in the naive model.

#### 3. Modeling - From a Naive Model to a Realistic Forecast

*   **Attempt 1: The Naive (Leaky) Model**
    *   **Approach:** Predict the `Close` price using the *same day's* `Open`, `High`, and `Low`.
    *   **Result:** The model achieved an R-squared score of **~0.999**, suggesting a near-perfect fit.
    *   **Conclusion:** This result was due to **data leakage**. In a real-world scenario, you wouldn't know the day's high and low prices before the day ends. This model was not a true forecast.

*   **Attempt 2: The Realistic Forecasting Model**
    *   **Feature Engineering:** The problem was reframed to predict the *next day's* closing price. This was achieved by creating a new target column, `Prediction`, by shifting the `Close` price column by one day (`df['Close'].shift(-1)`).
    *   **Data Splitting:** The data was split **chronologically** into a training set (first 80%) and a testing set (most recent 20%). This is crucial for time series data to prevent the model from seeing future data during training.
    *   **Model Training:** A `LinearRegression` model was trained on this properly structured data.

---

## 📈 Results and Evaluation

The realistic forecasting model was evaluated on the test set (the most recent ~1 year of data).

*   **Root Mean Squared Error (RMSE):** 4.60
    *   This means, on average, the model's prediction for the next day's closing price was off by about $2.85.
*   **R-squared (R²):** 0.94
    *   This indicates that the model explains about 98.2% of the variance in the stock price, showing it follows the general trend very well.

**Analysis of Predictions:**
The visual comparison plot shows that the predicted price line closely follows the actual price line but with a one-day lag. This is typical for a simple linear model, which has learned that **the best predictor for tomorrow's price is today's price**. While simple, this provides a solid baseline for performance.

---

## 💡 Future Improvements

This project can be extended in several ways:

*   **Advanced Models:** Implement more sophisticated time series models like ARIMA, SARIMA, or neural networks (LSTMs, GRUs) which can capture more complex temporal patterns.
*   **Additional Features:** Engineer new features, such as:
    *   **Technical Indicators:** RSI, MACD, Bollinger Bands.
    *   **Lagged Features:** Use prices from the last 5, 10, or 30 days as input.
*   **Sentiment Analysis:** Incorporate sentiment data from financial news headlines or social media to capture market mood.
*   **Hyperparameter Tuning:** Optimize model parameters using techniques like GridSearchCV.

---

## 📜 License

This project is licensed under the MIT License.
