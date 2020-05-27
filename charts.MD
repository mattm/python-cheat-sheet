# Charts

## Pyplot

```python
import matplotlib.pyplot as plt

# Chart type:
plt.plot(x, y)
plt.scatter(x, y)
plt.hist(x)

# Lots of options including:
plot.xlabel("...")
plot.ylabel("..")
plot.title("...")
plt.xticks(values, labels)

# Then show it:
plt.show()
```

## Box plot

```python
import pandas as pd
df = pd.read_csv('gapminder.csv')

# First argument is the y-axis, second argument is what to group by on the x-axis
df.boxplot('life', 'Region', rot=60)

plt.show()
```

## Resetting an existing plot

```python
plt.close()
```

## Count Plot

For visualizing the occurrences of a categorical variable:

```python
sns.countplot(x = 'education', hue = 'party', data = df)
```

* `x` is the column name of the categorical variable
* `hue` here is the column we want to segment by

## Styling

### seaborn

```python
import seaborn as sns

sns.set()

# Then use plt like normal
# ...

plt.show()
```

### ggplot

```python
plt.style.use('ggplot')
```

## Correlation heatmap

```python
sns.heatmap(df.corr(), square=True, cmap='RdYlGn')
```

## Seaborn histogram

```python
values = np.random.randint(0, 11, size=100)
sns.distplot(values, bins=11, kde=False, axlabel="Random Number", color="#0343DF")
```

## Two Seaborn charts side by side

```python
values1 = np.random.normal(size=1000)
values2 = np.random.normal(size=1000)

fig, axes = plt.subplots(nrows=1, ncols=2, figsize=(10, 4))
ax = sns.distplot(values1, ax=axes[0])
ax = sns.distplot(values2, ax=axes[1])
```
