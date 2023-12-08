# Page Table Entries (PTE)
## Contents
- Physical Page Number (PPN)
	- address of actual physical chunk of memory 
	- can be invalid
- access modes
- required privilege level
## States
1. invalid
	1. there is no mapping
	2. exception: segfault (usually kills the process)
2. valid and memory-resident
	1. permissions dictate if the access is allowed
	2. execution proceeds as normal
3. valid but not memory-resident
	1. permissions dictate if access is allowed
	2. VPN is valid, but page is not yet in memory
		1. could require OS reading page from disk
		2. could require OS creating page full of 0's
	3. execption: Page fault (read page from disk then proceed as normal)