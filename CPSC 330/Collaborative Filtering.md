# Collaborative Filtering
- [[Unsupervised Learning]]
- only uses user-item interactions given in the ratings matrix
	- i.e. ignores all info besides ratings
- intuition
	- may have **similar users and items** which can help predict missing values
	- **leverage social information** to provide recommendations
- identify [[Latent Features]]
## `SVD` Example using this [[Utility Matrix#Creating a Utility Matrix Example|setup]]
```python
# install scikit-surprise
import surprise
from surprise import SVD, Dataset, Reader, accuracy

reader = Reader()
data = Dataset.load_from_df(ratings, reader)  # Load the data

# I'm being sloppy here. Probably there is a way to create validset from our already split data.
trainset, validset = surprise.model_selection.train_test_split(
    data, test_size=0.2, random_state=42
)  # Split the data

k = 10
algo = SVD(n_factors=k, random_state=42)
algo.fit(trainset)
svd_preds = algo.test(validset)
accuracy.rmse(svd_preds, verbose=True)
```
## Cross-Validation
```python
from surprise.model_selection import cross_validate

pd.DataFrame(cross_validate(algo, data, measures=["RMSE", "MAE"], cv=5, verbose=True))
```