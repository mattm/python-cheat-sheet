# KMeans Clustering

## The Basics

The `n_clusters` hyperparameter is adjustable depending on how many clusters you want.

```python
from sklearn.cluster import KMeans

df = pd.DataFrame(np.random.randint(1, 100, size=(1000, 2)), columns=["x", "y"])

model = KMeans(n_clusters=10, random_state=1)
labels = model.fit_predict(df)

plt.scatter(df["x"], df["y"], c=labels)
plt.show()
```

![](https://github.com/mattm/python-cheat-sheet/blob/master/images/kmeans.png?raw=true)

## Measuring the inertia

The intertia measures how spread out the clusters are (lower is better). k-means attempts to minimize inertia when determining the clusters.

```python
model.inertia_
```

## Creating a scatter plot with the cluster centers visualized:

```python
import matplotlib.pyplot as plt

xs = new_points[:, 0]
ys = new_points[:, 1]

plt.scatter(xs, ys, c=labels, alpha=0.5)

centroids = model.cluster_centers_
centroids_x = centroids[:,0]
centroids_y = centroids[:,1]

plt.scatter(centroids_x, centroids_y, marker='D', s=50)
plt.show()
```
