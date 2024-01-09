# Virtual Memory
- hardware 
	- provides fast mechanism to map from a virtual address to a physical address
- software (OS)
	- sets up hardware mappings
	- manages the allocation of physical memory
	- implements policies about how memory is shared
- machine can have less physical memory than the size of the virtual address space

## Mapping
- [[Privilege Modes|OS]] should have access to things that are inaccessible to regular processes
- our mapping has to distinguish between mapping user processes and the OS
- read-only
	- mappings should disallow writes to some parts of memory
- data should not be executable
	- mappings should disallow execution of some parts of memory

- hardware maps these to a physical address
	- virtual address
	- type of access (r, w, e)
	- privilege level
- hardware either
	- produces physical address
	- faults
		- when hardware faults, software/OS takes over
- Divide virtual address space and memory into pages
	- x86: 4KB pages
		- require 12-bit offset
	- virtual addresses are divided into VPN and offset within the page
- mapping is cached in MMU
	- simplest implementation is [[Translation Lookaside Buffers (TLB)]]

## Faults
- virtual address invalid (no mapping)
	- segfault
	- nothing allocated to that part of the address space
	- i.e. sparse part of the VAS
	- OS terminates the process
- invalid access type
	- e.g. writing to page whose write bit is 0
		1. process is only allowed to read the page (e.g. shared library)
			- OS terminates the process
		2. page is shared by another process
			- e.g. when fork creates a child process
			- OS does not create a new copy right away
			- waits for page to be written to before creating a copy
			- after creating a copy, both parent and child's write bits are set to 1
		3. page is known to contain garbage
			- e.g. new page added to heap after `malloc()`
			- e.g. new page needed for the stack
			- first time we access the page, OS creates it
- inappropriate privilege level
- process is allowed to access the address, but OS needs to find it first
### Counting Page Faults
Assume a 1024-byte virtual page size. Let's say that you have an 8192-byte array and none of its data is in memory. If you access every 4th byte in the array just once, how many page faults will you experience?

Ans: pages are 1024 bytes, array is 8192 bytes. Therefore we access 8 pages, and take a fault on each one.

![[Pasted image 20231215032949.png]]
1. 