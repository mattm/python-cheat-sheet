# Principle Component Analysis

## Basics

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
```

```
Original:

   high_variance  medium_variance  low_variance
0      95.514873       101.450851    101.697078
1     117.801174        84.084475     99.802566
2     150.095851       108.533088    100.057573
3      39.939981       103.098673    100.444183
4     127.616822       110.675462     97.433507
5       8.012819        90.143933    101.071803
6     115.965020       105.774190    101.250734
7     126.444258        80.151325    100.253795
8     108.475801        90.912758    101.520122
9     104.454932       102.027281     99.742984

After PCA:

   high_variance  medium_variance  low_variance
0       3.774522        -3.881040     -1.446243
1     -17.806021        14.318993      0.766968
2     -51.062511        -8.771470     -0.490919
3      59.221604        -7.834653      0.240114
4     -28.716019       -11.906399      2.263857
5      91.653008         3.817205      0.272948
6     -16.837691        -7.376157     -1.305509
7     -26.277426        18.613585      0.357102
8      -8.749023         7.168306     -1.070549
9      -5.200443        -4.148371      0.412231

Explained Variance Ratio:

[9.43893829e-01 5.54678703e-02 6.38300793e-04]
```

Notice how most of the variance is explained by the first (high variance) feature.
