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

## Determining the optimal number of clusters

Experiment with varying number of clusters and chart them:

```python
inertias = []
for n in range(1, 10):
    model = KMeans(n_clusters=n, random_state=1)
    model.fit(X)
    inertias.append(model.inertia_)

fig, ax = plt.subplots()
ax.plot(range(1, len(inertias) + 1), inertias, marker='o')
ax.ticklabel_format(useOffset=False, style='plain', axis='y')
ax.set_ylim(ymin=0)
plt.xlabel("number of clusters, k")
plt.ylabel("inertia")
plt.show()
```

![](https://github.com/mattm/python-cheat-sheet/blob/master/images/kmeans-inertia.png?raw=true)

We can then pick a number of clusters where there's not much drop off past it like 3 in the example above.

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
