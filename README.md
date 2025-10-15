# SPY-historical-analysis-with-Python

import datetime as dt
import yfinance as yf  # type: ignore
import numpy as np  # type: ignore
import matplotlib.pyplot as plt  # type: ignore

ticker = "SPY"
end = dt.datetime.now()
data = yf.download(ticker, start="2000-01-01", end=end)
Adj_cl = data['Close']

# Log returns cumulés
log_return = Adj_cl.pct_change().apply(lambda x: np.log(1 + x)).cumsum()
# Volatilité journalière
rolling_vol = log_return.rolling(window= 21).std() 
# Drowdawn
drowdawn = Adj_cl / Adj_cl.cummax() - 1


# Visualisation of returns
plt.figure(figsize=(10, 6))
plt.plot(log_return)
plt.title("Log Returns of SPY")
plt.xlabel("Date")
plt.ylabel("Log Returns")
plt.grid()
plt.show()

# Visualisation of volatility
plt.figure(figsize=(10, 6))
plt.plot(rolling_vol)
plt.title("Daily volatility of SPY")
plt.xlabel("Date")
plt.ylabel("Daily Vol")
plt.grid()
plt.show()

# Visualisation of drawdown
plt.figure(figsize=(10,6))
plt.plot(drowdawn)
plt.title("Drawdown of SPY")
plt.xlabel("Date")
plt.ylabel("Drawdown")
plt.grid()
plt.show()
