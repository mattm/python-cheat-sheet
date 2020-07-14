# Time Series

## Basic Chart

Note that the x-axis values must be sorted in the data frame for the chart to render correctly.

```python
fig, ax = plt.subplots(figsize=(12, 6))
df.plot("sign_up_month", "new_trials", ax=ax)
ax.set(title="Sign Ups by Month", ylabel="New Trials", xlabel="Sign Up Month")
ax.get_legend().remove()
plt.show()
```
