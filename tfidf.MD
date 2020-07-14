# Term Frequency Inverse Document Frequency (tf-idf)

* Rescale features by how informative we expect them to be
* Give weight to any term that appears often in a particular document, but not in many documents
* `TfidfVectorizer` takes the text data and does both the bag-of-words feature extraction and tf-idf transformation

## Getting Started

```python
from sklearn.feature_extraction.text import TfidfVectorizer

df = pd.DataFrame({
    "words": ["Sola runs", "Sola is a dog", "Sola chews toys"]
})

vectorizer = TfidfVectorizer()
X = vectorizer.fit_transform(df["words"])
```

`X` here is a sparse matrix with one row per document and one column per unique word in the corpus (all the words in all the documents).

```python
X.todense()
```

```
[[0.         0.         0.         0.861037   0.50854232 0.        ]
 [0.         0.65249088 0.65249088 0.         0.38537163 0.        ]
 [0.65249088 0.         0.         0.         0.38537163 0.65249088]]
```

We can see which columns represent which words:

```python
vectorizer.get_feature_names()
```

```
['chews', 'dog', 'is', 'runs', 'sola', 'toys']
```

We can see, for example, that the first row (representing "Sola runs") has values in the "runs" and "Sola" columns.

Together these let us construct a data frame if we want:

```python
pd.DataFrame(X.todense(), columns=vectorizer.get_feature_names())
```

```
      chews       dog        is      runs      sola      toys
0  0.000000  0.000000  0.000000  0.861037  0.508542  0.000000
1  0.000000  0.652491  0.652491  0.000000  0.385372  0.000000
2  0.652491  0.000000  0.000000  0.000000  0.385372  0.652491
```

## Accessing the vocabulary

