# Memory Management Unit
- takes a virtual address from CPU and maps to a physical address in DRAM
- CPU doesn't know or care if it's using VA or PA
- stores a [[Translation Lookaside Buffers (TLB)|cache]] of some of the VA to PA mappings
	- instructions are tiny and have high spatial locality
