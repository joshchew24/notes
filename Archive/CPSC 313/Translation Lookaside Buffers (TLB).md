# Translation Lookaside Buffers (TLB)
![[Pasted image 20231204215909.png]]
- simplest form of MMU
- HARDWARE cache of translations and permissions and protections
- subset of the software page table mappings


***If the TLB encounters an exception, it traps into the OS, OS looks up entry in page table***
1. invalid
	- OS kills the process (reports segfault)
2. valid and memory-resident
	- OS enters the PTE information from memory into the TLB
3. valid and not memory-resident
	1. OS reads page from disk into page table (memory)
	2. creates PTE to map VA to PA in memory
	3. either
		1. restart instruction (generates another case 2 TLB fault)
		2. first load PTE into TLB, then restart instruction