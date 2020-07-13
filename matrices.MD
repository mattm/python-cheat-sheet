# Matrices

## Creating a CSR matrix

* Compressed Sparse Row (CSR) matrix

```python
from scipy.sparse import csr_matrix

row = np.array([0, 0, 1, 2, 2, 2])
col = np.array([0, 2, 2, 0, 1, 2])
data = np.array([1, 2, 3, 4, 5, 6])
matrix = csr_matrix((data, (row, col)), shape=(3, 3))

print(matrix.toarray())
```

```
[[1 0 2]
 [0 0 3]
 [4 5 6]]
```

## Interpreting a printed sparse matrix

```python
print(matrix)
```

```
  (0, 0)	1
  (0, 2)	2
  (1, 2)	3
  (2, 0)	4
  (2, 1)	5
  (2, 2)	6
```

This means (remembering that this is zero-indexed):

* The element in row 0, column 1 is a 1. 
* The element in row 0, column 2 is a 2. 
* The element in row 1, column 2 is a 3.
* And so on

## Printing as a dense matrix

```python
matrix.todense()
```

```
matrix([[1, 0, 2],
        [0, 0, 3],
        [4, 5, 6]], dtype=int64)
```

## Returning the max values in the rows or columns

`axis=0` will return max values in each column:

```python
matrix.max(axis=0).toarray().ravel()
```

```
[4 5 6]
```

`axis=1` will return max values in each row:

```python
matrix.max(axis=1).toarray().ravel()
```

```
[2 3 6]
```
