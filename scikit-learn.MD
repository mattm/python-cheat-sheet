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

## Imputing missing values and using a pipeline

```python
# Import necessary modules
from sklearn.preprocessing import Imputer
from sklearn.pipeline import Pipeline
from sklearn.svm import SVC
from sklearn.model_selection import train_test_split
from sklearn.metrics import classification_report

# Setup the pipeline steps: steps
steps = [('imputation', Imputer(missing_values='NaN', strategy='most_frequent', axis=0)),
        ('SVM', SVC())]

# Create the pipeline: pipeline
pipeline = Pipeline(steps)

# Create training and test sets
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.3, random_state=42)

# Fit the pipeline to the train set
pipeline.fit(X_train, y_train)

# Predict the labels of the test set
y_pred = pipeline.predict(X_test)

# Compute metrics
print(classification_report(y_test, y_pred))
```

## Scaling

```python
import numpy as np
from sklearn.preprocessing import scale

x = np.array([1, 2, 3])

# Quick way of scaling
x_scaled = scale(x)
# [-1.22474487  0.          1.22474487]

# We can see that scaling manually has the same result
standard_deviations = (x - np.mean(x)) / np.std(x)
```

## Measuring accuracy of different models

```python
from sklearn import linear_model
from sklearn.linear_model import LogisticRegression
from sklearn.ensemble import RandomForestClassifier
from sklearn.linear_model import Perceptron
from sklearn.linear_model import SGDClassifier
from sklearn.tree import DecisionTreeClassifier
from sklearn.neighbors import KNeighborsClassifier
from sklearn.svm import SVC, LinearSVC
from sklearn.naive_bayes import GaussianNB

# SGDClassifier
sgd = linear_model.SGDClassifier(max_iter=5, tol=None)
sgd.fit(X_train, y_train)
acc_sgd = sgd.score(X_train, y_train)

# Random Forest
random_forest = RandomForestClassifier(n_estimators=100)
random_forest.fit(X_train, y_train)
acc_random_forest = random_forest.score(X_train, y_train)

# Logistic Regression
logreg = LogisticRegression()
logreg.fit(X_train, y_train)
acc_log = logreg.score(X_train, y_train)

# KNN
knn = KNeighborsClassifier(n_neighbors=9)
knn.fit(X_train, y_train)
acc_knn = knn.score(X_train, y_train)

# Gaussian
gaussian = GaussianNB()
gaussian.fit(X_train, y_train)
acc_gaussian = gaussian.score(X_train, y_train)

# Perceptron
perceptron = Perceptron(max_iter=5)
perceptron.fit(X_train, y_train)
acc_perceptron = perceptron.score(X_train, y_train)

# Support Vector Machine
linear_svc = LinearSVC()
linear_svc.fit(X_train, y_train)
acc_linear_svc = linear_svc.score(X_train, y_train)

# Decision Tree
decision_tree = DecisionTreeClassifier()
decision_tree.fit(X_train, y_train)
acc_decision_tree = decision_tree.score(X_train, y_train)

results = pd.DataFrame({
    'Model': ['Support Vector Machines', 'KNN', 'Logistic Regression', 
              'Random Forest', 'Naive Bayes', 'Perceptron', 
              'Stochastic Gradient Decent', 
              'Decision Tree'],
    'Score': [acc_linear_svc, acc_knn, acc_log, 
              acc_random_forest, acc_gaussian, acc_perceptron, 
              acc_sgd, acc_decision_tree]})
result_df = results.sort_values(by='Score', ascending=False)
result_df = result_df.set_index('Score')
result_df.head(9)
```

## Random Forest Feature Importance

Using a data frame:

```python
importances = pd.DataFrame({
    "feature": X_train.columns,
    "importance": random_forest.feature_importances_
})
importances_sorted = importances.sort_values("importance", ascending=False).set_index("feature")
```

Using a series:

```python
importances = pd.Series(data=rf.feature_importances_, index=X_train.columns)
importances_sorted = importances.sort_values(ascending=False)
```

Can then create a horizontal bar chart:

```python
importances_sorted.plot(kind='barh')
plt.title('Features Importances')
plt.show()
```

## Voting classifier

