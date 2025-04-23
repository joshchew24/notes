# Symbolic Execution
- technique for exploring **sets of program executions** ***all at once***
## Concept
1. track input/unknown values as **symbolic values**
	- i.e. give names to unknowns
	- track a **symbolic state** in the analysis
	- map program variables to **symbolic expressions**
2. track set of **path conditions**
	- i.e. constraints representing **conditions** which **must be true** in order to **reach the current program point**
###T