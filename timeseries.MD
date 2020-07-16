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
