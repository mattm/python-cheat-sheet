# Principle Component Analysis

## Basics

* Unsupervised learning method
* Difficult to understand components beyond which have highest variance
* Good step to do at end of processing because of way data gets transformed and reshaped

```python
import numpy as np
from sklearn.decomposition import PCA

df = pd.DataFrame({
    'high_variance': np.random.normal(100, 50, 10),
    'medium_variance': np.random.normal(100, 10, 10),
    'low_variance': np.random.normal(100, 1, 10)
})

print("Original:\n")
print(df)

pca = PCA()
df_pca = pd.DataFrame(pca.fit_transform(df), columns=df.columns)

print("\nAfter PCA:\n")
print(df_pca)

print("\nExplained Variance Ratio:\n")
print(pca.explained_variance_ratio_)

print("\nExplained Variance:\n")
print(pca.explained_variance_)
```

```
Original:

   high_variance  medium_variance  low_variance
0     104.316362       100.749514     98.746562
1      44.939206        99.050535    101.147283
2      34.051548        86.710008     99.336062
3     151.321535       106.303130    101.127509
4      84.528101        92.965752     99.456895
5      44.720013       105.127476     98.644154
6     120.100353       105.418759     99.873366
7      41.465990       101.811094    100.550672
8      58.143133       116.483944    100.826879
9      78.490745        99.676329    100.736497

After PCA:

   high_variance  medium_variance  low_variance
0      28.021939        -2.311500      1.293196
1     -31.349135        -0.570662     -1.225076
2     -42.918075       -12.342788      0.073733
3      75.271997         0.682473     -0.820066
4       7.830210        -8.935174      0.250210
5     -31.234649         5.402512      1.522833
6      44.046632         1.504759      0.370552
7     -34.663741         2.354348     -0.519690
8     -17.188343        16.065453     -0.185021
9       2.183165        -1.849420     -0.760671

Explained Variance Ratio:

[9.61948843e-01 3.75439107e-02 5.07246500e-04]

Explained Variance:

[1.52829263e+03 5.96477476e+01 8.05885979e-01]
```

* Notice how most of the variance is explained by the first (high variance) feature.
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
