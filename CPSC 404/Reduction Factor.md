# Reduction Factor
- fraction of rows that satisfy query condition
- $RF = \frac{\text{Number of qualifying rows}}{\text{Total rows in table}}$
- [[Selectivity]] ($S$)
	- inverse of $RF$
	- $S = 1 - RF$
- high reduction factor $\equiv$ low selectivity (**less efficient**)
- high selectivity $\equiv$ low reduction factor (**more efficient**)
- often estimated using (statistical) **independence assumption**
	- i.e. assume no correlation between conditions
	- e.g. 
- **generally**, doing **selection** and **projection** before complex operations like **join** is beneficial
	- however, if these intermediaries aren't materialized, there may be no benefit
- **cardinality**
	- number of tuples remaining after applying selections
	- $\text{max \# tuples} \times \text{product of all RFs}$
## [[System R]] Conventions
- $col = value$ has RF $\frac{1}{NKeys(I)}$ given index $I$ on col
- $col1 = col2$ has RF $\frac{1}{MAX(NKeys(I1), NKeys(I2))}$
	- i.e. $RF = min(\frac{1}{NKeys(I1)},\frac{1}{Nkeys(I2)})$
- $col > value$ has RF $\frac{High(I) - value}{High(I) - Low(I)}$
- $col < value$ has RF $\frac{value - Low(I)}{High(I) - Low(I)}$
- if attribute domain is **real-valued**, bucket/discretize it
	- e.g. salary, temperature
## Example
Consider a table with **1,000,000** rows:
1. **Low Selectivity / High Reduction Factor:**
    - A filter like `WHERE status = 'active'` returns **800,000** rows.
    - **RF = 800,000 / 1,000,000 = 0.8**
    - **Selectivity = 1 - 0.8 = 0.2** (low selectivity)
2. **High Selectivity / Low Reduction Factor:**
    - A filter like `WHERE user_id = 12345` returns **only 1 row**.
    - **RF = 1 / 1,000,000 = 0.000001**
    - **Selectivity = 1 - 0.000001 ≈ 1** (high selectivity)
## Estimation
- for [[Selection Evaluation#Reduction Factor Estimation]]
	- multiple conditions, take the product of the RF of the atomic conditions
