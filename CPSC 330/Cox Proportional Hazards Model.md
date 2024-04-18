# Cox Proportional Hazards Model
- using base example from [[Customer Churn]] and data prep from [[Survival Analysis]]
- the [[Kaplan-Meier Survival Curve]] only used `tenure` and `churn` features
- **commonly used model** that allows us to interpret **how features influence a censored tenure/duration**
	- like [[Linear Regression]] for [[Survival Analysis]]
		- we get a coefficient for each feature that tells us how it influences survival
	- works multiplicatively 
		- hazard ratio associated with each covariate represents a multiplicative change in the hazard rate
## Example
- We add a penalizer for ConvergenceErrors, explained in [this URL](https://lifelines.readthedocs.io/en/latest/Examples.html#problems-with-convergence-in-the-cox-proportional-hazard-model)
	- this is related to switching from `LinearRegression` to `Ridge`.
	- Adding `drop='first'` on our OHE might have helped with this.
	- i.e. adding regularization; `lifelines` adds both L1 and L2 regularization, aka elastic net
```python
cph = lifelines.CoxPHFitter(penalizer=0.1)
cph.fit(train_df_surv, duration_col="tenure", event_col="Churn")

cph_params = pd.DataFrame(cph.params_).sort_values(by="coef", ascending=False)
cph_params
cph.baseline_hazard_ # baseline hazard
cph.summary
```
### Interpreting Coefficients
- A positive coefficient indicates higher values of the feature give higher hazard rates
	- associated with worse survival
	- e.g. `Contract_Month-to-month` has a positive coefficient 
		- If the contract is month-to-month, it leads to more churn / worse survival (😔)
- A negative coefficient indicates higher values of the feature give lower hazard rates
	- associated with better survival
	- e.g. `Contract_Two year` has a negative coefficient
		- If the contract is two-year contract, it leads to less churn / better survival (😊)
### Confidence Intervals
```python
plt.figure(figsize=(10, 12))
cph.plot();
```
![[Pasted image 20240417202523.png]]