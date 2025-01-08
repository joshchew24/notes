# Ethics
- **statistical parity**
	- each group of a protected class (e.g. sex) should receive the positive outcome at equal proportions
	- e.g. proportion of loans approved for female should be equal to male
- **equal opportunity**
	- each group should get the positive outcome at equal rates, assuming that people in this group qualify
	- e.g. if a man and woman have the same level of income, they should have equal chance of getting a loan
- trade-off between rationality and bias
	- rationality: adopting effective means to achieve your desired outcome
	- bias: systemic
## Will a new technology:
- disempower **individuals vs corporations**?
	- user modelling; data mining; fostering addictive behaviours; developmental effects on children
- disempower **individuals vs governments**?
	- facilitate disinformation (deepfakes, bots masquerading as peopoe; filter bubbles); enable qualitatively new military or security tactics
- take **autonomous actions** in a way that obscures responsibility
	- autonomous weapons; self-driving cars; loan approval systems
- disproportionately affect **vulnerable/marginalized groups**
	- automated decision making tools trained in ways that may encode existing biases
## Sources of Bias in ML
### Historical Bias
- misalignment between world as it is, and the values/objectives to be encoded and propagated in a model
- normative concern with the state of the world
	- exists even with perfect sampling and feature selection
- **example: image search**
	- in 2018, 5% of Fortune 500 CEOs were women
	- should image search results for "CEO" reflect that number?
	- stakeholders, including affected members of society, should evaluate the harms and make a judgement
	- decision may be at odds with available data, even if the data is a perfect reflection of the world
### Representation Bias
- arises when defining and sampling a development population
	- occurs when development population under-represents, and subsequently fails to generalize well, for some part of the use population
1. **the sampling methods only reach a portion of the population**
	- e.g. datasets collected through smartphone apps can under-represent lower-income and older-age groups
		- less likely to own smartphones
	- e.g. medical data for particular condition is only available for patients considered serious enough for further screening
2. **population of interest has changed, or is distinct from population used during model training**
	- data sampled from 30 years ago won't reflect today's population
	- data from one city won't necessarily represent another city within the same region/province/country
### Measurement Bias
- arises when choosing and measuring features and labels to use
	- often proxies for the desired quantities
	- chosen set of features/labels may leave out important factors
		- or introduce group/input dependent noise that leads to differential performance
1. **measurement process varies across groups**
	- a particular group of factory workers is more stringently/frequently monitored
		- more errors will be observed
		- apparent higher rate of mistakes leads to further monitoring (feedback loop)
2. **quality of data varies across groups**
	- structural discrimination can lead to systematically higher error rates in a certain group
	- e.g. women more likely to be misdiagnosed or not diagnosed for conditions where self-reported pain is a symptom
3. **defined classification task is an oversimplification**
	- for [[Supervised Learning]], we must choose a label to predict
	- reducing a decision to a single attribute can create a biased proxy label
		- it only captures a particular aspect of what we really want to measure
	- e.g. predicting if a student will be "successful"