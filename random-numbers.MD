# Random numbers

## Setting a seed for reproducability:

```
np.random.seed(123)
```

## Generate a random number between 0 and 1

```
np.random.rand()

# or: 

np.random.random()
```

Can also specify the quantity:

```
np.random.rand(size = 10)
```

## Generate a random integer between 0 and 10

```
print(np.random.randint(0, 11, size=100))
```
