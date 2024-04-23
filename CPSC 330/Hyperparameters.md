# Hyperparameters
- knobs that are set to control the learning
- based on expert knowledge, heuristics, or systematic/automated optimization
## Manual Optimization
- Advantage: we may have some intuition about what might work.
	- E.g. if I'm massively overfitting, try decreasing `max_depth` or `C`.
- Disadvantages
	- it takes a lot of work
	- not reproducible
	- in very complicated cases, our intuition might be worse than a data-driven approach
## Automated Optimization
- Formulate the hyperparameter optimization as a **one big search problem**. 
- Often we have many hyperparameters of different types: Categorical, integer, and continuous.
- Often, the search space is quite big and systematic search for optimal values is infeasible.
	- i.e. we have to train models for EVERY combination of EVERY hyperparameter we want to test, which can take a really long time
		- also cross-validation means each combination has to train multiple models
### Exhaustive Grid Search
`sklearn.model_selection.GridSearchCV` (`CV` stands for cross-validation, which is built-in)
![[Pasted image 20240212015645.png]]
- For `GridSearchCV` we need
    - an instantiated model or a pipeline
    - a parameter grid: A user specifies a set of values for each hyperparameter. 
    - other optional arguments 
- The method considers product of the sets and evaluates each combination one by one.    
- behaves like a classifier: we can call `fit`, `predict`, `score`

```python
from sklearn.model_selection import GridSearchCV

pipe_svm = make_pipeline(preprocessor, SVC())

param_grid = {
    "columntransformer__countvectorizer__max_features": [100, 200, 400, 800, 1000, 2000],
    "svc__gamma": [0.001, 0.01, 0.1, 1.0, 10, 100],
    "svc__C": [0.001, 0.01, 0.1, 1.0, 10, 100],
}

# Create a grid search object 
gs = GridSearchCV(pipe_svm, 
                  param_grid = param_grid, 
                  n_jobs=-1, 
                  return_train_score=True
                 )
gs.fit(X_train, y_train)
gs.best_score_
gs.best_params_
```
#### Visualize CV Results
```python
# how many CV experiments?
pd.DataFrame(param_grid)
np.prod(list(map(len, param_grid.values())))
# visualize CV results
results = pd.DataFrame(gs.cv_results_)
results.T
```
#### Parallel Jobs `n_jobs`
- Note the `n_jobs=-1` above.
- Hyperparameter optimization can be done _in parallel_ for each of the configurations.
- This is very useful when scaling up to large numbers of machines in the cloud.
- When you set `n_jobs=-1`, it means that you want to use all available CPU cores for the task.
#### `__` Syntax
- Above: we have a nesting of transformers.
- We can access the parameters of the "inner" objects by using __ to go "deeper":
- `svc__gamma`: the `gamma` of the `svc` of the pipeline
- `svc__C`: the `C` of the `svc` of the pipeline
- `columntransformer__countvectorizer__max_features`: the `max_features` hyperparameter of `CountVectorizer` in the column transformer `preprocessor`. 
#### Range of `C`
- Note the exponential range for `C`. This is quite common. Using this exponential range allows you to explore a wide range of values efficiently.
- There is no point trying $C=\{1,2,3\ldots,100\}$ because $C=1,2,3$ are too similar to each other.
- Often we're trying to find an order of magnitude, e.g. $C=\{0.01,0.1,1,10,100\}$. 
- We can also write that as $C=\{10^{-2},10^{-1},10^0,10^1,10^2\}$. 
- Or, in other words, $C$ values to try are $10^n$ for $n=-2,-1,0,1,2$ which is basically what we have above.
#### Visualizing the parameter grid as a heatmap 
```python
def display_heatmap(param_grid, pipe, X_train, y_train):
    grid_search = GridSearchCV(
        pipe, param_grid, cv=5, n_jobs=-1, return_train_score=True
    )
    grid_search.fit(X_train, y_train)
    results = pd.DataFrame(grid_search.cv_results_)
    scores = np.array(results.mean_test_score).reshape(6, 6)
    # plot the mean cross-validation scores
    my_heatmap(
        scores,
        xlabel="gamma",
        xticklabels=param_grid["svc__gamma"],
        ylabel="C",
        yticklabels=param_grid["svc__C"],
        cmap="viridis",
    );
```
#### Problems with exhaustive grid search 

- Required number of models to evaluate grows **exponentially with the dimensionality** of the configuration space. 
- Example: Suppose you have
    - 5 hyperparameters 
    - 10 different values for each hyperparameter
    - You'll be evaluating $10^5=100,000$ models! That is you'll be calling `cross_validate` $100,000$ times!
- Exhaustive search may become **infeasible fairly quickly**.
- Other options?


### Randomized Hyperparameter Search
- `sklearn.model-_selection.RandomizedSearchCV`
- samples configurations at **random until certain budget** (e.g. time) is exhausted
```python
from sklearn.model_selection import RandomizedSearchCV

param_grid = {
    "columntransformer__countvectorizer__max_features": [100, 200, 400, 800, 1000, 2000],
    "svc__gamma": [0.001, 0.01, 0.1, 1.0, 10, 100],
    "svc__C": np.linspace(2, 3, 6),
}

print("Grid size: %d" % (np.prod(list(map(len, param_grid.values())))))
param_grid

# Create a random search object
random_search = RandomizedSearchCV(pipe_svm,                                    
                  param_distributions = param_grid, 
                  n_iter=100, 
                  n_jobs=-1, 
                  return_train_score=True)

# Carry out the search
random_search.fit(X_train, y_train)
```
- can pass a probability distribution to random search
#### Advantages
- faster compared to `GridSearchCV`
- adding parameters that do not influence performance does not affect efficiency
- works better when some parameters are more important than others
	- all features are varied in every run
	- does not waste iterations for a single variation on unimportant feature
	- ![[Pasted image 20240422181832.png|450]]
### Optimization Bias / Overfitting of the Validation Set
- why do we need to evaluate the model on the test set in the end?
	- why not just use cross-validation on the whole dataset
	- because it's possible to overfit
#### Optimization Bias of Parameter learning
- overfitting of the training error
- e.g.
	- during training, search over tons of different decision trees
	- get lucky and find a tree with low training error
#### Optimization Bias of Hyper-parameter Learning
- overfitting of the validation error
- e.g.
	- optimize the validation error over 1000 values of `max_depth`
	- one of the 1000 trees might have low validation error by chance
### Using Multiple Metrics
- can use multiple metrics with `GridSearchCV` and `RandomizedSearchCV`
- need to set `refit` to the metric (string) for which the `best_params_` will be found and used to build the `best_estimator_` on the whole dataset
```python
search_multi = GridSearchCV(
    pipe_ridge,
    param_grid,
    return_train_score=True,
    n_jobs=-1,
    scoring=scoring,
    refit="sklearn MAPE",
)
search_multi.fit(X_train, y_train);

print("Best hyperparameter values: ", search_multi.best_params_)
print("Best score: %0.3f" % (search_multi.best_score_))
pd.DataFrame(search_multi.cv_results_).set_index("rank_test_mape_scorer").sort_index()
```