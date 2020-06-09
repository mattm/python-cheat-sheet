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

## Facet Grids

```python
import pandas as pd
import seaborn as sb
from matplotlib import pyplot as plt
df = sb.load_dataset('tips')
print(df.head())

# FacetGrid will take all the combinations of col/row
# and create individual charts for them.
# Here the columns have the different smoker values (Yes/No) and
# the rows have the different sex values (Male/Female)
# Hue is optional and can be used to color code the chart by
# some other variable
g = sb.FacetGrid(df, col="smoker", row="sex", hue="size")

# Both aren't necessary though, can just do one variable in
# which case there will just be a single row
# g = sb.FacetGrid(df, col="smoker")

# Because we're plotting histograms here, map
# just needs one variable
# g.map(plt.hist, "total_bill")

# Whereas if we're plotting scatter charts, map
# needs two variables, one for the x-axis and one
# for the y-axis
g.map(plt.scatter, "total_bill", "tip")
plt.show()
```

## Point Plots

The point is the mean and the bars represent the confidence interval:

```python
import seaborn as sns
tips = sns.load_dataset("tips")
ax = sns.pointplot(x="time", y="total_bill", data=tips)
```

Can also segment the chart by other variables:

```python
ax = sns.pointplot(x="time", y="total_bill", hue="smoker", data=tips)
```

## Correlation Matrix

```python
import pandas as pd
import seaborn as sns

train = pd.read_csv("train.csv")
_ = sns.heatmap(train.corr())
```

![](https://github.com/mattm/python-cheat-sheet/blob/master/images/correlation-matrix.png?raw=true)

## Correlations for a single feature

```python
import pandas as pd
import matplotlib.pyplot as plt

train = pd.read_csv("train.csv")
corr = train.corr().abs().unstack().sort_values(ascending=False).reset_index()
corr.rename(columns={"level_0": "Feature 1", "level_1": "Feature 2", 0: "Correlation"}, inplace=True)
survival_correlations = corr[corr["Feature 1"] == "Survived"]
print(survival_correlations)

plt.bar(survival_correlations["Feature 2"], survival_correlations["Correlation"])
plt.title("Survival Correlations")
plt.ylabel("Correlation")
plt.xlabel("Feature")
plt.xticks(rotation=90)
plt.show()
```

```
   Feature 1    Feature 2  Correlation
2   Survived     Survived     1.000000
14  Survived       Pclass     0.338481
17  Survived         Fare     0.257307
30  Survived        Parch     0.081629
32  Survived          Age     0.077221
37  Survived        SibSp     0.035322
45  Survived  PassengerId     0.005007
```
