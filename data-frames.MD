# Data Frames

## Building manually

With a default interger indeces:

```
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

```
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

```
rows, columns = df.shape
```

## Importing from a CSV

```
import pandas as pd
df = pd.read_csv("data.csv")
```

## Selecting a column as a data frame

```
df[['revenue']]

            revenue
2020-04-01     1234
2020-05-01     5678
```

Or multiple:

```
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

```
df['users'].tolist()
```

## Get array from column of Pandas dataframe

```
df['users'].to_numpy()
```

## Selecting rows

All columns for a row by its label:

```
df.loc[['2020-04-01']]

            revenue  users
2020-04-01     1234     68
```

Or specific columns for a row by its label:

```
df.loc[['2020-04-01'], ['users']]

            users
2020-04-01     68
```

Or all columns for a row by its index:

```
df.iloc[[1]]

            revenue  users
2020-05-01     5678     84
```

Or by a condition:

```
df[df['revenue'] > 3000]

            revenue  users
2020-05-01     5678     84
```

Or by multiple conditions:

```
df[np.logical_and(df['revenue'] > 1000, df['revenue'] < 2000)]

            revenue  users
2020-04-01     1234     68
```

## Adding a column

By applying a function to an existing column value:

```
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

## Renaming columns

```
df.rename(columns = {"old_name": "new name"})
```
