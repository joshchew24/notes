- using base example from [[Customer Churn]] and data prep from [[Survival Analysis]]
## Training
```python
kmf = lifelines.KaplanMeierFitter()
kmf.fit(train_df_surv["tenure"], train_df_surv["Churn"]);

kmf.survival_function_.plot();
plt.title("Survival function of customer churn")
plt.xlabel("Time with service (months)")
plt.ylabel("Survival probability");

# this function gives plot with "error bars"
# kmf.plot_survival_function()
```
![[Pasted image 20240417195942.png|325]]
- probability of survival (not churning) over time
- large drop at beginning suggest people leave early on
	- possibly due to leaving after free trial
## Sub-groups
```python
T = train_df_surv["tenure"]
E = train_df_surv["Churn"]
senior = train_df_surv["SeniorCitizen"] == 1

ax = plt.subplot(111)

kmf.fit(T[senior], event_observed=E[senior], label="Senior Citizens")
kmf.plot_survival_function(ax=ax)

kmf.fit(T[~senior], event_observed=E[~senior], label="Non-Senior Citizens")
kmf.plot_survival_function(ax=ax)

plt.ylim(0, 1)
plt.xlabel("Time with service (months)")
plt.ylabel("Survival probability");
```
![[Pasted image 20240417200146.png|400]]
Seniors churn quicker than others