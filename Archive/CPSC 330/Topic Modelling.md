# Topic Modelling
- summarizes major themes in a large collection of documents (a corpus)
- commonly use **unsupervised ML methods**.
## Overview of Process
- Given the hyperparameter $K$, the goal of topic modeling is to describe a set of documents using $K$ "topics".
- In unsupervised setting, the input of topic modeling is 
    - A large collection of documents
    - A value for the hyperparameter $K$ (e.g., $K = 3$)
- and the output is 
    1. Topic-words association 
        - For each topic, what words describe that topic? 
        - a topic is a mixture of words 
		- ![[Pasted image 20240402211755.png]]
    2. Document-topics association
        - For each document, what topics are expressed by the document? 
        - a document is a mixture of topics
        - ![[Pasted image 20240402213425.png]]
## Example using [[Latent Dirichlet Allocation]] Model
```python
# using toy data from cpsc330 repo
toy_df = pd.read_csv("../data/toy_lda_data.csv")
toy_df

from sklearn.feature_extraction.text import CountVectorizer

# input to the LDA topic model is a bag-of-words
vec = CountVectorizer(stop_words="english")
toy_X = vec.fit_transform(toy_df["text"])
toy_X

vocab = vec.get_feature_names_out() # vocabulary
vocab

from sklearn.decomposition import LatentDirichletAllocation

n_topics = 3 # number of topics
lda = LatentDirichletAllocation(
    n_components=n_topics, learning_method="batch", max_iter=10, random_state=0
)
lda.fit(toy_X) 
document_topics = lda.transform(toy_X)
```
- Word-topic association
    - `lda.components_` gives us the weights associated with each word for each topic. In other words, it tells us which word is important for which topic. 
- Document-topic association    
    - Calling transform on the data gives us document-topic association. 
- we can examine which words have the highest weights for which topics
```python
sorting = np.argsort(lda.components_, axis=1)[:, ::-1]
feature_names = np.array(vec.get_feature_names_out())

import mglearn

mglearn.tools.print_topics(
    topics=range(3),
    feature_names=feature_names,
    sorting=sorting,
    topics_per_chunk=5,
    n_words=10,
)
```
## [[Pipelines|Pipelining]]
1. Preprocess the corpus
2. Train LDA
3. Interpret the topics

First set up the data
```python
import wikipedia

queries = [
    "Artificial Intelligence",
    "unsupervised learning",
    "Supreme Court of Canada",
    "Peace, Order, and Good Government",
    "Canadian constitutional law",
    "ice hockey",
]
wiki_dict = {"wiki query": [], "text": []}
for i in range(len(queries)):
    wiki_dict["text"].append(wikipedia.page(queries[i]).content)
    wiki_dict["wiki query"].append(queries[i])

wiki_df = pd.DataFrame(wiki_dict)
wiki_df
```

### Preprocessing the Corpus
- **Preprocessing is crucial!**
- Tokenization, converting text to lower case
- Removing punctuation and stopwords
- Discarding words with length < threshold or word frequency < threshold        
- Possibly lemmatization/stemming: Consider the lemmas instead of inflected forms. 
- Depending upon your application, restrict to specific part of speech;
    * For example, only consider nouns, verbs, and adjectives
- We'll use [`spaCy`](https://spacy.io/) for preprocessing. Check out available token attributes [here](https://spacy.io/usage/rule-based-matching#adding-patterns-attributes). 
```python
import spacy

nlp = spacy.load("en_core_web_md", disable=["parser", "ner"])

def preprocess(
    doc,
    min_token_len=2,
    irrelevant_pos=["ADV", "PRON", "CCONJ", "PUNCT", "PART", "DET", "ADP", "SPACE"],
):
    """
    Given text, min_token_len, and irrelevant_pos carry out preprocessing of the text
    and return a preprocessed string.

    Parameters
    -------------
    doc : (spaCy doc object)
        the spacy doc object of the text
    min_token_len : (int)
        min_token_length required
    irrelevant_pos : (list)
        a list of irrelevant pos tags

    Returns
    -------------
    (str) the preprocessed text
    """

    clean_text = []

    for token in doc:
        if (
            token.is_stop == False  # Check if it's not a stopword
            and len(token) > min_token_len  # Check if the word meets minimum threshold
            and token.pos_ not in irrelevant_pos
        ):  # Check if the POS is in the acceptable POS tags
            lemma = token.lemma_  # Take the lemma of the word
            clean_text.append(lemma.lower())
    return " ".join(clean_text)

wiki_df["text_pp"] = [preprocess(text) for text in nlp.pipe(wiki_df["text"])]
wiki_df
```
## Train LDA
```python
from sklearn.feature_extraction.text import CountVectorizer

vec = CountVectorizer(stop_words='english')
X = vec.fit_transform(wiki_df["text_pp"])

from sklearn.decomposition import LatentDirichletAllocation

n_topics = 3
lda = LatentDirichletAllocation(
    n_components=n_topics, learning_method="batch", max_iter=10, random_state=0
)
document_topics = lda.fit_transform(X)

print("lda.components_.shape: {}".format(lda.components_.shape))

sorting = np.argsort(lda.components_, axis=1)[:, ::-1]
feature_names = np.array(vec.get_feature_names_out())

import mglearn

mglearn.tools.print_topics(
    topics=range(3),
    feature_names=feature_names,
    sorting=sorting,
    topics_per_chunk=5,
    n_words=10,
)
```