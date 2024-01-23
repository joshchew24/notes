# Page Tables
- software data structure maintained by the **OPERATING SYSTEM**
- used in conjunction with [[Translation Lookaside Buffers (TLB)]]
	- when a Virtual Address is requested by a process, first check TLB
	- if mapping found in TLB (TLB hit), can directly retrieve the Physical Address
	- else (TLB Miss), consult the page table and push the PTE to the TLB
- collection of mappings that describe an address space
- divide an address space into a number of pages
	- example
		- address space ranges from 0x0 to 0xFFFF
		- divide address space into 16 pages
			- numbered 0 through F
- contains [[Page Table Entries (PTE)]]
- index the page table using virtual page number (VPN)
- virtual pages and physical pages are the same size
	- page size set by hardware (CPU)
- *maximum* size of virtual address space determined by CPU
	- number of bits (size) of a virtual page number determined by CPU
- maximum number of physical pages determined by CPU
	- number of bits (size) of a physical page number determined by CPU 
- actual number of physical pages is specific to particular machine
	- not only determined by CPU
- virtual page *numbers* and physical page *numbers* can be different sizes
	- i.e. the virtual address space can be differently sized from physical memory

## Address Translation
| VPN | PPN     | Access        | Privilege |
| --- | ------- | ------------- | --------- |
| 0   | Invalid |               |           |
| 1   | 0x0000A | Read, Execute | U         |
| 2   | 0x1100B | Read, Execute | U         |
| 3   | 0x98765 | Read, Execute | U         |
| 4   | 0xCAFE0 | Read, Execute | U         |
| 5   | 0xFACE1 | Read, Write   | U         |
| 6   | 0xC0FFF | Read, Write   | U         |
| 7   | Invalid |               |           |
| 8   | Invalid |               |           |
| 9   | Invalid |               |           |
| A   | Invalid |               |           |
| B   | Invalid |               |           |
| C   | Invalid |               |           |
| D   | 0x50505 | Read, Write   | U         |
| E   | 0x12345 | Read, Write   | U         |
| F   | 0x24680 | Read, Write   | U          |
*assume we are running in user mode*
```
xor %rax, %rax
mrmovq 0xFEED(%rax), %rbx
```
1. What virtual address are we accessing?
	- 0xFEED
2. What is the VPN to translate?
	- 0xF
3. What is the corresponding PTE?
	- 0x24680/RW/U
5. Do we have the proper permissions?
	- yes
7. What is the PA for the data?
	- 0x24680EED

## OS Page Fault Handling
*Context: Process requests a virtual address, but the mapping is not found in the TLB (there is no PTE). The TLB traps into the OS, and the OS executes the following trap handler*
1. interpret the PTE to determine if the virtual address has a backing page
	- case 1: PTE indicates that the address is invalid (validity bit)
		- segfaults
	- case 2: PTE indicates that the address is within the address space
		- continue
2. check PTE to determine if the page present is in memory (presence bit)
	- if page is present in memory
		- load it into TLB
			- TLB (MMU) will perform [[Page Replacement|eviction]]
	- case 2: page is not present
		- PTE indicates where to find the page
			- which disk, which disk block, etc.
		- or, we can 0-fill the page
3. page is not present
4. find a free physical page in memory
6. fill the page with data
	- PTE indicates where to find the data on disk
	- or can fill with zeroes
7. update PTE with physical page number
	- PPN is where the data physically exists in memory
8. restart instruction

## Implementation
### Considerations
- there is one page table per process
- the OS must keep track of all page tables for all processes
	- can be very large
		- e.g. Intel x86-64
			- 4 KB pages
				- 12 bit offset
			- 48-bit address space
				- 36 bits remaining = $2^{35}$ = 64 G pages (64 billion)
			- one PTE per page, 8 bytes per PTE
				- 512 GB per page table
- ***page tables (virtual address spaces) are sparse***
	- most processes won't use most of the address space
		- most only need a few, e.g. hello world program
			- 1 page each for text, data, stack
			- space for shared libraries (code/data)
			- total number of required pages is only a few hundred, not 64 billion
- similar problem to [[File Representation (Index Types)]]
### Requirements
- simple enough data structure to work with in hardware (at least for x86)
- efficient for small address space
- supports a large address space
## x86 Page Tables
- uses the same tree structure as a file index
- instead of inode/index, hardware register ***CR3***
	- contains physical address of L1 Page Table
- indirect blocks are now called page tables
	- page tables contain PTEs
		- can reference other in-memory physical pages
		- sometimes disk blocks
	- sizing
		- 512 PTEs
		- PTEs are 64-bits/8-bytes
		- one page table is 4KB
			- same as page size
- x86-64 has 4 levels of page tables
	- L1 Page Table is quadruple indirect
		- memory: `512 entries * 512 GB = 256 TB`
		- should have access to entire virtual address space
			- 48-bit addresses = 256 TB
	- L2 Page Table is triple indirect
		- memory: `512 entries * 1 GB = 512 GB`
	- L3 Page Table is double indirect
		- memory: `512 entries * 2 MB = 1 GB`
	- L4 Page Table is indirect
		- memory: `512 entries * 4 KB = 2 MB`
- all page tables are indexed by bits in the virtual address
- small processes only need one of each level page table, requiring only 16KB
	- all processes have 4-levels because virtual memory is inherently sparse
		- e.g. stack and heap

### Address Translation
- 64 bit address
- page table is 4KB: need 12 bits of offset within (0-11)
- first 16 bits are unused (48-63)
- middle 36 bits are VPN (12-47)
	- total VPN is 2^36
	- each PTE within a table is numbered 0-511
	- L1 index bits (39-47)
	- L2 index bits (30-38)
	- L3 index bits (21-29)
	- L4 index bits (12-20)