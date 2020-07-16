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
np.random.rand(10)
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

Can specify the mean with `loc` and standard deviration with `scale`:

```python
returns = np.random.normal(loc=0.02, scale=0.05, size=1000)
```

```python
mean = np.mean(returns)
std = np.std(returns)
print("The mean is %5.3f and the standard deviation is %5.3f" %(mean, std))
```

```
The mean is 0.018 and the standard deviation is 0.050
```

