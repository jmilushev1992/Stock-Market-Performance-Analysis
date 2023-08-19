# Stock Market Performance Analysis

This project performs a stock market performance analysis for the last 3 months using Python. It retrieves stock data for selected companies, calculates moving averages, calculates volatility, and visualizes the data using Plotly.

## Installation

To run this project, ensure that you have the following dependencies installed:

- pandas
- yfinance
- datetime
- plotly
- plotly_express

You can install these dependencies using pip:

```bash
pip install pandas yfinance plotly plotly_express
Import the Required Libraries
python
Copy code
import pandas as pd
import yfinance as yf
from datetime import datetime
import plotly.express as px
Set Dates
Set the start and end dates for the analysis:

python
Copy code
start_date = datetime.now() - pd.DateOffset(months=3)
end_date = datetime.now()
Define Tickers
Define the list of tickers for the companies you want to analyze:

python
Copy code
tickers = ['AAPL', 'MSFT', 'NFLX', 'GOOG']
