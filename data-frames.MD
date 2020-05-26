# Data Frames

## Building manually

With a default interger indeces:

```python
data = { 
  'month': ['2020-04-01', '2020-05-01'], 
  'revenue': [1234, 5678],
  'users': [68, 84]
}
df = pd.DataFrame(data)

#         month  revenue  users
# 0  2020-04-01     1234     68
# 1  2020-05-01     5678     84
```

With named indeces:

```python
data = {
  'revenue': [1234, 5678],
  'users': [68, 84] 
}
df = pd.DataFrame(data)
df.index = ['2020-04-01', '2020-05-01']

#             revenue  users
# 2020-04-01     1234     68
# 2020-05-01     5678     84
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
df = pd.read_csv("data.csv")
```

## Selecting a column as a data frame

```python
df[['revenue']]

#             revenue
# 2020-04-01     1234
# 2020-05-01     5678
```

Or multiple:

```python
df[['revenue', 'users']]

            revenue  users
2020-04-01     1234     68
2020-05-01     5678     8

# or

df.loc[:, ['revenue', 'users']]

#             revenue  users
# 2020-04-01     1234     68
# 2020-05-01     5678     84
```

## Get list from column of Pandas dataframe

```python
df['users'].tolist()
df.users.tolist()
```

## Get array from column of Pandas dataframe

```python
df['users'].values
df.users.values

# or

df['users'].to_numpy()
df.users.to_numpy()
```

## Methods work on data frame as well as columns

```python
df = pd.DataFrame({
    'col1': [1, 2, 3],
    'col2': [4, 5, 6]
})

# To sum all of the values
print(df.to_numpy().sum())
# 21

# To return a data frame with the sum of each column
print(df.sum())
# col1     6
# col2    15
# dtype: int64

# To sum a specific column:
print(df.col1.sum())
# 6
```


## Selecting rows

All columns for a row by its label:

```python
df.loc[['2020-04-01']]

#             revenue  users
# 2020-04-01     1234     68
```

Or specific columns for a row by its label:

```python
df.loc[['2020-04-01'], ['users']]

#             users
# 2020-04-01     68
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

#             revenue  users
# 2020-05-01     5678     84
```

Or by multiple conditions:

```python
df[np.logical_and(df['revenue'] > 1000, df['revenue'] < 2000)]

#             revenue  users
# 2020-04-01     1234     68
```

## Subsetting by index number

```python
mydict = [{'a': 1, 'b': 2, 'c': 3, 'd': 4},
          {'a': 100, 'b': 200, 'c': 300, 'd': 400},
          {'a': 1000, 'b': 2000, 'c': 3000, 'd': 4000 }]

df = pd.DataFrame(mydict)
#       a     b     c     d
# 0     1     2     3     4
# 1   100   200   300   400
# 2  1000  2000  3000  4000

# Select one or more rows as a data frame
print(df.iloc[[0, 1]])
#      a    b    c    d
# 0    1    2    3    4
# 1  100  200  300  400

# General format
# df.iloc[rows, columns]

# Select a row as a series
print(df.iloc[0])
# a    1
# b    2
# c    3
# d    4

# Select a column as a series
print(df.iloc[:, 1])
# 0       2
# 1     200
# 2    2000

# Select a specific value
print(df.iloc[1, 1])
# 200

# Select multiple rows as a data frame
print(df.iloc[0:2])
#      a    b    c    d
# 0    1    2    3    4
# 1  100  200  300  400

# Select multiple columns as a data frame
print(df.iloc[:, 0:2])
#       a     b
# 0     1     2
# 1   100   200
# 2  1000  2000
```

## Adding a column

By applying a function to an existing column value:

```python

data = {
  'revenue': [1234, 5678],
  'users': [68, 84] 
}
df = pd.DataFrame(data)

df['next_month_projection'] = df['revenue'].apply(lambda num: num * 1.05)

#    revenue  users  next_month_projection
# 0     1234     68                 1295.7
# 1     5678     84                 5961.9
```

On some data sets it's necessary to create a copy of the original data frame before manipulating it:

```python
test_df = pd.read_csv("test.csv")

# The copy is needed because we're going to be modifying the subset 
# Omitting it will lead to a SettingWithCopyWarning warning
df = test_df[['PassengerId', 'Sex']].copy()

df['Survived'] = df['Sex'].apply(lambda gender: int(gender == "female"))

df = df.drop('Sex', axis=1)
df
```

## Modifying a column

```python
df = pd.DataFrame({
    'name': ["cat", "dog", "cow", "lion"]
})

# Option 1:
df['name'] = df['name'].apply(lambda item: item.replace('c', 'm'))

# Option 2:
# for i, name in enumerate(df['name']):
#     df['name'][i] = df['name'][i].replace('c', 'm')
```

## Filling NaN values in a column

```python
X['Age'].fillna(np.mean(X['Age']), inplace=True)
```

## Removing a column

```python
# One column:
df = df.drop('revenue', axis=1)

# Multiple columns:
df = df.drop(['revenue', 'users'], axis=1)
```

## Renaming columns

```python
df.rename(columns = {"old_name": "new name"})
```

## Replacing values in a column

```python
df = pd.DataFrame({
  'revenue': [1234, 0],
  'users': [68, 84] 
})
df.revenue.replace(0, np.nan, inplace=True)

#    revenue  users
# 0   1234.0     68
# 1      NaN     84
```

## Replacing values in all columns

```python
df = df.replace('?', np.nan)

# or

df[df == '?'] = np.nan
```

## Removing all rows containing NA values

```python
df = pd.DataFrame({
  'revenue': [1234, np.nan],
  'users': [68, 84] 
})
df = df.dropna()

#    revenue  users
# 0   1234.0     68
```

## Merging two data frames

```python
data1 = pd.Series([5, 2, 3,7], index=['a', 'b', 'c', 'd'])
data2 = pd.Series([10, 11, 12, 13], index=['d', 'c', 'b', 'a'])

df = pd.concat([data1, data2], axis=1, keys=['col1', 'col2'])

#    col1  col2
# a     5    13
# b     2    12
# c     3    11
# d     7    10
```

## Grouping

This will return a dataframe:

```python
X.groupby('Pclass').mean()
```

```
             Sex        Age     SibSp     Parch       Fare  Embarked_Q 
Pclass                                                                   
1       0.435185  37.048118  0.416667  0.356481  84.154687    0.009259   
2       0.413043  29.866958  0.402174  0.380435  20.662183    0.016304   
3       0.293279  26.403259  0.615071  0.393075  13.675550    0.146640  
```

Can also grab a series from this:

```python
X.groupby('Pclass').mean().Age
```

```
Pclass
1    37.048118
2    29.866958
3    26.403259
```

## Mapping column values to a dict

```python
df = pd.DataFrame({
    'vertical': [0, 0, 3, 2, 1, 3, 0]
})

mapping = {
    0: 'work',
    1: 'school',
    2: 'teaching',
    3: 'personal-use'
}

df['vertical'] = df['vertical'].map(mapping)
df
```

```
       vertical
0          work
1          work
2  personal-use
3      teaching
4        school
5  personal-use
6          work
```

