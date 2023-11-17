---
tags: cpsc313
---

# File Metadata
- POSIX provides two+ APIs to obtain information about a file's metadata
	- many variants
	- returns file metadata via passed `struct stat` pointer
		- which device this file's inode resides on
		- **inode number**
		- inode protection mode
		- number of hard links to the file
		- user-id of owner
		- group-id of owner
		- device type, for special file inode
		- time of last access
		- time of last data modification
		- time of last file status change
		- **file size in bytes**
		- **blocks allocated for file**
		- **optimal file sys I/O ops blocksize**
		- user defined flags for file
		- file generation number
- file size $\neq$ number of blocks
	- sparse files
	- internal fragmentation
	- blocks allocated for index
## stat
`int stat(const char *restrict path, struct stat *restrict buf);`
access by name
## fstat
`int fstat(ind fd, struct stat *buf);`
access by file descriptor