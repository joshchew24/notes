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
![[Pasted image 20240212015645.png]]

