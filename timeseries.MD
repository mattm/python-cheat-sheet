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
df = pd.read_csv("help_scout companies 2020-07-15T1629.csv", names=["close_month", "new_customers", "new_mrr"], header=0, index_col="close_month")

# Arguments are number of rows, number of columns
fig, ax = plt.subplots(2, 1)
df[["new_customers"]].plot(ax=ax[0])
df[["new_mrr"]].plot(ax=ax[1])
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
