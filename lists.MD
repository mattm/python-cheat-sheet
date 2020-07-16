# Lists

## Appending to a list

```python
x = []
x.append("hi")
```

## List comprenension

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
