# Disk Access
## Access Time
- composed of 3 components
### Seek Time
- position disk head at desired cylinder
- 1-20 ms
	- minimum 1 ms for setup
	- max 20 ms for setup + time for arm to sweep to desired track
		- worst case is innermost to outermost (and vice versa)
	- avg seek time is $\frac{1}{3}$  
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
	- max: $1 + \frac{1}{500}(8192 -1) = 17.382 \text{ms}$
		- $\text{setup} + \text{sweep rate}\times \text{max number of tracks to move}$
	- min: $1\text{ms}$
	- avg: $1 + \frac{1}{3}\frac{8192 -1}{500} = 6.46 \text{ms}$
		- $\text{setup} + \text{avg sweep time} \times \text{sweep rate}\times \text{max number of tracks to move}$
		- avg sweep time is approx 1/3
2. max/min/avg rotational delay?
	- $r_{max}$: $\frac{1}{3840} \times 60 \times 10^3 = 15.625\text{ms}$
	- $r_{min}$: $0 \text{ms}$
	- $r_{avg}: \frac{1}{2}r_{max} = 7.8125\text{ms}$
3. transfer time for a single page? $\tau$
	- $\tau = \frac{\text{\# sectors/page}}{\text{\# sectors/track}}\times r_{max}=\frac{2^3}{2^8} = 2^{-5}$
	- $\text{\# sectors/page} = \frac{\text{block size}}{\text{sector size}} = \frac{4\times2^{10}}{2^9} = 8$
	- time for a full revolution times the fraction of a track consumed by a single page

## [[Page Prefetch]]
- fetches more data from disk into RAM to reduce I/O operations
## Data Layout
- consecutive disk blocks
	- blocks on same track; 
	- blocks on same cylinder
	- blocks on adjacent cylinder
- minimize seek and rotational delays
- first block is chosen arbitrarily (innermost vs outermost)
![[Pasted image 20250115194742.png]]
