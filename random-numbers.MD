# Random numbers

## Setting a seed for reproducability:

```python
np.random.seed(123)
```

## Generate a random number between 0 and 1

```python
np.random.rand()

# or: 

np.random.random()
```

Can also specify the quantity:

```python
np.random.rand(size = 10)
```

## Generate a random integer between 0 and 10

```python
np.random.randint(0, 11, size=100)
```

## Generate a 2d array of random integers

```python
np.random.randint(1, 100, size=(10, 2)
```

```
array([[18, 91],
       [82, 75],
       [93, 48],
       [50, 66],
       [67, 54],
       [78, 90],
       [70,  1],
       [63, 24],
       [10, 90],
       [ 5, 51]])
```

## Generate random numbers from a normal distribution

```python
np.random.normal(size=100)
```
