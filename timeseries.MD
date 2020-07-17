# Time Series

## Creating a basic time series chart

```python
df = pd.read_csv("data.csv", names=["sign_up_month", "new_trials"], header=0, index_col="sign_up_month")
```

Method 1:

```python
df.plot(legend=None)
plt.title("Sign Ups by Month")
plt.xlabel("Sign Up Month")
plt.ylabel("New Trials")
plt.show()
```

Method 2:

```python
fig, ax = plt.subplots()
df.plot(ax=ax)
ax.set(title="Sign Ups by Month", xlabel="Sign Up Month", ylabel="New Trials")
ax.get_legend().remove()
plt.show()
```

Can plot multiple charts using this second method:

```python
# We'll plot two columns from one data frame in two separate charts
# but can easily plot a different data frame on each chart
df = pd.read_csv("data.csv", names=["close_month", "new_customers", "new_mrr"], header=0, index_col="close_month")

# Arguments are number of rows, number of columns
# You can also adjust the size of the charts by passing an argument to `subplots` like `figsize=(6, 8)`
# `plot` can also take a `x` argument if the x values are not the index
fig, ax = plt.subplots(2, 1)
df.plot(y="new_customers", ax=ax[0])
df.plot(y="new_mrr", ax=ax[1])
plt.show()
```

## Subsetting when the index is a datetime

```python
df = pd.DataFrame({
    'stock_price': ['2018-01-01', '2019-01-01', '2020-01-01'],
    'value': [10, 25, 50]
})
df = df.set_index('stock_price')
df.index = pd.to_datetime(df.index)

# Subset the data frame to only include rows where the index reflects the year 2019
df['2019']
```

```
             value
stock_price       
2019-01-01      25
```

## Resampling

For example, if your data frame has daily values but you want monthly:

```python
df = pd.DataFrame({
    "date": pd.to_datetime(["2020-01-05", "2020-01-06", "2020-02-10"]),
    "revenue": [10, 18, 3]
})
df = df.set_index("date")
df = df.resample(rule="MS").sum()
df
```

```
            revenue
date               
2020-01-01       28
2020-02-01        3
```

