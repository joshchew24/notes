# Virtual Memory
- hardware 
	- provides fast mechanism to map from a virtual address to a physical address
- software (OS)
	- sets up hardware mappings
	- manages the allocation of physical memory
	- implements policies about how memory is shared
- machine can have less physical memory than the size of the virtual address space

## Mapping
- [[Privilege Modes | OS]] should have access to things that are inaccessible to regular processes
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
		- virtual address invalid (no mapping)
		- invalid access type
		- inappropriate privilege level
		- process is allowed to access the address, but OS needs to find it first
	- when hardware faults, software/OS takes over
- Divide virtual address space and memory into pages
	- x86: 4KB pages
		- require 12-bit offset
	- virtual addresses are divided into VPN and offset within the page
- mapping is cached in MMU
	- simplest implementation is [[Translation Lookaside Buffers (TLB)]]
