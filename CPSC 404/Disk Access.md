# Disk Access
## Access Time
- composed of 3 components
### Seek Time
- position disk head at desired cylinder
- 1-20 ms
	- minimum 1 ms for setup
	- max 20 ms for setup + time for arm to sweep to desired track
		- worst case is innermost to outermost (and vice versa)
### Rotational Delay
- wait for block to rotate under head
- 0-10ms
	- 0 if block already under head
	- 10 if need full rotation
### Transfer Time
- move data between disk and RAM
- usually no more than 1 ms per 4K (4096 byte) page
	- usually less
	- time for disk area containing page to rotate under head
- no min or max because page size is variable
	- also dependent on rotation spede

### Example: Megatron 747
