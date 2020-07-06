# Stats

## Getting the percentile of a number

```python
from scipy import stats

items = [1, 2, 3, 4]
stats.percentileofscore(items, 3)
```

```
75.0
```

## Cacluating the correlation between two variables

```python
from scipy.stats import pearsonr

correlation, pvalue = pearsonr(width, length)
```
