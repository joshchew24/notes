# Page Replacement
- cannot use typical cache replacement algos
	- most accesses are handled by MMU
	- OS does not see every page access
- MMU provides two things
	- access bits per PTE (use bits)
	- dirty bits per PTE
- OS will use an algorithm that uses these two bits to approximate LRU
	- try to make sure lots of clean pages available, so we don't get stuck doing writes before reads

## Clock Algorithm
1. imagine all physical pages in MMU arranged around a clock
2. a hand points to one page at any time
3. when a page is accessed, HW sets its use bit to 1
4. when we need to evict a page, look at where the hand points
	- if the use bit is 0, evict 
	- if the use bit is 1, set the use bit to 0 and advance the hand. repeat until we find a page with use bit 0
- Hits are handled purely by MMU
- Misses are trapped into OS
	- OS will find the physical page and bring it into memory
	- OS will give the page to MMU
	- MMU evicts and 

### Performance
- clock hand is moving slowly
	- virtual memory system is working efficiently
	- memory is plentiful
	- system is not taking many page faults
- clock hand is moving quickly
	- memory is fully in use
	- system is spending a lot of time moving pages to/from disk
		- thrashing