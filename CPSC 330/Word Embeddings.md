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