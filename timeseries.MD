# Time Series

## Visualizing

```python
df = pd.read_csv("data.csv", names=["sign_up_month", "new_trials"], header=0, index_col="sign_up_month")
df = df.sort_values("sign_up_month")

df.plot(legend=None)
plt.title("Sign Ups by Month")
plt.xlabel("Sign Up Month")
plt.ylabel("New Trials")
plt.show()
```
