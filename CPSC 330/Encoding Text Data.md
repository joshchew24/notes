# Encoding Text Data
- feature is in the form of raw text
	- OHE doesn't work, no fixed number of categories
		- each category/feature value is likely to occur only once
- many popular representations
	- bag of words
	- [TF-IDF](https://github.com/cheillie/CS3245)
	- Embedding representations
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