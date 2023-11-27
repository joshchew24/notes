---
tags:
  - cpsc313
  - 11-19
---
# V6 file system

- ancestor of most modern file systems

## Disk
- 512-byte blocks
	- block 0: boot block
		- contains info about the hard drive
		- first read at system boot
	- block 1: superblock (struct filsys)
	- blocks 2 to $\frac{Ninodes}{16} + 1$
		- sixteen 32-byte inodes per block
			- each inode can hold 8 addresses
		- Ninodes = number of inodes
	- rest of disk contains file data (and spare blocks for "bad block" handling)
		- e.g. if controller detects bad block, forwards the data request to a spare

## Superblock
```
struct	filsys
{
	int	s_isize;	    /* size in blocks of I list */
	int	s_fsize;	    /* size in blocks of entire volume */
	int	s_nfree;	    /* number of in core free blocks (0-100) */
	int	s_free[100];	/* in core free blocks */
	int	s_ninode;	    /* number of in core I nodes (0-100) */
	int	s_inode[100];	/* in core free I nodes */
	char	s_flock;	/* lock during free list manipulation */
	char	s_ilock;	/* lock during I list manipulation */
	char	s_fmod;		/* super block modified flag */
	char	s_ronly;	/* mounted read-only flag */
	int	s_time[2];	    /* current date of last update */
	int	pad[50];
};
```
### Free Space Management
- superblock contains pointers to 100 blocks (s_nfree and sfree)
	- read into memory at system start (assumption)
- 1st address is head of linked list to other blocks containing addresses of free blocks
- other 99 addresses 
- s_nfree
	- number of addresses currently in-core/in-memory that point to free blocks
	- everytime we allocate space for a new file, we decrement this number for each block allocated
	- if this number reaches 0, s_free\[0\] points to the next node in the linked list of free blocks
		- replace s_free array with the one in s_free\[0\]
- s_free
	- array of addresses of free blocks
	- index 0 stores the head of a linked list of other s_free arrays
	- when we allocate a new file, start assigning addresses at s_free\[s_nfree\], decrementing s_nfree each time
	- when we deallocated a file, increment s_nfree, then store the freed block address in s_free\[s_nfree\]
	- if s_nfree == 100 when we deallocate,
		- copy s_free into the newly freed block
		- set s_nfree to 1
		- set s_free\[0\] to the address of the newly freed block
- [v7 man reference](https://www.unix.com/man-page/v7/5/filsys/#:~:text=The%20%20free%20%20list,and%20%20increment%0A%20%20%20%20%20%20%20s_nfree.)

## inode: Per-file Metadata (on-disk)