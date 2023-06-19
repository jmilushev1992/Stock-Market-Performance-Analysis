# Stock-Market-Performance-Analysis
This project performs a stock market performance analysis for the last 3 months using Python. It retrieves stock data for selected companies, calculates moving averages, calculates volatility, and visualizes the data using Plotly.

# Installation- To run this project, ensure that you have the following dependencies installed:
pandas
yfinance
datetime
plotly
plotly_express

# You can install these dependencies using pip:
pip install pandas yfinance plotly plotly_express

# Import the required libraries:
<br>import pandas as pd
<br>import yfinance as yf
<br>from datetime import datetime>
<br>import plotly.express as px
# Set the start and end dates for the analysis:
start_date = datetime.now() - pd.DateOffset(months=3)
end_date = datetime.now()

# Define the list of tickers for the companies you want to analyze:
tickers = ['AAPL', 'MSFT', 'NFLX', 'GOOG']
# Retrieve stock data for the specified tickers and store it in a list:
df_list = []
for ticker in tickers:
    data = yf.download(ticker, start=start_date, end=end_date)
    df_list.append(data)
    
# Concatenate the dataframes and add a multi-level index:
df = pd.concat(df_list, keys=tickers, names=['Ticker', 'Date'])

# Print the head of the dataframe:
print(df.head())

# Reset the index of the dataframe:
df = df.reset_index()

# Visualize the stock market performance using line and area charts:
fig = px.line(df, x='Date', y='Close', color='Ticker', title="Stock Market Performance for the Last 3 Months")
fig.show()

fig = px.area(df, x='Date', y='Close', color='Ticker', facet_col='Ticker',
              labels={'Date':'Date', 'Close':'Closing Price', 'Ticker':'Company'},
              title='Stock Prices for Apple, Microsoft, Netflix, and Google')
fig.show()

# Calculate and visualize the moving averages:
df['MA10'] = df.groupby('Ticker')['Close'].rolling(window=10).mean().reset_index(0, drop=True)
df['MA20'] = df.groupby('Ticker')['Close'].rolling(window=20).mean().reset_index(0, drop=True)

<br>for ticker, group in df.groupby('Ticker'):
    print(f'Moving Averages for {ticker}')
    print(group[['MA10', 'MA20']])
    fig = px.line(group, x='Date', y=['Close', 'MA10', 'MA20'], 
                  title=f"{ticker} Moving Averages")
    fig.show()
# Calculate and visualize the volatility:
df['Volatility'] = df.groupby('Ticker')['Close'].pct_change().rolling(window=10).std().reset_index(0, drop=True)
fig = px.line(df, x='Date', y='Volatility', 
              color='Ticker', 
              title='Volatility of All Companies')
fig.show()

# Perform correlation analysis between two companies:
apple = df.loc[df['Ticker'] == 'AAPL', ['Date', 'Close']].rename(columns={'Close': 'AAPL'})
microsoft = df.loc[df['Ticker'] == 'MSFT', ['Date', 'Close']].rename(columns={'Close': 'MSFT'})
df_corr = pd.merge(apple, microsoft, on='Date')

# create a scatter plot to visualize the correlation
fig = px.scatter(df_corr, x='AAPL', y='MSFT', 
                 trendline='ols', 
                 title='Correlation between Apple and Microsoft')
fig.show()
