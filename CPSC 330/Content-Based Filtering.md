# Content-Based Filtering
- [[Supervised Learning]]
- use other info besides just rating data
	- e.g. genre, topic, age, location, etc.
## Example
Uses a toy movie ratings dataset
```python
import surprise
from surprise import SVD, Dataset, Reader, accuracy

reader = Reader()
data = Dataset.load_from_df(toy_ratings, reader)  # Load the data

trainset, validset = surprise.model_selection.train_test_split(
    data, test_size=0.01, random_state=42
)  # Split the data

k = 2
algo = SVD(n_factors=k, random_state=42)
algo.fit(trainset)
preds = algo.test(trainset.build_testset())

from collections import defaultdict

rating_preds = defaultdict(list)
for uid, iid, true_r, est, _ in preds:
    rating_preds[uid].append((iid, est))

rating_preds
```