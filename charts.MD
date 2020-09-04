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

## Stacked bar chart

Assuming we have the data in a format like this:

```
Pclass     1      2      3
Deck                      
A       15.0    0.0    0.0
B       47.0    0.0    0.0
C       59.0    0.0    0.0
D       29.0    4.0    0.0
E       25.0    4.0    3.0
F        0.0    8.0    5.0
G        0.0    0.0    4.0
M       41.0  168.0  479.0
```

Then it's simply a matter of:

```python
_ = df.plot(kind='bar', stacked=True, title="Passengers by Deck")
_.set_ylabel("Passengers")
```

![](https://github.com/mattm/python-cheat-sheet/blob/master/images/stacked-bar-chart.png?raw=true)

With some data manipulation it's also possible to get a [stacked bar chart %](https://github.com/mattm/python-cheat-sheet/blob/master/data-frames.MD#calculating-percentages-for-a-stacked-bar-chart).

## Scatter Plot

```python
import matplotlib.pyplot as plt
from matplotlib.colors import ListedColormap

colors = ListedColormap(['gray', 'cyan'])
scatter = plt.scatter(samples["total_emails_received_count"], samples["total_ticket_response_count"], c=samples["has_closed"], cmap=colors, alpha=0.4)
plt.legend(handles=scatter.legend_elements()[0], labels=["Never Closed", "Closed"], loc="upper left", bbox_to_anchor=(1, 1))
plt.xlabel("Emails Received")
plt.ylabel("Ticket Responses")
plt.show()
```

## Legend Position

Placing the legend on the right of the chart itself:

```python
_.legend(loc='upper left', bbox_to_anchor=(1, 1), prop={'size': 15})
```

## Hiding a legend

```python
plt.legend('', frameon=False)
```

or

```python
ax.get_legend().remove()
```

or

```python
df.plot(legend=False)
```

## Setting the chart size

Arguments are width and height:

```python
plt.figure(figsize=(10, 6))
```

## Formatting y-axis as a percent

```python
import matplotlib.ticker as mtick

_.yaxis.set_major_formatter(mtick.PercentFormatter())
```

This will not convert numbers like 0.20 to 20%, so you need to multiply by 100 before using this.

## Trend Line

```python
z = np.polyfit(x, y, 1)
p = np.poly1d(z)
plt.plot(x,p(x),"r--")
```

## Bar Chart

```python
# Vertical
df.plot(kind="bar")

# Horizontal
df.plot(kind="barh")
```
