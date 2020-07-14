# Time Series

## Basic Chart

```python
fig, ax = plt.subplots(figsize=(12, 6))
df.plot("sign_up_month", "new_trials", ax=ax)
ax.set(title="Sign Ups by Month", ylabel="New Trials", xlabel="Sign Up Month")
ax.get_legend().remove()
plt.show()
```
