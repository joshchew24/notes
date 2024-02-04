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



