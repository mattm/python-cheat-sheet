# Python Cheat Sheet

## Conditions

```python
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

Ternary:

```python
prediction = "Closed" if ticket_responses > 10 else "Not Closed"
```

 
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

## Lists

### Appending to a list

```python
x = []
x.append("hi")
```

### List comprenension

```python
nums = [1, 2, 3, 4]
updated = [num + 1 for num in nums]
print(updated)

[2, 3, 4, 5]
```

With a conditional on the predicate expression:

```python
names = ['matt', 'simon', 'morgan']
m_names = [name for name in names if name[0] == 'm']
print(m_names)

['matt', 'morgan']
```

With a conditional on the output expression:

```python
names = ['matt', 'simon', 'morgan']
m_names = [name if name[0] == 'm' else '' for name in names]
print(m_names)

['matt', '', 'morgan']
```

Creating a dict:

```python
names = ['matt', 'simon', 'morgan']
lengths = { name: len(name) for name in names }
print(lengths)

{'matt': 4, 'simon': 5, 'morgan': 6}
```

## Arrays

### Creating an array from a list

```
>>> np.array([5, 10, 15])
array([ 5, 10, 15])
```

### Subsetting an array

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

## Functions

```python
def square(num):
    """Squres a number"""
    return num ** 2

print(square(5))
```

As a lambda:

```python
square = lambda x: x ** 2
```
