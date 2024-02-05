# Support Vector Machines (SVM) with RBF Kernel
- superficially, SVM RBFs are like weighted $k$-NNs
	- decision boundary defined by **a set of positive and negative examples** and **their weights** together with **their similarity measure**
	- test example is labelled positive if on average it looks more like positive examples than negative ones
- differences:
	- unlike $k$-NNs, SVM RBFs **only remember the key examples (support vectors)**
		- more efficient
	- SVMs use a different similarity metric called a "kernel"
		- popular kernel is Radial Based Functions (RBFs)
	- better performance than $k$-NNs
## Decision Boundary
- think of SVM RBFs as "smooth $k$-NN"
```python
fig, axes = plt.subplots(1, 2, figsize=(16, 5))

for clf, ax in zip([knn, svm], axes):
    clf.fit(X_train, y_train)
    plot_2d_separator(
        clf, X_train.to_numpy(), fill=True, eps=0.5, ax=ax, alpha=0.4
    )
    mglearn_utils.discrete_scatter(X_train.iloc[:, 0], X_train.iloc[:, 1], y_train, ax=ax)
    ax.set_title(clf)
    ax.set_xlabel("longitude")
    ax.set_ylabel("latitude")
```
![[Pasted image 20240205014743.png]]
## Support Vectors
- each training example either is or isn't a "support vector"
	- decided during `fit
- **decision boundary only depends on support vectors**
```python
# see support vectors