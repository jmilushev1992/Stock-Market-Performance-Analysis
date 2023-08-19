<!DOCTYPE html>
<html>
<head>
    <title>Stock Market Performance Analysis</title>
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

<pre><code>&lt;script type="text/javascript"&gt;
    import pandas as pd
    import yfinance as yf
    from datetime import datetime
    import plotly.express as px
&lt;/script&gt;
</code></pre>

<h2>Set Dates</h2>

<pre><code>start_date = datetime.now() - pd.DateOffset(months=3)
end_date = datetime.now()
</code></pre>

<h2>Define Tickers</h2>

<pre><code>tickers = ['AAPL', 'MSFT', 'NFLX', 'GOOG']
</code></pre>
