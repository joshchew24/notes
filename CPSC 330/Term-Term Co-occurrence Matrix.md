# Term-Term Co-occurrence Matrix
- based on [[Distributional Hypothesis]]
- keep a count of words that appear within a context of each word
- word vectors are long and sparse
    - length |V| is usually large (e.g., > 50,000) 
    - most elements are zero

Visualization of Word Vector and Similarity
![[Pasted image 20240402204434.png|Jurafsky and Martin 3rd edition]]

## Small Example
```python
corpus = [
    "How tall is Machu Picchu?",
    "Machu Picchu is 13.164 degrees south of the equator.",
    "The official height of Machu Picchu is 2,430 m.",
    "Machu Picchu is 80 kilometres (50 miles) northwest of Cusco.",
    "It is 80 kilometres (50 miles) northwest of Cusco, on the crest of the mountain Machu Picchu, located about 2,430 metres (7,970 feet) above mean sea level, over 1,000 metres (3,300 ft) lower than Cusco, which has an elevation of 3,400 metres (11,200 ft).",
]
sents = MyPreprocessor(corpus)

cm = CooccurrenceMatrix(
    sents, window_size=2
)  # Let's build term-term co-occurrence matrix for our text.
comat = cm.fit_transform()
vocab = cm.get_feature_names()
df = pd.DataFrame(comat.todense(), columns=vocab, index=vocab, dtype=np.int8)
df.head()

print_cosine_similarity(df, "tall", "height")
print_cosine_similarity(df, "tall", "official")
```
## Larger Example
```python
import wikipedia
from nltk.tokenize import sent_tokenize, word_tokenize

corpus = []

queries = [
    "Machu Picchu",
  # "Everest",
    "Sequoia sempervirens",
    "President (country)",
    "Politics Canada",
]

for i in range(len(queries)):
    sents = sent_tokenize(wikipedia.page(queries[i]).content)
    corpus.extend(sents)
print("Number of sentences in the corpus: ", len(corpus))

sents = MyPreprocessor(corpus)
cm = CooccurrenceMatrix(sents)
comat = cm.fit_transform()
vocab = cm.get_feature_names()
df = pd.DataFrame(comat.todense(), columns=vocab, index=vocab, dtype=np.int8)
df

print_cosine_similarity(df, "tall", "height")
print_cosine_similarity(df, "tall", "south")
```
