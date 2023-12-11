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


## x86 Page Tables
