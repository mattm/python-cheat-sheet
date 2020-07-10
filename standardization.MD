# Standardization

## Standard Scaler

```python
from sklearn.preprocessing import StandardScaler

df = pd.DataFrame({
    'col1': [1, 2, 3],
    'col2': [10, 20, 30],
    'col3': [0, 20, 22]
})
print("Original:\n")
print(df)
print("\nColumn means:\n")
print(df.mean())
print("\nOriginal variance:\n")
print(df.var(ddof=0))
print("\nOriginal standard deviations:\n")
print(df.std(ddof=0))

scaler = StandardScaler()
df1 = pd.DataFrame(scaler.fit_transform(df), columns=df.columns)
print("\nAfter scaling:\n")
print(df1)
print("\nColumn means:\n")
print(round(df1.mean(), 3))
print("\nVariance:\n")
print(df1.var(ddof=0))
print("\nStandard deviations:\n")
print(df1.std(ddof=0))

print("\nExample calculation for col2:")
print("z = (x - mean) / std")
print("z = (10 - 20) / 8.164966 = -1.224745")

```

Note that `DataFrame.var` and `DataFrame.std` default to using 1 degree of freedom (`ddof=1`) but StandardScaler is using `numpy`'s versions which default to `ddof=0`. That's why when printing the variance and standard deviation of the original data frame, we're specifying `ddof=0`. `ddof=1` is known as [Bessel's correction](https://en.wikipedia.org/wiki/Bessel%27s_correction).

```
Original:

   col1  col2  col3
0     1    10     0
1     2    20    20
2     3    30    22

Column means:

col1     2.0
col2    20.0
col3    14.0
dtype: float64

Original variance:

col1     0.666667
col2    66.666667
col3    98.666667
dtype: float64

Original standard deviations:

col1    0.816497
col2    8.164966
col3    9.933110
dtype: float64

After scaling:

       col1      col2      col3
0 -1.224745 -1.224745 -1.409428
1  0.000000  0.000000  0.604040
2  1.224745  1.224745  0.805387

Column means:

col1    0.0
col2    0.0
col3    0.0
dtype: float64

Variance:

col1    1.0
col2    1.0
col3    1.0
dtype: float64

Standard deviations:

col1    1.0
col2    1.0
col3    1.0
dtype: float64

Example calculation for col2:
z = (x - mean) / std
z = (10 - 20) / 8.164966 = -1.224745
```

Notice how the scaled columns each have a mean of 0 and a variance of 1.

## Log

```python
wine['Proline_log'] = np.log(wine['Proline'])
```

Remember this is log base `e`, not log base 10.