> The attribute `vocabulary_` outputs a dictionary in which all ngrams are the dictionary keys and the respective values are the column positions of each ngram (feature) in the tfidf matrix. [#](https://stackoverflow.com/a/54338182/156835)

```python
vectorizer.vocabulary_
```

```
{'sola': 4, 'runs': 3, 'is': 2, 'dog': 1, 'chews': 0, 'toys': 5}
```

We can swap the keys and values to map the column number to the word:

```python
vocab = {v:k for k, v in vectorizer.vocabulary_.items()}
```

```
{4: 'sola', 3: 'runs', 2: 'is', 1: 'dog', 0: 'chews', 5: 'toys'}
```

## Accessing document data

For any given document, we can access the word indices and tf-idt weights:

```python
X[0].indeces
```

```
[3, 4]
```

```python
X[0].data
```

```
[0.861037, 0.50854232]
```

We can `zip` these together and convert them into a `dict`:

```python
dict(zip(X[0].indices, X[0].data))
```

```
{3: 0.8610369959439764, 4: 0.5085423203783267}
```

## Filtering a tf-idf vector

The problem with the tf-idf vector is that it may contain thousands of words depending on the size of the corpus.

We don't want to train a model on using all of those features (the words). Instead, we can identify the most `top_n` most important terms in each document and then filter the tf-idf vector so it only contains the columns for terms that are in the `top_n` for any of the documents.

This code is inspired via DataCamp's [Selecting Features for Modeling course](https://campus.datacamp.com/courses/preprocessing-for-machine-learning-in-python/selecting-features-for-modeling):

This function returns the indeces of the `top_n` terms in a given row.

```python
def return_indeces_of_top_terms_in_document(vocab, original_vocab, vector, vector_index, top_n):
    zipped = dict(zip(vector[vector_index].indices, vector[vector_index].data))
    zipped_series = pd.Series({vocab[i]:zipped[i] for i in vector[vector_index].indices})
    zipped_index = zipped_series.sort_values(ascending=False)[:top_n].index
    return [original_vocab[i] for i in zipped_index]
```

For example:

```python
return_indeces_of_top_terms_in_document(vocab, vectorizer.vocabulary_, X, 0, 1)
```

```
[3]
```

It returns 3 because in the first row (index `0`), we're looking at the top `1` weight, which is `0.861037`, which is in the fourth column (index `3`).

If instead we looked at the top `2` terms:

```python
return_indeces_of_top_terms_in_document(vocab, vectorizer.vocabulary_, X, 0, 2)
```

```
[3, 4]
```

Because the two terms (`Sola` and `runs`) are in the fourth and fifth columns (index `3` and `4`).

The second column has a tie in weights between `dog` and `is` (both have a weight of `0.652491`) but `sort_values` winds up putting `dog` first for some reason, so this returns index `2`:

```python
return_weights(vocab, vectorizer.vocabulary_, X, 1, 1)
```

```
[2]
```

Using this, we can iterate over all of the rows (documents) and determine the `top_n` words. `return_top_term_indeces` returns their indeces. 

```python
def return_top_term_indeces(vocab, original_vocab, vector, top_n):
    filter_list = []
    for i in range(0, vector.shape[0]):
        filtered = return_weights(vocab, original_vocab, vector, i, top_n)
        filter_list.extend(filtered)

return set(filter_list)
```

For example, if we specify `top_n` is 1, then it iterates over each row and grabs the index for the highest weighted term:

```python
indeces = return_top_term_indeces(vocab, vectorizer.vocabulary_, X, 1)
```

```
{0, 2, 3}
```

This returns a set of `{0, 2, 3}` because the in row 1 the highest weighted term is at index 3 (`Sola`), row 2 at index 2 (`dog`), row 3 at index 0 (`chews`).

Then we can filter the original vector down to just these important terms:

```python
X_filtered = X[:, list(indeces)]
X_filtered.todense()
```

```
[[0.         0.         0.861037  ]
 [0.         0.65249088 0.        ]
 [0.65249088 0.         0.        ]]
```

Putting it all together:

```python
def return_indeces_of_top_terms_in_document(vocab, original_vocab, vector, vector_index, top_n):
    zipped = dict(zip(vector[vector_index].indices, vector[vector_index].data))    
    zipped_series = pd.Series({vocab[i]:zipped[i] for i in vector[vector_index].indices})
    zipped_index = zipped_series.sort_values(ascending=False)[:top_n].index
    return [original_vocab[i] for i in zipped_index]

def return_top_term_indeces(vocab, original_vocab, vector, top_n):
    filter_list = []
    for i in range(0, vector.shape[0]):    
        filtered = return_indeces_of_top_terms_in_document(vocab, original_vocab, vector, i, top_n)
        filter_list.extend(filtered)
    return set(filter_list)

def select_highest_weighted_terms(vectorizer, X, top_n):
    vocab = {v:k for k, v in vectorizer.vocabulary_.items()}

    indeces = return_top_term_indeces(vocab, vectorizer.vocabulary_, X, 1)
    return X[:, list(indeces)]

df = pd.DataFrame({
    "words": ["Sola runs", "Sola is a dog", "Sola chews toys"]
})

vectorizer = TfidfVectorizer()
X = vectorizer.fit_transform(df["words"])

print("Data Frame:\n{}\n".format(pd.DataFrame(X.todense(), columns=vectorizer.get_feature_names())))
print("Values:\n{}\n".format(X.todense()))

X_filtered = select_highest_weighted_terms(vectorizer, X, 1)
print("Filtered values:\n{}\n".format(X_filtered.todense()))
```

```
Data Frame:
      chews       dog        is      runs      sola      toys
0  0.000000  0.000000  0.000000  0.861037  0.508542  0.000000
1  0.000000  0.652491  0.652491  0.000000  0.385372  0.000000
2  0.652491  0.000000  0.000000  0.000000  0.385372  0.652491

Values:
[[0.         0.         0.         0.861037   0.50854232 0.        ]
 [0.         0.65249088 0.65249088 0.         0.38537163 0.        ]
 [0.65249088 0.         0.         0.         0.38537163 0.65249088]]

Filtered values:
[[0.         0.         0.861037  ]
 [0.         0.65249088 0.        ]
 [0.65249088 0.         0.        ]]
```

## Fitting a model

We can either fit a model to all of the words or just the most important ones (using the technique above to limit it to the `top_n` in each document):

```python
y = df["converted"]
X_train, X_test, y_train, y_test = train_test_split(X.toarray(), y, stratify=y)

nb.fit(X_train, y_train)
score = nb.score(X_test, y_test)
```
