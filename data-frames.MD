# Data Frames

## Building manually

With a default interger indeces:

```python
import pandas as pd

data = { 
  'month': ['2020-04-01', '2020-05-01'], 
  'revenue': [1234, 5678],
  'users': [68, 84]
}
df = pd.DataFrame(data)

        month  revenue  users
0  2020-04-01     1234     68
1  2020-05-01     5678     84
```

With named indeces:

```python
import pandas as pd
data = {
  'revenue': [1234, 5678],
  'users': [68, 84] 
}
df = pd.DataFrame(data)
df.index = ['2020-04-01', '2020-05-01']

            revenue  users
2020-04-01     1234     68
2020-05-01     5678     84
```

## Determining a data frame's dimensions

Returns a tuple with the rows and columns.

```python
rows, columns = df.shape
```

## Determining column names

```python
df.columns
```

## Determining column types

```python
df.info()

<class 'pandas.core.frame.DataFrame'>
RangeIndex: 2 entries, 0 to 1
Data columns (total 3 columns):
 #   Column   Non-Null Count  Dtype 
---  ------   --------------  ----- 
 0   month    2 non-null      object
 1   revenue  2 non-null      int64 
 2   users    2 non-null      int64 
dtypes: int64(2), object(1)
memory usage: 176.0+ bytes
```

## Viewing statistics about numeric columns

```python
df.describe()

           revenue      users
count     2.000000   2.000000
mean   3456.000000  76.000000
std    3142.382536  11.313708
min    1234.000000  68.000000
25%    2345.000000  72.000000
50%    3456.000000  76.000000
75%    4567.000000  80.000000
max    5678.000000  84.000000
```

## Importing from a CSV

```python
import pandas as pd
df = pd.read_csv("data.csv")
```

## Selecting a column as a data frame

```python
df[['revenue']]

            revenue
2020-04-01     1234
2020-05-01     5678
```

Or multiple:

```python
df[['revenue', 'users']]

            revenue  users
2020-04-01     1234     68
2020-05-01     5678     8

# or

df.loc[:, ['revenue', 'users']]

            revenue  users
2020-04-01     1234     68
2020-05-01     5678     84
```

## Get list from column of Pandas dataframe

```python
df['users'].tolist()
```

## Get array from column of Pandas dataframe

```python
df['users'].values

# or

df['users'].to_numpy()
```

## Selecting rows

All columns for a row by its label:

```python
df.loc[['2020-04-01']]

            revenue  users
2020-04-01     1234     68
```

Or specific columns for a row by its label:

```python
df.loc[['2020-04-01'], ['users']]

            users
2020-04-01     68
```

Or all columns for a row by its index:

```python
df.iloc[[1]]

            revenue  users
2020-05-01     5678     84
```

Or by a condition:

```python
df[df['revenue'] > 3000]

            revenue  users
2020-05-01     5678     84
```

Or by multiple conditions:

```python
df[np.logical_and(df['revenue'] > 1000, df['revenue'] < 2000)]

            revenue  users
2020-04-01     1234     68
```

## Adding a column

By applying a function to an existing column value:

```python
import pandas as pd

data = {
  'revenue': [1234, 5678],
  'users': [68, 84] 
}
df = pd.DataFrame(data)

df['next_month_projection'] = df['revenue'].apply(lambda num: num * 1.05)

   revenue  users  next_month_projection
0     1234     68                 1295.7
1     5678     84                 5961.9
```

## Removing a column

```python
# One column:
df = df.drop('revenue', axis=1)

# Multiple columns:
df = df.drop(['revenue', 'users'] axis=1)
```

## Renaming columns

```python
df.rename(columns = {"old_name": "new name"})
```
