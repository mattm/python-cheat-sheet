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

## k-NN Classifier

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

## Linear Regression

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

## Lasson regregssion analysis

To identify which features are the most important

```python
import pandas as pd
import matplotlib.pyplot as plt
from sklearn.linear_model import Lasso

df = pd.read_csv("data.csv")
df = df.rename(columns = {
    "Company ID": "com_id",
    "Billings Total Users": "users",
    "Billings Total Docs": "docs",
    "Billings Total Addons": "addons",
    "Billings Total Messages": "messages",
    "Billings Total Billings": "billings"
})

features = df.drop(["com_id", "billings"], axis=1)
feature_names = features.columns
X = features.values
y = df["billings"].values

# Instantiate a lasso regressor
lasso = Lasso(alpha=0.4, normalize=True)

# Fit the regressor to the data
lasso.fit(X, y)
lasso_coef = lasso.coef_

# Plot the coefficients
plt.plot(range(len(feature_names)), lasso_coef)
plt.xticks(range(len(feature_names)), feature_names.values, rotation=60)
plt.show()
```

## Confuision Matrix

```python
import pandas as pd
from sklearn.neighbors import KNeighborsClassifier
from sklearn.model_selection import train_test_split
from sklearn.metrics import classification_report
from sklearn.metrics import confusion_matrix

df = pd.read_csv("data.csv")
df = df.drop(['company_id', 'sign_up_email_domain', 'trial_score', 'trial_score_description'], axis=1)
df['sign_up_email_type'] = df['sign_up_email_type'].apply(lambda x: 1 if x == 'Work Email' else 0)

X = df.drop(['has_closed'], axis=1)
y = df['has_closed'].values
X_train, X_test, y_train, y_test = train_test_split(X, y, random_state=42, stratify=y)
knn = KNeighborsClassifier(n_neighbors=7)
knn.fit(X_train, y_train)
y_pred = knn.predict(X_test)

# +---------------+------------------+-----------------+
# |               | Predicted: False | Predicted: True |
# +---------------+------------------+-----------------+
# | Actual: False | True Negative    | False Positive  |
# | Actual: True  | False Negative   | True Positive   |
# +---------------+------------------+-----------------+

c_matrix = confusion_matrix(y_test, y_pred)
c_report = classification_report(y_test, y_pred)

true_negative = c_matrix[0, 0]     # Free customer, correctly predicted
false_positive = c_matrix[0, 1]    # Free customer, but predicted to be paid
false_negative = c_matrix[1, 0]    # Conversion, but predicted to be free
true_positive = c_matrix[1, 1]     # Conversion, correctly predicted

precision = true_positive / (true_positive + false_positive)
recall = true_positive / (true_positive + false_negative)
accuracy = (true_negative + true_positive) / (true_negative + false_positive + false_negative + true_positive)

print("Confusion Matrix:\n", c_matrix, "\n")
print("Classification Report:\n", c_report, "\n")
print("Precision:", precision)     # The % of companies classified as a conversion that actually converted
print("Recall:", recall)           # The % of conversions that were correctly identified
print("Accuracy:", accuracy)       # The overall % correct. Same as knn.score(X_test, y_test)
```

## ROC Curves

```python
import pandas as pd
import matplotlib.pyplot as plt
from sklearn.neighbors import KNeighborsClassifier
from sklearn.model_selection import train_test_split
from sklearn.linear_model import LogisticRegression
from sklearn.metrics import roc_curve
from sklearn.metrics import roc_auc_score
from sklearn.model_selection import cross_val_score

# Prepare the data
df = pd.read_csv("data.csv")
df = df.drop(['company_id', 'sign_up_email_domain', 'trial_score', 'trial_score_description'], axis=1)
df['sign_up_email_type'] = df['sign_up_email_type'].apply(lambda x: 1 if x == 'Work Email' else 0)

# Train a logistic regression model
X = df.drop(['has_closed'], axis=1)
y = df['has_closed'].values
X_train, X_test, y_train, y_test = train_test_split(X, y, random_state=42, stratify=y)
logreg = LogisticRegression()
logreg.fit(X_train, y_train)

# predict_proba returns a 2d array, where each array item is an array containing the probability for each class
# In this case (binomial classification) it's: [probability_free, probability_converted]
# We grab the probability estimates of the positive class (a conversion) for each observation in the test data:
y_pred_prob = logreg.predict_proba(X_test)[:, 1]

# We can see for each actual result (whether a company in the test data converted or not)
# what the model assigned as the company's conversion probability
for index, y_actual in enumerate(y_test[0:10]):
    model_conversion_prob = y_pred_prob[index]
    print(y_actual, model_conversion_prob.round(2))

# Now what we want to do is understand the relationship between TPR and FPR

# We can calculate TPR and FPR manually at a given threshold:
positive_identifier = lambda i: i > 0.2
df = pd.DataFrame({
    'y_test': y_test,
    'y_pred_prob': y_pred_prob,
    'is_positive': positive_identifier(y_pred_prob)
})
df['is_true_positive'] = df.apply(lambda x: (x['y_test'] == True) & (x['is_positive'] == True), axis=1)
df['is_false_positive'] = df.apply(lambda x: (x['y_test'] == False) & (x['is_positive'] == True), axis=1)
true_positive_rate = np.sum(df['is_true_positive']) / np.sum(df['y_test'])
false_positive_rate = np.sum(df['is_false_positive']) / np.sum(df['y_test'] == False)

# At a threshold of 0.2, we get a FPR of 019. and a TPR of 0.72 which we'll later verify is on the ROC curve

# Instead of doing this manually though, we can use roc_curve to plot all of the FPR/TPR points at various thresholds
fpr, tpr, thresholds = roc_curve(y_test, y_pred_prob)

# We can also compute the area under the curve (AUC) which will range from 
# 0.5 (model is no better than random guesses) to 1 (100% TPR and 0% FPR).
print('AUC:', roc_auc_score(y_test, y_pred_prob))

# And instead of just checking the AUC for this training data, we can check it against different training sets:
cv_auc = cross_val_score(logreg, X, y, cv=5, scoring='roc_auc')
print('Cross Val AUC:', cv_auc)

plt.plot([0, 1], [0, 1], 'k--')
plt.plot(fpr, tpr, label='Logistic Regression')
plt.scatter([false_positive_rate], [true_positive_rate], c='red')
plt.xlabel('False Positive Rate')
plt.ylabel('True Positive Rate')
plt.title('Logistic Regerssion ROC Curve')
plt.show()
```