Here `MS` is "month start" but could easily use `M` for "month end" or [other date offsets](https://pandas.pydata.org/pandas-docs/stable/user_guide/timeseries.html#dateoffset-objects).

## Autocorrelation

```python
df["pct_change"] = df["col"].pct_change()
df["pct_change"].autocorr()
```

```
-0.040576
```

* Autocorrelation (aka serial correlation) is the correlation of a time series with a lagged copy of itself
* It's lag-one autocorrelation. For example, with daily time series we'd be comparing one day's value to the value the previous day.
* When autocorrelation is negative, we say the series is _mean reverting_ (when it goes up, it later tends to go down)
* When autocorrelation is positive, we say the series is _trend following_ or that it has _momentum_ (when it goes up, it later tends to keep going up)

## Autocorrelation function

This lets us look at the a bunch of correlations, not just the lag-one correlation.

```python
from statsmodels.graphics.tsaplots import plot_acf
from statsmodels.tsa.stattools import acf

series = pd.Series([1, 2, 10, 1, 2, 10])

_ = plot_acf(series, lags=3, alpha=1)

acf_values = acf(series, nlags=3, fft=False)
print(acf_values)
```

```
[ 1.         -0.30593607 -0.44406393  0.5       ]
```

* At 0 lags, the autocorrelation is always 1 because a value is always perfectly correlated with itself
* At 3 lags, the autocorrelation here is 0.5 (not 1) because the first 3 values out of the 6 don't have a lag-three.

## Random walks

* In a random walk, today's price is equal to yesterday's price plus some noise (`eta`)
* You can't forecast a random walk; best guess is the last price
* There can also be _drift_: the price changes by some amount on average (`mu`) in addition to the white noise
* To test whether a series is a random walk, you can regress current prices on lag prices. If the slope coefficient `beta` is not significantly different than 1, then we cannot reject the null hypothesis that the series is a random walk.

## Stationary vs non-stationary time series

```python
from statsmodels.tsa.stattools import adfuller
import pandas as pd
import numpy as np

def interpret(p_value):
    if p_value < 0.05:
        return "  p-value < 0.05 so we reject the null hypothesis because the data does not have a unit root and is stationary"
    else:
        return "  p-value >= 0.05 so we fail to reject the null hypothesis because the data has a unit root and is non-stationary"

print("Null hypothesis: series is non-stationary")
print("Alternative hypothesis: series is stationary\n")

# Stationary: mean and standard deviation of the observations are consistent over time
# It does NOT have a unit root meaning that it is predictable
stationary = np.random.normal(loc=10, scale=1, size=1000)
p_value = adfuller(stationary)[1]
print("p-value for observations pulled from normal distribution: {:2f}".format(p_value))
print(interpret(p_value))

# Non-stationary: the observations are impacted by seasonality, trends, and other things that time impacts
# It does have a unit root meaning it is unpredictable
randomness = np.random.normal(loc=0, scale=1, size=1000)
randomness[0] = 0
nonstationary = 100 + np.cumsum(randomness)
p_value = adfuller(nonstationary)[1]
print("\np-value for random walk: {:2f}".format(p_value))
print(interpret(p_value))

nonstationary_stationary = pd.Series(nonstationary).diff().dropna()
p_value = adfuller(nonstationary_stationary)[1]
print("\np-value for random walk diff: {:2f}".format(p_value))
print(interpret(p_value))

fig, ax = plt.subplots(3, 1, figsize=(6, 6))
fig.set_facecolor("white")
fig.tight_layout(pad=3)

ax[0].set(title="Stationary:")
ax[0].plot(stationary)

ax[1].set(title="Non-stationary:")
ax[1].plot(nonstationary)

ax[2].set(title="Non-stationary made stationary:")
ax[2].plot(nonstationary_stationary)
plt.show()
```

```
Null hypothesis: series is non-stationary
Alternative hypothesis: series is stationary

p-value for observations pulled from normal distribution: 0.000000
  p-value < 0.05 so we reject the null hypothesis because the data does not have a unit root and is stationary

p-value for random walk: 0.936132
  p-value >= 0.05 so we fail to reject the null hypothesis because the data has a unit root and is non-stationary

p-value for random walk diff: 0.000000
  p-value < 0.05 so we reject the null hypothesis because the data does not have a unit root and is stationary
```

![](https://github.com/mattm/python-cheat-sheet/blob/master/images/stationary-vs-nonstationary-3.png?raw=true)

Other notes:

* To take an exponential trend with seasonality (like Amazon's quarterly earnings) you could take a log of it (to make it linear) then diff-4 (to remove the seasonality).

## ARMA Process

Code and charts here via [DataCamp's time series course](https://campus.datacamp.com/courses/time-series-analysis-in-python/autoregressive-ar-models?ex=2).

* AR stands for Autoregressive. Regressive (you're trying to predict something from other things) and auto (you're trying to predict based on past values of that same thing). Typically when doing regression we're predicting based on other things, but we may be able to predict values based on what the value was last week/month/year etc.
* An AR(1) model will be based on one laggard value. Can also have AR(2), AR(3), etc.
* `Rt = mu + phi * Rt-1 + eta`. If `phi` is 1, then the process is a random walk because the next value in the series is very dependent on the last value. If `phi` is 0, then the process is white noise because it's not dependent at all on the last value. If `phi` is between -1 and 1 then the process is stationary. Otherwise (if it's less than -1 or greater than 1) then it's non-stationary. Negative `phi` values are _mean reverting_ (a positive value is more likely to be followed by a negative value). A positive value means a positive value is more likely to be followed by a positive value (it has _momentum_ and is _trend following_).

#### Autoregressive time series with different values of phi

![](https://github.com/mattm/python-cheat-sheet/blob/master/images/autoregression-timeseries-phi.png?raw=true)

#### Autocorrelation function with different values of phi

![](https://github.com/mattm/python-cheat-sheet/blob/master/images/autoregression-functions-phi.png?raw=true)

#### Simulating a autoregressive trend

```python
from statsmodels.tsa.arima_process import ArmaProcess

plt.subplot(2,1,1)
ar1 = np.array([1, -0.9])
ma1 = np.array([1])
AR_object1 = ArmaProcess(ar1, ma1)
simulated_data_1 = AR_object1.generate_sample(nsample=1000)
plt.plot(simulated_data_1)
plt.show()
```

* ARMA stands for Autoregressive-moving-average
* First argument to `ArmaProcess` is `ar`. The AR part involves regressing the variable on its own lagged (i.e., past) values ([#](https://en.wikipedia.org/wiki/Autoregressive%E2%80%93moving-average_model)). It's an array that represents the coefficient for autoregressive lag polynomial, including zero lag (which is almost always 1). Also, due to using the lag-polynomial representation, the AR parameters should have the opposite sign of what one would write in the ARMA representation ([#](https://www.statsmodels.org/stable/generated/statsmodels.tsa.arima_process.ArmaProcess.html)). So if you want the AR parameter to be 0.9, you have to use -0.9 in the array.
* Second argument is `ma`. The MA part involves modeling the error term as a linear combination of error terms occurring contemporaneously and at various times in the past. It's an array that represents the coefficients for moving-average lag polynomial, including zero lag.

#### Modeling an autoregressive trend

```python
from statsmodels.tsa.arima_process import ArmaProcess
from statsmodels.tsa.arima_model import ARMA

# Simulate data using a phi of +0.9
AR_object1 = ArmaProcess([1, -0.9], [1])
simulated_data_1 = AR_object1.generate_sample(nsample=1000)

# Now see if we can fit an autoregressive model to the simulated data
model = ARMA(simulated_data_1, order=(1, 0))
result = model.fit()

print(result.summary())

# Print out the estimate for the constant and for phi
print("\nWhen the true phi=0.9, the estimate of phi (and the constant) are:")
print("Mu: {:.2f}".format(result.params[0]))
print("Phi: {:.2f}".format(result.params[1]))
```

```
                              ARMA Model Results                              
==============================================================================
Dep. Variable:                      y   No. Observations:                 1000
Model:                     ARMA(1, 0)   Log Likelihood               -1407.583
Method:                       css-mle   S.D. of innovations              0.988
Date:                Fri, 17 Jul 2020   AIC                           2821.167
Time:                        10:56:26   BIC                           2835.890
Sample:                             0   HQIC                          2826.763
                                                                              
==============================================================================
                 coef    std err          z      P>|z|      [0.025      0.975]
------------------------------------------------------------------------------
const          0.5388      0.273      1.976      0.048       0.004       1.073
ar.L1.y        0.8863      0.015     60.659      0.000       0.858       0.915
                                    Roots                                    
=============================================================================
                  Real          Imaginary           Modulus         Frequency
-----------------------------------------------------------------------------
AR.1            1.1283           +0.0000j            1.1283            0.0000
-----------------------------------------------------------------------------

When the true phi=0.9, the estimate of phi (and the constant) are:
Mu: -0.00
Phi: 0.89
```
