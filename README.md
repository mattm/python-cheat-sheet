# Python Cheat Sheet

## Packages

### Installing a package
```
$ pip3 install numpy
```

### Importing a package
```
import numpy
import numpy as np
```

### Importing a function from a package

Not recommended because it's less clear where the function comes from. Better to import the entire package.

```
from numpy import array
from numpy import array as my_array 
```

## Conditions

```
if condition :
  ...
  ...
elif condition :
  ...
  ...
else :
  ...
  ...
 ```
 
## While loops
 
```
error = 50.0
while error > 1 :
  error = error / 2
  print(error)
```
 
## For loops
 
```
areas = [11.25, 18.0, 20.0, 10.75, 9.50]
for area in areas :
  print(area)
```
 
Or with an index:
 
```
areas = [11.25, 18.0, 20.0, 10.75, 9.50]
for index, area in enumerate(areas) :
  print("At index " + str(index) + ": " + str(area))
```

Iterate over a dict:

```
dictionary = {
	"2020-04-01": 6,
	"2020-05-01": 8
}

for key, value in dictionary.items() :
	print(key + ": " + str(value))
```

Iterating over an array:

```
arr = np.array([5, 7, 9])
for item in arr :
	print(item)
```

Iterating over an array of arrays:

```
arr1 = np.array([5, 7, 9])
arr2 = np.array([11, 13, 15])
arr = np.array([arr1, arr2])
for item in np.nditer(arr) :
	print(item)
```

Iterating from 0 to 10:

```
for x in range(11) : 
	print(x)
```

Iterating 5 to 10:

```
for x in range(5, 11) : 
	print(x)
```

## Lists

### Appending to a list

```
x = []
x.append("hi")
```

## Arrays

### Creating an array from a list

```
>>> np.array([5, 10, 15])
array([ 5, 10, 15])
```

### Subsetting an array

```
>>> x = np.array([5, 10, 15])
>>> x[x > 8]
array([10, 15])
```

With multiple conditions:

```
x[np.logical_and(x > 8, x < 12)]
array([10])
```

## Functions

```
def square(num):
    """Squres a number"""
    return num ** 2

print(square(5))
```

## Charts

## Simple charts

```
import matplotlib.pyplot as plt

# Chart type:
plt.plot(x, y)
plt.scatter(x, y)
plt.hist(x)

# Lots of options including:
plot.xlabel("...")
plot.ylabel("..")
plot.title("...")
plt.xticks(values, labels)

# Then show it:
plt.show()
```

## Data Frames

### Building manually

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

### Importing from a CSV

```
import pandas as pd
df = pd.read_csv("data.csv")
```

### Selecting a column as a data frame

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

### Selecting rows

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

## Random numbers

Setting a seed for reproducability:

```
np.random.seed(123)
```

Between 0 and 1:

```
np.random.rand()
```

Between 0 and 10:

```
print(np.random.randint(0, 11))
```
