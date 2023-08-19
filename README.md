Stock Market Performance Analysis
This project performs a stock market performance analysis for the last 3 months using Python. It includes the following steps:

Retrieve stock data for selected companies
Calculate moving averages
Calculate volatility
Visualize the data using Plotly
Installation
To run this project, ensure that you have the following dependencies installed:

pandas
yfinance
datetime
plotly
plotly_express
You can install these dependencies using pip:

bash
Copy code
pip install pandas yfinance plotly plotly_express
Import Libraries
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
Retrieve Stock Data
Retrieve stock data for the specified tickers and store it in a list:

python
Copy code
df_list = []
for ticker in tickers:
    data = yf.download(ticker, start=start_date, end=end_date)
    df_list.append(data)

df = pd.concat(df_list, keys=tickers, names=['Ticker', 'Date'])
Visualize Data
Print the head of the dataframe:

python
Copy code
print(df.head())
Reset the index of the dataframe:

python
Copy code
df = df.reset_index()
Visualize the stock market performance using line and area charts:

python
Copy code
fig = px.line(df, x='Date', y='Close', color='Ticker', title="Stock Market Performance for the Last 3 Months")
fig.show()

fig = px.area(df, x='Date', y='Close', color='Ticker', facet_col='Ticker', labels={'Date':'Date', 'Close':'Closing Price', 'Ticker':'Company'}, title='Stock Prices for Apple, Microsoft, Netflix, and Google')
fig.show()
Moving Averages
Calculate and visualize the moving averages:

python
Copy code
df['MA10'] = df.groupby('Ticker')['Close'].rolling(window=10).mean().reset_index(0, drop=True)
df['MA20'] = df.groupby('Ticker')['Close'].rolling(window=20).mean().reset_index(0, drop=True)

for ticker, group in df.groupby('Ticker'):
    print(f'Moving Averages for {ticker}')
    print(group[['MA10', 'MA20']])
    fig = px.line(group, x='Date', y=['Close', 'MA10', 'MA20'], title=f"{ticker} Moving Averages")
    fig.show()
Volatility
Calculate and visualize the volatility:

python
Copy code
df['Volatility'] = df.groupby('Ticker')['Close'].pct_change().rolling(window=10).std().reset_index(0, drop=True)
fig = px.line(df, x='Date', y='Volatility', color='Ticker', title='Volatility of All Companies')
fig.show()
Correlation Analysis
Perform correlation analysis between two companies:

python
Copy code
apple = df.loc[df['Ticker'] == 'AAPL', ['Date', 'Close']].rename(columns={'Close': 'AAPL'})
microsoft = df.loc[df['Ticker'] == 'MSFT', ['Date', 'Close']].rename(columns={'Close': 'MSFT'})
df_corr = pd.merge(apple, microsoft, on='Date')

# Create a scatter plot to visualize the correlation
fig = px.scatter(df_corr, x='AAPL', y='MSFT', trendline='ols', title='Correlation between Apple and Microsoft')
fig.show()
