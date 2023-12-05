# Privilege Modes
- hardware provides at least two modes of operation
	- *User Mode*: how all "regular programs" run
	- *[[Supervisor Mode|Kernel/Supervisor Mode]]*: How the OS runs
- software can run with different privileges
- mode determines
	- what instructions may be executed
	- how addresses are translated
	- what memory locations may be accessed (enforced through translation)

## [[Traps]]