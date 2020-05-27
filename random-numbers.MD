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

## Generate random numbers from a normal distribution

```python
np.random.normal(size=100)
```
