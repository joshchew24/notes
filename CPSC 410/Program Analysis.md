# Program Analysis
## [[Impossible 4 Criteria]]
## Options for Program Analysis Approximation
### Pessimistic
- consider more control-flow paths than possible
	- take the worse result over them
- "sound" static analyses
	- use [[Static Program Slicing]]
### Optimistic
- consider fewer contrl-flow paths than possible
- *or* consider more control-flow paths than possible
	- take the **best** result over them
- use [[Dynamic Program Slicing]]
### Consider all paths separately
- **unbounded** number of control flow paths
	- impossible/undecidable
- use [[Symbolic Execution]]
## [[Static Program Slicing]]
## [[Dynamic Program Slicing]]
## [[Symbolic Execution]]
## [[Concolic Execution]]