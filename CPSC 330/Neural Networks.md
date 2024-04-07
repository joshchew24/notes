# Neural Networks
- underlying tech in [[Deep Learning]]
- apply a sequence of transformations on input data
- **generalization of [[Linear Models]]** where we apply a series of transformations
![[Pasted image 20240406210525.png]]

```python
import mglearn

mglearn.plots.plot_logistic_regression_graph()
```
![[Pasted image 20240406211024.png]]
- we have 4 features $x[i]$ and 4 associated weights $w[i]$
- we add hidden layers of transformations between features and the target
	- repeat the process of computing the weighted sum multiple times
```python
mglearn.plots.plot_single_hidden_layer_graph()
mglearn.plots.plot_two_hidden_layer_graph()
```
![[Pasted image 20240406211157.png|275]]![[Pasted image 20240406211217.png|425]]
- each node is a **neuron**
	- small decision-making unit
	- takes input, processes it, produces an output
- each connection between neuron has a weight