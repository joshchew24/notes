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

vec = CountVectorizer(stop_words="english")
toy_X = vec.fit_transform(toy_df["text"])
toy_X
```
