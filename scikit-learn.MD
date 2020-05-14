# scikit-learn

## Expected Format

* The features need to be in an array where each column is a feature and each row a different observation or data point
* The target needs to be a single column with the same number of observations as the feature data

```python
from sklearn import datasets
import pandas as pd

iris = datasets.load_iris()
```

Here `iris` is of type `sklearn.utils.Bunch` and has these keys:

```python
iris.keys()
dict_keys(['data', 'target', 'frame', 'target_names', 'DESCR', 'feature_names', 'filename'])
```

```python
X = iris.data

array([[5.1, 3.5, 1.4, 0.2],
       [4.9, 3. , 1.4, 0.2],
       [4.7, 3.2, 1.3, 0.2],
       [4.6, 3.1, 1.5, 0.2],
       [5. , 3.6, 1.4, 0.2],
       [5.4, 3.9, 1.7, 0.4],
       ...
```

```python
y = iris.target

array([0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,
       0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,
       0, 0, 0, 0, 0, 0, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1,
       1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1,
       1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2,
       2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2,
       2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2])
```

"Notice we named the feature array X and response variable y: This is in accordance with the common scikit-learn practice."

```python
iris.feature_names

['sepal length (cm)',
 'sepal width (cm)',
 'petal length (cm)',
 'petal width (cm)']
 ```
 
We can build a data frame using the features and feature names:

```python
df = pd.DataFrame(X, columns = iris.feature_names)

     sepal length (cm)  sepal width (cm)  petal length (cm)  petal width (cm)
0                  5.1               3.5                1.4               0.2
1                  4.9               3.0                1.4               0.2
2                  4.7               3.2                1.3               0.2
3                  4.6               3.1                1.5               0.2
4                  5.0               3.6                1.4               0.2
..                 ...               ...                ...               ...
145                6.7               3.0                5.2               2.3
146                6.3               2.5                5.0               1.9
147                6.5               3.0                5.2               2.0
148                6.2               3.4                5.4               2.3
149                5.9               3.0                5.1               1.8
```

# k-NN Classifier

```python
# Import necessary modules
from sklearn.neighbors import KNeighborsClassifier
from sklearn.model_selection import train_test_split

# Create feature and target arrays
X = digits.data
y = digits.target

# Split into training and test set
## The test_size is 25% by default
## random_state lets you make the results reproducable
## stratify ensures the labels are proportionally split between training and test sets
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42, stratify=y)

# Create a k-NN classifier with 7 neighbors: knn
knn = KNeighborsClassifier(n_neighbors=7)

# Fit the classifier to the training data
knn.fit(X_train, y_train)

# Print the accuracy
print(knn.score(X_test, y_test))
```

# Linear Regression

```python
import numpy as np
from sklearn.linear_model import LinearRegression
import matplotlib.pyplot as plt

# Simulate some observations that are approximately linear
x_actual = np.arange(30)
y_actual = 5 * x_actual + np.random.randint(0, 20, len(x_actual))

# Use skikit-learn to fit a linear regression model to the data
reg = LinearRegression()
X = x_actual.reshape(-1, 1)
y = y_actual.reshape(-1, 1)
reg.fit(X, y)

# Print out the r-squared value
print("R-squared:", reg.score(X, y))

# Create a visualization showing the data and the model predictions
plt.scatter(X, y)

prediction_space = np.linspace(min(X), max(X)).reshape(-1, 1)
y_pred = reg.predict(prediction_space)
plt.plot(prediction_space, y_pred, color='black', linewidth=2)

plt.show()
```
