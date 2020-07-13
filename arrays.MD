# Arrays

## Creating an array from a list

```python
>>> np.array([5, 10, 15])
array([ 5, 10, 15])
```

## Slicing an array

```python
arr = np.array([5, 10, 15, 20, 25, 30, 35, 40])
```

Getting elements 2 and 3:

```python
arr[2:4]
```

```
[15, 20]
```

Getting first 2 elements:

```python
arr[:2]
``

```
[5, 10]
```

Getting last 2 elements:

```python
arr[-2:]
```

```
[35, 40]
```

## Subsetting an array

```python
>>> x = np.array([5, 10, 15])
>>> x[x > 8]
array([10, 15])
```

With multiple conditions:

```python
x[np.logical_and(x > 8, x < 12)]
array([10])
```

## Mapping items

```python
x = np.array([4, 6, 8, 10])
squarer = lambda i: i ** 2
squared = squarer(x)

# [ 16  36  64 100]
```


## Reshaping

```python
x = np.array([
    [3, 4, 5],
    [6, 7, 8]])

# x.reshape((num_rows, num_columns))

x.reshape((1, 6))
# array([[3, 4, 5, 6, 7, 8]])

# Equivalent to this because "If an integer, 
# then the result will be a 1-D array of that length."
x.reshape(6)
# array([[3, 4, 5, 6, 7, 8]])

x.reshape((2, 3))
# array([[3, 4, 5],
#        [6, 7, 8]])

x.reshape((6, 1))

# array([[3],
#        [4],
#        [5],
#        [6],
#        [7],
#        [8]])

# Equivalent to:
# Because "One shape dimension can be -1. In this case, the 
# value is inferred from the length of the array and remaining dimensions.""
x.reshape((-1, 1))
# array([[3],
#        [4],
#        [5],
#        [6],
#        [7],
#        [8]])
```

This last one comes up a lot: `reshape((-1, 1))`: it takes an array of `n` items:

```python
[1, 2, 3, 4, 5]
```

And converts it into a 2d array with 1 column and `n` rows:

```python
array([[1],
       [2],
       [3],
       [4],
       [5]])
```

## Creating an array of evenly spaced numbers between two values

```python
np.linspace(1, 2, num=2)
array([1. , 1.5 ])

np.linspace(1, 2, num=3)
array([1. , 1.5, 2. ])
```

## Stacking

Horizontal stacking:

```python
a1 = np.array(['a', 'b', 'c'])
a2 = np.array(['d', 'e', 'f'])

np.hstack([a1, a2])
```

```
['a' 'b' 'c' 'd' 'e' 'f']
```

Vertical stacking:

```python
a1 = np.array(['a', 'b', 'c'])
a2 = np.array(['d', 'e', 'f'])

np.vstack([a1, a2])
```

```
[['a' 'b' 'c']
 ['d' 'e' 'f']]
 ```
 
When vertically stacking, each array needs to have the same number of items otherwise it will error:

```python
a1 = np.array(['a', 'b', 'c'])
a2 = np.array(['d', 'e', 'f', 'g'])

np.vstack([a1, a2])
```

```
ValueError: all the input array dimensions for the concatenation axis must match exactly, but along dimension 1, the array at index 0 has size 3 and the array at index 1 has size 4
```

## Calculating the maximum values element-wise

```python
a1 = np.array([2, 5, 10])
a2 = np.array([3, 4, 6])

np.maximum(a1, a2)
```

```
[ 3  5 10]
```

Can also supply a number for the second argument in which case it will be _broadcast_ to an array and compared element-wise:

```python
a = np.array([2, 5, 10])
np.maximum(a1, 6)
```

```
[ 6  6 10]
```

## Finding the positions of the largest values in an array

```python
a = np.array([[1, 2, 3], [4, 5, 6]])
np.argmax(a)
```

Because 6 is the largest value and is in the 5th position of the flattened array:

```
5
```

We can also look at it column-wise:

```python
a = np.array([[1, 2, 3], [4, 5, 6]])
np.argmax(a, axis=0)
```

Because 4 is greater than 1, 5 is greater than 2, and 6 is greater than 3:

```
[1 1 1]
```

Or row-wise:

```python
a = np.array([[1, 2, 3], [4, 5, 6]])
np.argmax(a, axis=1)
```

Because 3 (in the 2nd position of the first array) is the largest value in [1, 2, 3] and because 6 (in the 2nd position of the second array) is the largest value in [4, 5, 6]):

```
[2 2]
```

## Returning the indeces that would sort an array

```python
arr = np.array([3, 10, 2])
arr.argsort()
```

```
[2, 0, 1]
```

Because the smallest value is position 2, then the next smallest is in position 0, then the next smallest is in position 1.

## Finding array values that contain a string

via [StackOverflow](https://stackoverflow.com/a/38974252/156835):

```python
arr = np.array(['company_id', 'days_after_sign_up', 'user_count'])

arr[np.flatnonzero(np.core.defchararray.find(arr, "_count") != -1)]
```

Or:

```python
[column for column in columns if "_count" in column]
```
