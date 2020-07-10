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
print(df.var())

scaler = StandardScaler()
df1 = pd.DataFrame(scaler.fit_transform(df), columns=df.columns)
print("\nAfter scaling:\n")
print(df1)
print("\nColumn means:\n")
print(round(df1.mean(), 3))
print("\nVariance:\n")
print(df1.var())
```

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

col1      1.0
col2    100.0
col3    148.0
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

col1    1.5
col2    1.5
col3    1.5
dtype: float64
```

## Log

```python
wine['Proline_log'] = np.log(wine['Proline'])
```

Remember this is log base `e`, not log base 10.
