# Seaborn Heatmap
Install with `conda install -n cpsc330 -c anaconda seaborn`

Lecture 12: SimpleFeature Correlations Heatmap
```python
import seaborn as sns

cor = pd.concat((y_train, X_train_enc), axis=1).iloc[:, :10].corr()
plt.figure(figsize=(8, 6))
sns.set(font_scale=0.8)
sns.heatmap(cor, annot=True, cmap=plt.cm.Blues);
```
