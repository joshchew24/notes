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
- disk RPM = 3840
- block size = 4096 bytes (4 KB)
- 4 platters (2 surfaces each)
- $2^{13}=8192$ tracks per surface == # of cylinders 
- average $2^8 = 256$ sectors per track
	- take an average because each track has different num of sectors
- $2^9=512$ bytes per sector
- disk head move speed is 1 ms setup + 1 ms per 500 cylinders
1. max/min/avg seek time?
	- max: $1 \text{ ms (setup)} + \frac{1}{500}$
	- 