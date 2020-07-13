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

## Accessing the vocabulary

> The attribute `vocabulary_` outputs a dictionary in which all ngrams are the dictionary keys and the respective values are the column positions of each ngram (feature) in the tfidf matrix. [#](https://stackoverflow.com/a/54338182/156835)

```python
vectorizer.vocabulary_
```

```
{'real': 5144, 'property': 4991, 'management': 3830, ... }
```

We can swap the keys and values to map the column number to the word:

```python
vocab = {v:k for k, v in vectorizer.vocabulary_.items()}
```

```
{5144: 'real', 4991: 'property', 3830: 'management', 3990, ... }
```

## Fitting a model

y = volunteer["category_desc"]
X_train, X_test, y_train, y_test = train_test_split(text_tfidf.toarray(), y, stratify=y)

nb.fit(X_train, y_train)
score = nb.score(X_test, y_test)
```

via [DataCamp](https://campus.datacamp.com/courses/preprocessing-for-machine-learning-in-python/selecting-features-for-modeling):

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
