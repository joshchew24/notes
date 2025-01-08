# Encoding Text Data
- feature is in the form of raw text
	- OHE doesn't work, no fixed number of categories
		- each category/feature value is likely to occur only once
- many popular representations
	- bag of words
	- [TF-IDF](https://github.com/cheillie/CS3245)
	- [[Word Embeddings]]
## Bag of Words
Represents a document as a set of words and their frequencies, ignoring syntax and word order
### `CountVectorizer`
- Converts a collection of text documents to a matrix of word counts.  
- Each row represents a "document" (e.g., a text message in our example). 
- Each column represents a word in the vocabulary (the set of unique words) in the training data. 
- Each cell represents how often the word occurs in the document.       
```python
from sklearn.feature_extraction.text import CountVectorizer

vec = CountVectorizer()
X_counts = vec.fit_transform(toy_df["sms"])
bow_df = pd.DataFrame(
    X_counts.toarray(), columns=vec.get_feature_names_out(), index=toy_df["sms"]
)
bow_df
```
Unlike other transformers we are **passing a `Series`** object to `fit_transform`. For other transformers, you can define one transformer for more than one columns. But with `CountVectorizer` you need to define **separate `CountVectorizer` transformers for each text column**, if you have more than one text columns.

### Spare Matrices
- `CountVectorizer.fit_transform` returns a spare matrix because most words do not appear in a given document
- massive computational savings by only storing nonzero elements
	- some overhead from storing locations
### Hyperparameters
- `binary`
    - whether to use absence/presence feature values or counts
- `max_features`
    - only consider top `max_features` ordered by frequency in the corpus
- `max_df`
    - max "document frequency": ignore features which occur in more than `max_df` documents
- `min_df` 
    - min "document frequency": ignore features which occur in less than `min_df` documents 
- `ngram_range`
    - consider word sequences in the given range 
> [!note] `binary` and `max_features`
> - when using these hyperparameters together, binarization is done before limiting the features to `max_features`
> - we are actually looking at the document counts (in how many documents it occurs) rather than term count

### Preprocessing
- text is normalized
	- convert all words to lowercase
		- `lowercase=True`
	- remove punctuation and special characters 
		- `token_pattern ='(?u)\\b\\w\\w+\\b'`