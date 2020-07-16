# Loops

## While loops
 
```python
error = 50.0
while error > 1 :
  error = error / 2
  print(error)
```
 
## For loops
 
```python
areas = [11.25, 18.0, 20.0, 10.75, 9.50]
for area in areas :
  print(area)
```
 
Or with an index:
 
```python
areas = [11.25, 18.0, 20.0, 10.75, 9.50]
for index, area in enumerate(areas) :
  print("At index " + str(index) + ": " + str(area))
```

Iterate over a dict:

```python
dictionary = {
	"2020-04-01": 6,
	"2020-05-01": 8
}

for key, value in dictionary.items() :
	print(key + ": " + str(value))
```

Iterating over an array:

```python
arr = np.array([5, 7, 9])
for item in arr :
	print(item)
```

Iterating over an array of arrays:

```python
arr1 = np.array([5, 7, 9])
arr2 = np.array([11, 13, 15])
arr = np.array([arr1, arr2])
for item in np.nditer(arr) :
	print(item)
```

Iterating from 0 to 10:

```python
for x in range(11) : 
	print(x)
```

Iterating 5 to 10:

```python
for x in range(5, 11) : 
	print(x)
```