```python
from sklearn.ensemble import VotingClassifier

lr = LogisticRegression(random_state=1)
knn = KNN(n_neighbors=27)
dt = DecisionTreeClassifier(min_samples_leaf=0.13, random_state=1)

classifiers = [
       ('Logistic Regression', lr),
       ('K Nearest Neighbours', knn),
       ('Classification Tree', dt)]

for clf_name, clf in classifiers:    
    clf.fit(X_train, y_train)    
    y_pred = clf.predict(X_test)
    accuracy = accuracy_score(y_test, y_pred) 
    print('{:s} : {:.3f}'.format(clf_name, accuracy))

from sklearn.ensemble import VotingClassifier

vc = VotingClassifier(estimators=classifiers)     
vc.fit(X_train, y_train)   
y_pred = vc.predict(X_test)
accuracy = accuracy_score(y_test, y_pred)
print('Voting Classifier: {:.3f}'.format(accuracy))
```

* Used when we want to combine multiple classifiers
* Individual classifiers must be fit before being passed to VotingClassifier
* Default voting method is `hard` - the voting classifier chooses whichever prediction is the most common
* Alternative is `soft` where instead of looking at 0/1 prediction, it uses the probabilities

## Bagging classifier

```python
from sklearn.tree import DecisionTreeClassifier
from sklearn.ensemble import BaggingClassifier

dt = DecisionTreeClassifier(random_state=1)
bc = BaggingClassifier(base_estimator=dt, n_estimators=50, random_state=1)

bc.fit(X_train, y_train)
y_pred = bc.predict(X_test)
acc_test = accuracy_score(y_test, y_pred)
print('Test set accuracy of bc: {:.2f}'.format(acc_test)) 
```

* Used when we want to increase the accuracy of a single classifier
* Classifier/estimator can be any model
* The bagging classifier chooses random samples from the training set with replacement
* This means some instances may be sampled multiple times, others not at all
* Instances that are _not_ sampled are called Out of Bag (OOB) instances
* We can evaluate the performance of the model on the OOB instances, eliminating the need for cross-validation. This is known as OOB evaluation.

Here's how to grab the OOB score:

```python
dt = DecisionTreeClassifier(random_state=1)
bc = BaggingClassifier(base_estimator=dt, n_estimators=50, oob_score=True, random_state=1)

bc.fit(X_train, y_train)
y_pred = bc.predict(X_test)
acc_test = accuracy_score(y_test, y_pred)
acc_oob = bc.oob_score_
print('Test set accuracy: {:.3f}, OOB accuracy: {:.3f}'.format(acc_test, acc_oob))
```

* Setting `oob_score=True` tells the classifier to use out-of-bag samples to estimate the generalization error.
* After fitting we can then grab the OOB score via `oob_score_`

## Adaboost

```python
from sklearn.tree import DecisionTreeClassifier
from sklearn.ensemble import AdaBoostClassifier
from sklearn.metrics import roc_auc_score

dt = DecisionTreeClassifier(max_depth=2, random_state=1)
ada = AdaBoostClassifier(base_estimator=dt, n_estimators=180, random_state=1)
ada.fit(X_train, y_train)

y_pred_proba = ada.predict_proba(X_test)[:, 1]
ada_roc_auc = roc_auc_score(y_test, y_pred_proba)

print('ROC AUC score: {:.2f}'.format(ada_roc_auc))
```

* See [why use decision trees with Adaboosting](https://stats.stackexchange.com/questions/124628/why-adaboost-with-decision-trees)

## Gradient Boosting

```python
from sklearn.ensemble import GradientBoostingRegressor
from sklearn.metrics import mean_squared_error

gb = GradientBoostingRegressor(max_depth=4, n_estimators=200, random_state=2)
gb.fit(X_train, y_train)

y_pred = gb.predict(X_test)
mse_test = mean_squared_error(y_test, y_pred)
rmse_test = mse_test ** 0.5

print('Test set RMSE of gb: {:.3f}'.format(rmse_test))
```

Stochastic Gradient Boosting

```python
from sklearn.ensemble import GradientBoostingRegressor
from sklearn.metrics import mean_squared_error as MSE

sgbr = GradientBoostingRegressor(max_depth=4, subsample=0.9, max_features=0.75, n_estimators=200, random_state=2)
sgbr.fit(X_train, y_train)

y_pred = sgbr.predict(X_test)
mse_test = MSE(y_test, y_pred)
rmse_test = mse_test ** 0.5

print('Test set RMSE of sgbr: {:.3f}'.format(rmse_test))
```
