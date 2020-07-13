# Principle Component Analysis

## Basics

* Unsupervised learning method
* Difficult to understand components beyond which have highest variance
* Good step to do at end of processing because of way data gets transformed and reshaped

```python
import numpy as np
from sklearn.decomposition import PCA

df = pd.DataFrame({
    'low_variance': np.random.normal(100, 1, 10),
    'high_variance': np.random.normal(100, 50, 10),
    'medium_variance': np.random.normal(100, 10, 10)
})

print("Original:\n")
print(df)

pca = PCA()
arr = pca.fit_transform(df)

print("\nAfter PCA:\n")
print(arr)

print("\nExplained Variance Ratio:\n")
print(pca.explained_variance_ratio_)

print("\nExplained Variance:\n")
print(pca.explained_variance_)

features = range(pca.n_components_)
plt.bar(features, pca.explained_variance_)
plt.xlabel('PCA feature')
plt.ylabel('variance')
plt.xticks(features)
plt.show()
```

```
Original:

   low_variance  high_variance  medium_variance
0     98.853326      74.973886        90.331814
1     99.218176      80.554743       110.264567
2    100.336048      88.807404       106.167272
3     99.941872     206.608862       103.196729
4    101.175028     116.555090       124.883467
5     98.555317      45.198237       118.807557
6    100.243787     180.985931       100.396761
7     99.904926     130.968479        91.238566
8     99.829526      58.465493       106.435892
9     97.265555     190.332202       111.082900

After PCA:

[[-41.85855336 -17.25228019   0.5981049 ]
 [-36.8956139    2.84491171   0.34150271]
 [-28.52079541  -0.98956602  -0.80001367]
 [ 89.31634592  -0.32519344  -0.43979183]
 [ -1.3641927   18.57837332  -1.54072497]
 [-72.49871119  10.28890281   1.05641304]
 [ 63.79193465  -3.91291627  -0.75317685]
 [ 14.08104691 -14.61211066  -0.45692694]
 [-58.85638037  -1.66026391  -0.2874725 ]
 [ 72.80491947   7.04014265   2.28208612]]

Explained Variance Ratio:

[9.66398855e-01 3.32534266e-02 3.47718766e-04]

Explained Variance:

[3.35494567e+03 1.15442438e+02 1.20713881e+00]
```

* Notice how most of the variance is explained by one feature (the high variance one).
* The order of `explained_variance_` and `explained_variance_ratio_` _do not_ necessarilly line up with the order of the features in the original data frame
* By default PCA uses the same number of components as features, but can be reduced by setting `n_components`
* Explained Variance Ratio: Percentage of variance explained by each of the selected components (adds up to 1)
* Explained Variance: The amount of variance explained by each of the selected components.

## Charting the variance

```python
features = range(pca.n_components_)
plt.bar(features, pca.explained_variance_)
plt.xlabel('PCA feature')
plt.ylabel('variance')
plt.xticks(features)
plt.show()
```

![](https://github.com/mattm/python-cheat-sheet/blob/master/images/pca-variance.png?raw=true)
