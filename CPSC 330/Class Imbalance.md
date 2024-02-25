# Class Imbalance
## Class Imbalance
- when the dataset classes have a highly disproportional distribution of examples
- we can obtain very high accuracy scores even if we are frequently mislabelling one class with lower representation
- Real world data is often imbalanced. 
    - Our Credit Card Fraud dataset is imbalanced.
    - Ad clicking data is usually drastically imbalanced. (Only around ~0.01% ads are clicked.)
    - Spam classification datasets are also usually imbalanced.
## Addressing Imbalance
- why does it exist?
	- is one class much rarer than the other?
		- do we care about one type of error more than the other?
	- is it because of data collection methods?
		- then our test and training data come from different distributions
- sometimes, we can just ignore
