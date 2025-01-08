# One-Hot Representation
- see [[One-hot encoding]]
- simplest word representation
- each word is has a unique vector where all elements are zero except for one element
	- represents the index of that word in the vocabulary
	- each vector is length $V$, i.e. the size of the vocabulary
- typically wouldn't store all vectors, just a map of words to their indices
	- generate the word vectors based on the query phrase
## Problem
- does not capture similarities between different words
	- e.g. tall vs height
	- i.e. the dot product between non-equal words is always zero
## Example
"How tall is Machu_Picchu"

- one-hot representation of the word *tall*:
    * Vocabulary size = 5 and index of the word *tall* = 1
    * One-hot vector for *tall*: $\begin{bmatrix} 0 & 1 & 0 & 0 & 0 \end{bmatrix}$
```python
def get_onehot_encoding(word, vocab):
    onehot = np.zeros(len(vocab), dtype="float64")
    onehot[vocab[word]] = 1
    return onehot
```

```python
from sklearn.metrics.pairwise import cosine_similarity


def print_cosine_similarity(df, word1, word2):
    """
    Returns similarity score between word1 and word2
    Arguments
    ---------
    df    -- (pandas.DataFrame)
        Dataframe containing word representations
    word1 -- (array)
        Representation of word1
    word2 -- (array)
        Representation of word2

    Returns
    --------
    None. Returns similarity score between word1 and word2 with the given representation
    """
    vec1 = df.loc[word1].to_numpy().reshape(1, -1)
    vec2 = df.loc[word2].to_numpy().reshape(1, -1)
    sim = cosine_similarity(vec1, vec2)
    print(
        "The dot product between %s and %s: %0.2f and cosine similarity is: %0.2f"
        % (word1, word2, np.dot(vec1.flatten(), vec2.flatten()), np.squeeze(sim))
    )
```
Vocabulary and One-Hot Encoding
```python
corpus = (
    """how tall is machu_picchu ? the official height of machu_picchu is 2,430 m ."""
)
unique_words = list(set(corpus.split()))
unique_words.sort()
vocab = {word: index for index, word in enumerate(unique_words)}
print("Size of the vocabulary: %d" % (len(vocab)))
print(vocab)
```

```python
data = {}
for word in vocab:
    data[word] = get_onehot_encoding(word, vocab)
ohe_df = pd.DataFrame(data).T
ohe_df
```