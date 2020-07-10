# Natural Language Processing

## Tfidf

```python
title_text = volunteer['title']
tfidf_vec = TfidfVectorizer()
text_tfidf = tfidf_vec.fit_transform(title_text)

y = volunteer["category_desc"]
X_train, X_test, y_train, y_test = train_test_split(text_tfidf.toarray(), y, stratify=y)

nb.fit(X_train, y_train)
score = nb.score(X_test, y_test)
```

via [DataCamp](https://campus.datacamp.com/courses/preprocessing-for-machine-learning-in-python/selecting-features-for-modeling):

```
def return_weights(vocab, original_vocab, vector, vector_index, top_n):
    zipped = dict(zip(vector[vector_index].indices, vector[vector_index].data))
    
    # Let's transform that zipped dict into a series
    zipped_series = pd.Series({vocab[i]:zipped[i] for i in vector[vector_index].indices})
    
    # Let's sort the series to pull out the top n weighted words
    zipped_index = zipped_series.sort_values(ascending=False)[:top_n].index
    return [original_vocab[i] for i in zipped_index]

print(return_weights(vocab, tfidf_vec.vocabulary_, text_tfidf, 8, 3))
```
