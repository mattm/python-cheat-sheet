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

## Plotting a confusion matrix

```python
from sklearn.metrics import plot_confusion_matrix

plot_confusion_matrix(clf, X_train, y_train, display_labels=["Died", "Survived"], cmap="YlGn")
plt.show()
```

![](https://github.com/mattm/python-cheat-sheet/blob/master/images/plot_confusion_matrix.png?raw=true)

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

## Hyperparameter Tuning

```python
import pandas as pd
from sklearn.linear_model import LogisticRegression
from sklearn.model_selection import GridSearchCV

# Prepare the data
df = pd.read_csv("data.csv")
df = df.drop(['company_id', 'sign_up_email_domain', 'trial_score', 'trial_score_description'], axis=1)
df['sign_up_email_type'] = df['sign_up_email_type'].apply(lambda x: 1 if x == 'Work Email' else 0)

c_space = np.logspace(-5, 8, 15)
param_grid = {'C': c_space}

logreg = LogisticRegression()
logreg_cv = GridSearchCV(logreg, param_grid, cv=5)

X = df.drop(['has_closed'], axis=1)
y = df['has_closed'].values

logreg_cv.fit(X, y)

# Print the tuned parameters and score
print("Tuned Logistic Regression Parameters: {}".format(logreg_cv.best_params_)) 
print("Best score is {}".format(logreg_cv.best_score_))
```

With `RandomSearchCV`:

```python
# Import necessary modules
from scipy.stats import randint
from sklearn.tree import DecisionTreeClassifier
from sklearn.model_selection import RandomizedSearchCV

# Setup the parameters and distributions to sample from: param_dist
param_dist = {"max_depth": [3, None],
              "max_features": randint(1, 9),
              "min_samples_leaf": randint(1, 9),
              "criterion": ["gini", "entropy"]}

# Instantiate a Decision Tree classifier: tree
tree = DecisionTreeClassifier()

# Instantiate the RandomizedSearchCV object: tree_cv
tree_cv = RandomizedSearchCV(tree, param_dist, cv=5)

# Fit it to the data
tree_cv.fit(X, y)

# Print the tuned parameters and score
print("Tuned Decision Tree Parameters: {}".format(tree_cv.best_params_))
print("Best score is {}".format(tree_cv.best_score_))
```

## Preprocessing categorical variables using get_dummies

```python
import pandas as pd
from sklearn.linear_model import LogisticRegression
from sklearn.model_selection import GridSearchCV

# Prepare the data
df = pd.read_csv("data.csv")
df = df.drop(['company_id', 'sign_up_email_domain', 'trial_score', 'trial_score_description'], axis=1)

# `get_dummies` will take the categorical labels (sign_up_email_type here) and create additional columns
# for each possible value: 
# 1. 'sign_up_email_type_None' 
# 2. 'sign_up_email_type_Personal Email' 
# 3. 'sign_up_email_type_Work Email'
# ... where the values are are a 0 or 1
df = pd.get_dummies(df)
print(df.columns)

# However, we having all three columns is unnecessary, the model only needs two because
# the third is implied by the values in the other two

# We can either drop a column manually...
df = df.drop('sign_up_email_type_None', axis=1)

# Or we could have done this directly in get_dummies:
# df = pd.get_dummies(df, drop_first=True)

# And finally we'll clean up the column name that get_dummies created
df = df.rename(columns={
    'sign_up_email_type_Work Email': 'has_work_email',
    'sign_up_email_type_Personal Email': 'has_personal_email'
})

df.head()
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

## Pipeline with hyperparameter tuning

```python
# Setup the pipeline
steps = [('scaler', StandardScaler()),
         ('SVM', SVC())]

pipeline = Pipeline(steps)

# Specify the hyperparameter space
parameters = {'SVM__C':[1, 10, 100],
              'SVM__gamma':[0.1, 0.01]}

# Create train and test sets
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=21)

# Instantiate the GridSearchCV object: cv
cv = GridSearchCV(pipeline, parameters)

# Fit to the training set
cv.fit(X_train, y_train)

# Predict the labels of the test set: y_pred
y_pred = cv.predict(X_test)

# Compute and print metrics
print("Accuracy: {}".format(cv.score(X_test, y_test)))
print(classification_report(y_test, y_pred))
print("Tuned Model Parameters: {}".format(cv.best_params_))
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

