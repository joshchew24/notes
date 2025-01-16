# Feature Engineering
> [!Definition]
> The process of **transforming raw data into features** that better represent the underlying problem to the predictive models, resulting in improved model accuracy on unseen data

## Garbage In, Garbage Out
- Better features usually help more than a better model
- good features:
	- capture **most important aspects** of the problem
	- allow learning with **few examples**
	- **generalize** to new scenarios
- **trade-off** between **simple** and **expressive** features
	- **simple features** have lower scores with less risk of overfitting
	- **complicated features** have higher scores with more risk of overfitting
## Model Dependent
- the best features may depend on what model you use
- for counting-based methods like decision trees, seperate relevant **groups of variable values**
	- discretization
		- partitioning and converting continuous attributes into discrete intervals
		- enables the use of continuous features in algorithms that require discrete features
- for distance-based methods like kNN, we want different class labels to be "far"
	- standardization
		- avoid dominance of wide-ranging features over other features with smaller ranges
- for regression-based methods like linear regressionm we want targets to hvae a linear dependency on features
## [[Feature Cross]]
- synthetic feature formed by multiplying or crossing two or more features
## Numeric Features
- what happens if we train [[Ridge]] on a dataset without applying transformations to the categorical features?
	- error! **linear models require all features in numeric form**
### Discretization
- bucketing or binning
	- transforming numeric features into categorical features
- the model learns how each bin is associated with the target variable
	- in `Ridge`, the model learns the optimal coefficients (slopes) associated with each bin
- `KBinsDiscretizer`
```python
from sklearn.preprocessing import KBinsDiscretizer

discretization_feats = ["latitude", "longitude"]
numeric_feats = ["rooms_per_household"]

preprocessor2 = make_column_transformer(
    (KBinsDiscretizer(n_bins=20, encode="onehot"), discretization_feats),
    (make_pipeline(SimpleImputer(), StandardScaler()), numeric_feats),
)

lr_2 = make_pipeline(preprocessor2, Ridge())
pd.DataFrame(
    cross_validate(lr_2, X_train_housing, y_train_housing, return_train_score=True)
)

lr_2.fit(X_train_housing, y_train_housing)

pd.DataFrame(
    preprocessor2.fit_transform(X_train_housing).todense(),
    columns=preprocessor2.get_feature_names_out(),
)
```
Discretizing all three features:
```python
from sklearn.preprocessing import KBinsDiscretizer

discretization_feats = ["latitude", "longitude", "rooms_per_household"]

preprocessor3 = make_column_transformer(
    (KBinsDiscretizer(n_bins=20, encode="onehot"), discretization_feats),
)

lr_3 = make_pipeline(preprocessor3, Ridge())
pd.DataFrame(
    cross_validate(lr_3, X_train_housing, y_train_housing, return_train_score=True)
)

lr_3.fit(X_train_housing, y_train_housing)
```
## Text Features
- extract interesting information from text using pre-trained models
	- `nltk`
		- `SentimentIntensityAnalyzer`
	- `spaCy`
		- `displacy`
	- `spacymoji`
### Example
```python
import en_core_web_md
import spacy

nlp = en_core_web_md.load()
from spacymoji import Emoji

nlp.add_pipe("emoji", first=True)

def get_relative_length(text, TWITTER_ALLOWED_CHARS=280.0):
    """
    Returns the relative length of text.

    Parameters:
    ------
    text: (str)
    the input text

    Keyword arguments:
    ------
    TWITTER_ALLOWED_CHARS: (float)
    the denominator for finding relative length

    Returns:
    -------
    relative length of text: (float)

    """
    return len(text) / TWITTER_ALLOWED_CHARS


def get_length_in_words(text):
    """
    Returns the length of the text in words.

    Parameters:
    ------
    text: (str)
    the input text

    Returns:
    -------
    length of tokenized text: (int)

    """
    return len(nltk.word_tokenize(text))


def get_sentiment(text):
    """
    Returns the compound score representing the sentiment: -1 (most extreme negative) and +1 (most extreme positive)
    The compound score is a normalized score calculated by summing the valence scores of each word in the lexicon.

    Parameters:
    ------
    text: (str)
    the input text

    Returns:
    -------
    sentiment of the text: (str)
    """
    scores = sid.polarity_scores(text)
    return scores["compound"]

def get_avg_word_length(text):
    """
    Returns the average word length of the given text.

    Parameters:
    text -- (str)
    """
    words = text.split()
    return sum(len(word) for word in words) / len(words)


def has_emoji(text):
    """
    Returns the average word length of the given text.

    Parameters:
    text -- (str)
    """
    doc = nlp(text)
    return 1 if doc._.has_emoji else 0
    
train_df = train_df.assign(n_words=train_df["OriginalTweet"].apply(get_length_in_words))
train_df = train_df.assign(vader_sentiment=train_df["OriginalTweet"].apply(get_sentiment))
train_df = train_df.assign(rel_char_len=train_df["OriginalTweet"].apply(get_relative_length))

test_df = test_df.assign(n_words=test_df["OriginalTweet"].apply(get_length_in_words))
test_df = test_df.assign(vader_sentiment=test_df["OriginalTweet"].apply(get_sentiment))
test_df = test_df.assign(rel_char_len=test_df["OriginalTweet"].apply(get_relative_length))


train_df = train_df.assign(
    average_word_length=train_df["OriginalTweet"].apply(get_avg_word_length)
)
test_df = test_df.assign(average_word_length=test_df["OriginalTweet"].apply(get_avg_word_length))

# whether all letters are uppercase or not (all_caps)
train_df = train_df.assign(
    all_caps=train_df["OriginalTweet"].apply(lambda x: 1 if x.isupper() else 0)
)
test_df = test_df.assign(
    all_caps=test_df["OriginalTweet"].apply(lambda x: 1 if x.isupper() else 0)
)

train_df = train_df.assign(has_emoji=train_df["OriginalTweet"].apply(has_emoji))
test_df = test_df.assign(has_emoji=test_df["OriginalTweet"].apply(has_emoji))

train_df.head()

train_df.shape

(train_df['all_caps'] == 1).sum()

X_train = train_df.drop(columns=['Sentiment'])

numeric_features = ['vader_sentiment', 
                    'rel_char_len', 
                    'average_word_length']
passthrough_features = ['all_caps', 'has_emoji'] 
text_feature = 'OriginalTweet'
drop_features = ['UserName', 'ScreenName', 'Location', 'TweetAt']

preprocessor = make_column_transformer(
    (StandardScaler(), numeric_features),
    ("passthrough", passthrough_features), 
    (CountVectorizer(stop_words='english'), text_feature),
    ("drop", drop_features)
)

pipe = make_pipeline(preprocessor, LogisticRegression(max_iter=1000))
results["LR (more feats)"] = mean_std_cross_val_scores(
    pipe, X_train, y_train, return_train_score=True, scoring=scoring_metrics
)
pd.DataFrame(results).T

pipe.fit(X_train, y_train)

cv_feats = pipe.named_steps['columntransformer'].named_transformers_['countvectorizer'].get_feature_names_out().tolist()

feat_names = numeric_features + passthrough_features + cv_feats

coefs = pipe.named_steps['logisticregression'].coef_[0]

df = pd.DataFrame(
    data={
        "features": feat_names,
        "coefficients": coefs,
    }
)
df.sort_values('coefficients')
```

