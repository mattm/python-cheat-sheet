# Time Series

## Creating a basic time series chart

```python
df = pd.read_csv("data.csv", names=["sign_up_month", "new_trials"], header=0, index_col="sign_up_month")
df = df.sort_values("sign_up_month")
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
ax.set(title="Sign Ups by Month", ylabel="New Trials", xlabel="Sign Up Month")
ax.get_legend().remove()
plt.show()
```
