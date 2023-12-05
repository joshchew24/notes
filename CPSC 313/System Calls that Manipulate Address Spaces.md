# System Calls that Manipulate Address Spaces

- [[fork]]
	- gets a copy of parent's vmem
- [[execve]]
	- replace current process image with program image stored in file
	- i.e. vmem space gets replaced with something loaded from disk
	- [[execvp]]
