# Bias and Variance Tradeoff
> [!important] Fundamental Tradeoff of Supervised Learning
> As you increase model complexity, $E_{train}$ tends to go down but $E_{valid} - E{train}$ tends to go up

## Bias
- tendency to consistently learn the same wrong thing 
- high bias corresponds to [[Underfitting]]
## Variance
- tendency to learn random things irrespective of the real signal
- high variance corresponds to [[Overfitting]]
## Sweet Spot
![[Pasted image 20240204002836.png]]
- common practice is to pick the model with lowest cross-validation error
> [!error] Golden Rule
> TEST DATA CANNOT INFLUENCE THE TRAINING PHASE IN ANY WAY
### Process
- **Splitting**: Before doing anything, split the data `X` and `y` into `X_train`, `X_test`, `y_train`, `y_test` or `train_df` and `test_df` using `train_test_split`. 
- **Select the best model using cross-validation**: Use `cross_validate` with `return_train_score = True` so that we can get access to training scores in each fold. (If we want to plot train vs validation error plots, for instance.) 
- **Scoring on test data**: Finally score on the test data with the chosen hyperparameters to examine the generalization performance.



