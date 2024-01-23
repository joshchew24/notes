# Traps
- fundamental mechanism that transfers control from less privileged to more privileged

## Types
### System Calls
- an application might want the OS to do something on its behalf
- process-invoked
### Exceptions
- an application unintentionally does something that requires OS assistance
	- e.g. divide by 0, read a page not in memory
- process-invoked
### Interrupts
- an async event (e.g. I/O completion)
- device-invoked

## [[Timeouts]]
- processors have timers
	- OS schedules timer interrupt on processes
	- when timer expires, generate an interrupt
- guarantees that the OS eventually runs
	- what if a process never does a system call or exception?


## Trap Handling
- at startup, OS creates trap handler table
	- indexed by trap number
	- contains address of trap handler
- OS receives a trap with a trap number