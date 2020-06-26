# Series

## Operators are performed by index

```python
import pandas as pd

data1 = pd.Series([5, 2, 3,7], index=['a', 'b', 'c', 'd'])
# a    5
# b    2
# c    3
# d    7

data2 = pd.Series([10, 11, 12, 13], index=['d', 'c', 'b', 'a'])
# d    10
# c    11
# b    12
# a    13

data1 + data2
# a    18
# b    14
# c    14
# d    17
```

## Sorting

Ascending

```python
data1 = pd.Series([5, 2, 3,7], index=['a', 'b', 'c', 'd'])
print(data1.sort_values())
```
```
b    2
c    3
a    5
d    7
```

Descending

```python
data1 = pd.Series([5, 2, 3,7], index=['a', 'b', 'c', 'd'])
print(data1.sort_values(ascending=False))
```

```
d    7
a    5
c    3
b    2
```

## Viewing value counts

```python
data = pd.Series(["m", "f", "m", "f", "f"])
data.value_counts()
```

```
f    3
m    2
```

## Determining the most common value

```python
data = pd.Series(["m", "f", "m", "f", "f"])
data.mode()[0]
```

```
'f'
```

## Counting number of unique values

```python
data = pd.Series(["m", "f", "m", "f", "f"])
data.nunique()
```

```
2
```

## Casting boolean to int

```python
data = pd.Series([True, False, True])
data.astype(int)
```

```
0    1
1    0
2    1
```

## Replacing uncommon values with "Other"

```python
series = pd.Series(["US", "India", "US", "Australia", "US", "India", "Canada"])

print("Original:")
print(series)

counts = series.value_counts()
print("\nCounts:")
print(counts)

indeces = counts[counts < 2].index
print("\nIndeces:")
print(indeces)

mask = series.isin(indeces)
print("\nMask:")
print(mask)

series[mask] = "Other"
print("\nFinal Series:")
series
```

```
Original:
0           US
1        India
2           US
3    Australia
4           US
5        India
6       Canada
dtype: object

Counts:
US           3
India        2
Australia    1
Canada       1
dtype: int64

Indeces:
Index(['Australia', 'Canada'], dtype='object')

Mask:
0    False
1    False
2    False
3     True
4    False
5    False
6     True
dtype: bool

Final Series:
0       US
1    India
2       US
3    Other
4       US
5    India
6    Other
dtype: object
```

## Replacing charcters

```python
series = pd.Series(["10000", "12,000"])
series = series.str.replace(",", "").astype(float)
series
```

```
0    10000.0
1    12000.0
dtype: float64
```
