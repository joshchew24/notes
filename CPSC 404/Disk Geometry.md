# Disk Geometry
![[Pasted image 20250115165015.png]]
- platter
	- spin to let disk head read (e.g. 15000 rpm)
	- not many platters per disk (8-10)
- surface
	- two surfaces per platter
- track
	- many tracks per surface (e.g. 10000s)
	- all tracks with same # (e.g. $k$) compose cylinder $k$
- sectors
	- size is fixed (decided at manufacture time)
		- variable number of sectors per track 
			- inner tracks have fewer sectors due to physical limitations
	- block/page size is a multiple of sector size
		- determined at application time
			- e.g. file system, DBMS
- disk head
	- one head per surface
	- whole arm assembly moves together to position head on desired track $k$
