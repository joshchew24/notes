# execve
- must provide `path`, `argv[]`, and `envp[]`
	- `path` : pathname of command to execute
	- `argv` : argument vector
	- `envp` : environment
		- set of name/value pairs that are frequently used to communicate environmental information to processes
			- where to look for commands, what OS are we running