```python
importances = pd.DataFrame({
    "feature": X_train.columns,
    "importance": np.round(random_forest.feature_importances_, 3)
})
importances = importances.sort_values("importance", ascending=False).set_index("feature")
importances.head(15)
```

## Gridsearch with a pipeline

```python
from sklearn.pipeline import make_pipeline
from sklearn.preprocessing import MinMaxScaler
from sklearn.model_selection import GridSearchCV

pipe = make_pipeline(MinMaxScaler(), XGBClassifier())
param_grid = {
    "xgbclassifier__max_depth": [1, 3, 5],
    "xgbclassifier__n_estimators": [10, 100, 1000],
    "xgbclassifier__learning_rate": [0.01, 0.05, 0.1]
}
grid = GridSearchCV(pipe, param_grid, cv=5)
grid.fit(X_train, y_train)
print(" Tuned Parameters: {}".format(grid.best_params_)) 
print(" Best score is {:.2f}".format(grid.best_score_))

...
predictions = grid.predict(X_test)
```

## Using LabelEncoder

```python
import pandas as pd
import numpy as np
from sklearn.preprocessing import LabelEncoder

df = pd.DataFrame({
    "user_id": [1, 10, 5, 11],
    "vertical": ['work','student','professional-use','student']
})  

enc = LabelEncoder()
df['user_id_encoded'] = enc.fit_transform(df['user_id'])
df['vertical_encoded'] = enc.fit_transform(df['vertical'])
```

```
   user_id          vertical  user_id_encoded  vertical_encoded
0        1              work                0                 2
1       10           student                2                 1
2        5  professional-use                1                 0
3       11           student                3                 1
```

Because we're calling `fit_transform` on each column, it's refitting the label encoder to each column's values (and not using the previous fitting).

Downside of label encoding is that "the numeric values can be misinterpreted by algorithms as having some sort of hierarchy/order in them".

# OneHotEncoder

OneHotEncoder converts a column of values and encodes the values in their own columns:

### Integers

```python
import pandas as pd
import numpy as np
from sklearn.preprocessing import OneHotEncoder

df = pd.DataFrame({
    "vertical_code": [1, 3, 1, 2]
})

ohe = OneHotEncoder(sparse=False)
ohe_df = pd.DataFrame(ohe.fit_transform(df[['vertical_code']]))
df = df.join(ohe_df)
print(df)
```

```
   vertical_code    0    1    2
0              1  1.0  0.0  0.0
1              3  0.0  0.0  1.0
2              1  1.0  0.0  0.0
3              2  0.0  1.0  0.0
```

### String example

```
import pandas as pd
import numpy as np
from sklearn.preprocessing import OneHotEncoder

df = pd.DataFrame({
    "vertical": ['work','student','professional-use','student']
})

# sparse=False will cause it to return an array (vs a sparse matrix otherwise)
ohe = OneHotEncoder(sparse=False)
ohe_df = pd.DataFrame(ohe.fit_transform(df[['vertical']]))
df = df.join(ohe_df)
print(df)
```

```
           vertical    0    1    2
0              work  0.0  0.0  1.0
1           student  0.0  1.0  0.0
2  professional-use  1.0  0.0  0.0
3           student  0.0  1.0  0.0
```

### Unencoded values

```python
import pandas as pd
import numpy as np
from sklearn.preprocessing import OneHotEncoder

ohe = OneHotEncoder(sparse=False)

print("Transformation on fitted data:")
s1 = np.array([1, 3, 1, 2]).reshape(-1, 1)
enc = ohe.fit(s1)
transformed1 = enc.transform(s1)
print(transformed1)

print("\nTransformation on new data with encoded values:")
s2 = np.array([1]).reshape(-1, 1)
transformed2 = enc.transform(s2)
print(transformed2)

# As-is, this will result in an error
# We can avoid the error by passing handle_unknown="ignore" to the OneHotEncoder constractuor
print("\nTransformation on new data with unencoded values:")
s3 = np.array([5]).reshape(-1, 1)
transformed3 = enc.transform(s3).toarray()
```

```
Transformation on fitted data:
[[1. 0. 0.]
 [0. 0. 1.]
 [1. 0. 0.]
 [0. 1. 0.]]

Transformation on new data with encoded values:
[[1. 0. 0.]]

Transformation on new data with unencoded values:

ValueError: Found unknown categories [5] in column 0 during transform
```
