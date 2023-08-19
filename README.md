<!DOCTYPE html>
<html>
<head>
   
</head>
<body>

<h1>Stock Market Performance Analysis</h1>
<p>This project performs a stock market performance analysis for the last 3 months using Python. It includes the following steps:</p>

<ul>
    <li>Retrieve stock data for selected companies</li>
    <li>Calculate moving averages</li>
    <li>Calculate volatility</li>
    <li>Visualize the data using Plotly</li>
</ul>

<h2>Installation</h2>
<p>To run this project, ensure that you have the following dependencies installed:</p>

<ul>
    <li>pandas</li>
    <li>yfinance</li>
    <li>datetime</li>
    <li>plotly</li>
    <li>plotly_express</li>
</ul>

<p>You can install these dependencies using pip:</p>

<pre><code>pip install pandas yfinance plotly plotly_express</code></pre>

<h2>Import Libraries</h2>

<pre><code>
    import pandas as pd
    import yfinance as yf
    from datetime import datetime
    import plotly.express as px
</code></pre>

<h2>Set Dates</h2>

<pre><code>start_date = datetime.now() - pd.DateOffset(months=3)
end_date = datetime.now()
</code></pre>

<h2>Define Tickers</h2>

<pre><code>tickers = ['AAPL', 'MSFT', 'NFLX', 'GOOG']
</code></pre>

<h2>Retrieve Stock Data and Visualize</h2>
    <pre><code class="language-python">
df_list = []
for ticker in tickers:
    data = yf.download(ticker, start=start_date, end=end_date)
    df_list.append(data)

df = pd.concat(df_list, keys=tickers, names=['Ticker', 'Date'])

print(df.head())

df = df.reset_index()

fig = px.line(df, x='Date', y='Close', color='Ticker', title="Stock Market Performance for the Last 3 Months")
fig.show()

fig = px.area(df, x='Date', y='Close', color='Ticker', facet_col='Ticker', labels={'Date':'Date', 'Close':'Closing Price', 'Ticker':'Company'}, title='Stock Prices for Apple, Microsoft, Netflix, and Google')
fig.show()
    </code></pre>

<h2>Calculate and Visualize Moving Averages</h2>
    <pre><code>
df['MA10'] = df.groupby('Ticker')['Close'].rolling(window=10).mean().reset_index(0, drop=True)
df['MA20'] = df.groupby('Ticker')['Close'].rolling(window=20).mean().reset_index(0, drop=True)

for ticker, group in df.groupby('Ticker'):
    print(f'Moving Averages for {ticker}')
    print(group[['MA10', 'MA20']])
    fig = px.line(group, x='Date', y=['Close', 'MA10', 'MA20'], title=f"{ticker} Moving Averages")
    fig.show()
    </code></pre>

<h2>Volatility</h2>
    <pre><code class="language-python">
df['Volatility'] = df.groupby('Ticker')['Close'].pct_change().rolling(window=10).std().reset_index(0, drop=True)
fig = px.line(df, x='Date', y='Volatility', color='Ticker', title='Volatility of All Companies')
fig.show()
</code></pre>

<h2>Correlation Analysis</h2>
    <pre><code class="language-python">
apple = df.loc[df['Ticker'] == 'AAPL', ['Date', 'Close']].rename(columns={'Close': 'AAPL'})
microsoft = df.loc[df['Ticker'] == 'MSFT', ['Date', 'Close']].rename(columns={'Close': 'MSFT'})
df_corr = pd.merge(apple, microsoft, on='Date')
    </code></pre>

<h2>Create a scatter plot to visualize the correlation</h2>
<pre><code>
fig = px.scatter(df_corr, x='AAPL', y='MSFT', trendline='ols', title='Correlation between Apple and Microsoft')
fig.show()
</code></pre>

</body>
</html>
