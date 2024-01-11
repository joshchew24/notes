---
tags:
  - cpsc313
  - 11-19
---
# Unix v6 file system

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
```c
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

# Per-file Metadata
NOTE: int is 2 bytes
## inode: (on-disk)
```c
struct ino {
	int i_mode;     /* File type, size, permissions */
	char i_nlink;   /* Link count */
	char i_uid;     /* Owner user id */
	char i_gid;     /* Group id */
	char i_size0;   /* most significant bits of size */
	int i_size1;    /* least sig */
	int i_addr[8];  /* Disk addresses of blocks */
	int i_atime[2]; /* Access time */
	int i_mtime[2]; /* Modified time */
}
```

- Small Files
	- 0 in i_mode's file size bit indicates "small" file
	- **Eight or fewer blocks**
	- i_addr (daddr on slides?) contains address of data blocks
	- max size
		- 8 blocks * 512 bytes per block == 4KB
- Large files
	- 1 in i_mode's file size bit
	- first seven entries contain address of indirect blocks
	- disk addresses are 2 bytes, so 256 addresses per block
		- i.e. one indirect block can access 256 addresses of data blocks * 512 bytes per data block == 128 KB of data
	- Max Size:
		- 7 entries * 128 KB / indirect block == 896 KB
- Huge files
	- 1 in i_mode's file size bit
	- 7 indirect blocks in daddr
	- daddr\[7\] contains [[File Representation (Index Types) | double indirect]] block
		- reaches 256 indirect blocks * 128 KB / indirect block == 32 MB
	- Max Size:
		- 896 KB + 32 MB

## vnode: (in-memory)
```c
struct inode {
	char i_flag;
	char i_count;   /* reference count */
	int i_dev;      /* device where inode resides */
	int i_number;   /* i number 1:1 w/device addr */
	int i_mode;
	char i_nlink;   /* directory entries*/
	char i_uid;     /* owner */
	char i_gid;     /* group of owner */
	char i_size0;   /* most significant of size */
	int i_size1;    /* least sig */
	int i_addr[8];  /* device addresses constituting file */
	int i_lastr;    /* last logical block read */
}
```
in-memory abstraction of actual inodes

# Directory Entries
- hierarchical data structure
- directory entries are fixed size 16 bytes
	- 14 bytes (right padded) of human readable name
	- 2 bytes of mapped inode number
- directory entry with inode = 0 means it is unused
