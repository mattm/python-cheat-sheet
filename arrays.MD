# Arrays

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
