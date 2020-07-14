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

## Fitting a model

y = volunteer["category_desc"]
X_train, X_test, y_train, y_test = train_test_split(text_tfidf.toarray(), y, stratify=y)

nb.fit(X_train, y_train)
score = nb.score(X_test, y_test)
```

via [DataCamp](https://campus.datacamp.com/courses/preprocessing-for-machine-learning-in-python/selecting-features-for-modeling):

For a given row (the tf-idf weights for the terms in a document), this will return an array of the `top_n` weighted words:

```python
def return_weights(vocab, original_vocab, vector, vector_index, top_n):
    zipped = dict(zip(vector[vector_index].indices, vector[vector_index].data))
    
    # Let's transform that zipped dict into a series
    zipped_series = pd.Series({vocab[i]:zipped[i] for i in vector[vector_index].indices})
    
    # Let's sort the series to pull out the top n weighted words
    zipped_index = zipped_series.sort_values(ascending=False)[:top_n].index
    return [original_vocab[i] for i in zipped_index]

print(return_weights(vocab, tfidf_vec.vocabulary_, text_tfidf, 8, 3))
```

```python
def words_to_filter(vocab, original_vocab, vector, top_n):
    filter_list = []
    for i in range(0, vector.shape[0]):
    
        # Here we'll call the function from the previous exercise, and extend the list we're creating
        filtered = return_weights(vocab, original_vocab, vector, i, top_n)
        filter_list.extend(filtered)
    # Return the list in a set, so we don't get duplicate word indices
    return set(filter_list)

# Call the function to get the list of word indices
filtered_words = words_to_filter(vocab, tfidf_vec.vocabulary_, text_tfidf, 3)

# By converting filtered_words back to a list, we can use it to filter the columns in the text vector
filtered_text = text_tfidf[:, list(filtered_words)]
```

```python
# Split the dataset according to the class distribution of category_desc, using the filtered_text vector
train_X, test_X, train_y, test_y = train_test_split(filtered_text.toarray(), y, stratify=y)

# Fit the model to the training data
nb.fit(train_X, train_y)

# Print out the model's accuracy
print(nb.score(test_X, test_y))
```
