# Word Embeddings
- create representations of words so that distances between words in the feature space indicate relationships between them
- useful to capture word meaning 
	- in terms of relationships between words
	- draw useful inferences to solve meaning-related problems
	- find relationships between words
		- which are similar
		- which have positive/negative connotations
## Similarity Measures
- numeric features use [[Euclidean Distance]] 
- sparse features use [[Dot Product]] or [[Cosine Distance]]
![[Pasted image 20240402201249.png]]
![[Pasted image 20240402201255.png]]
- Item A has the largest norm, and is ranked higher according to the dot-product. Item C has the smallest angle with the query, and is thus ranked first according to the cosine similarity. Item B is physically closest to the query so Euclidean distance favors it.
## Representations
- [[One-Hot Representation]]
- [[Term-Term Co-occurrence Matrix]]

A number of pre-trained word embeddings are available. The most popular ones are:  
- [word2vec](https://code.google.com/archive/p/word2vec/)
    * trained on several corpora using the word2vec algorithm 
- [wikipedia2vec](https://wikipedia2vec.github.io/wikipedia2vec/pretrained/)
    * pretrained embeddings for 12 languages 
- [GloVe](https://nlp.stanford.edu/projects/glove/)
    * trained using [the GloVe algorithm](https://nlp.stanford.edu/pubs/glove.pdf) 
    * published by Stanford University 
- [fastText pre-trained embeddings for 294 languages](https://fasttext.cc/docs/en/pretrained-vectors.html) 
    * trained using [the fastText algorithm](http://aclweb.org/anthology/Q17-1010)
    * published by Facebook    

- You can download pre-trained embeddings from their original source. 
- `Gensim` provides an api to conveniently load them. You can install `gensim` as follows. 
	- `conda install -n cpsc330 -c anaconda gensim`
	- e.g. 
```python
import gensim
import gensim.downloader as api

print(list(api.info()["models"].keys()))

google_news_vectors = api.load('word2vec-google-news-300')

print("Size of vocabulary: ", len(google_news_vectors))
```

## Document Representation using Word Embeddings
- Assuming that we have reasonable representations of words. 
- How do we represent meaning of paragraphs or documents?
- Two simple approaches
    - Averaging embeddings
    - Concatenating embeddings

### Averaging embeddings
> "All empty promises"   

$(embedding(all) + embedding(empty) + embedding(promise))/3$

- We can do this conveniently with [spaCy](https://spacy.io/usage/linguistic-features#vectors-similarity). 
- We need `en_core_web_md` model to access word vectors. 
- You can download the model by going to command line and in your course `conda` environment and download `en_core_web_md` as follows.   

```sh
conda activate cpsc330
python -m spacy download en_core_web_md
```

```python
s = "All empty promises"
doc = nlp(s)
avg_sent_emb = doc.vector
print(avg_sent_emb.shape)
print("Vector for: {}\n{}".format((s), (avg_sent_emb[0:10])))
```