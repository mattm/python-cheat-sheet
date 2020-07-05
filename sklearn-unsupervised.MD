# Unsupervised Learning

# KMeans Clustering

```python
from sklearn.cluster import KMeans

model = KMeans(n_clusters=3)
model.fit(points)

labels = model.predict(new_points)
print(labels)
```

Creating a scatter plot with the cluster centers visualized:

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
