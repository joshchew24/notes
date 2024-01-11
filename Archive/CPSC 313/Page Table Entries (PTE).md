# Page Table Entries (PTE)
## Contents
- Physical Page Number (PPN)
	- address of actual physical chunk of memory 
	- can be invalid
- access modes
- required privilege level
## States
***If the TLB encounters an exception, it traps into the OS, OS looks up entry in page table***
1. invalid
	- there is no mapping
	- exception: segfault (usually kills the process)
2. valid and memory-resident
	- permissions dictate if the access is allowed
	- execution proceeds as normal
3. valid but not memory-resident
	- permissions dictate if access is allowed
	- VPN is valid, but page is not yet in memory
		- could require OS reading page from disk
		- could require OS creating page full of 0's
	- exception: Page fault (read page from disk then proceed as normal)