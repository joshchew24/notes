---
tags:
  - cpsc313
---
# Linux ext2 File System
- simple, more modern than v6

### Overview
- disk is divided into blocks
	- size specified when you create the file system
		- 1, 2, 4, 8 KB
		- theoretically could be larger
- superblock:
	- 1024 bytes located at byte 1024 from the beginning of the volume
		- similar contents to v6
		- `struct ext2_super_block`
- Block Descriptor Table
	- tells you where to find things within the block groups
	- disk is divided into #BlockGroups 
		- facilitates placing file data close together
		- each block group is almost like its own file system
		
## Block Group Content
	- superblock copy
		- in every group in version 0
		- in some groups in later versions
	- block descriptor table copy
	- data block bitmap (1 block)
		- this and inode bitmap limit size of block groups
		- indicates which data blocks in this group are in use
	- inode bitmap (1 block)
		- sequence of `0`s and `1`s 
			- `0` : corresponding _inode_ in the _inode_table_ is free 
			- `1` : corresponding *inode* in the *inode_table* is in use
	- inode table
		- contains number of entries allowed per group, dictated by superblock
	- data blocks (rest of the block group)

### Free Space Management: Bitmaps
- data block and inode bitmap occupy EXACTLY 1 BLOCK EACH
- a collection of bits where each bit corresponds to one object
- the value of the bit indicates if the object is in use or available/free
- helpful for efficient allocation of fix-sized objects
- example 1
	- given 512-byte blocks, how many bits?
		- 512 bytes * 8 bits/byte = 4096 bits/block
		- if the inode bitmap is one block, then we can have up to 4096 inodes in a block group
	- given that inodes are 64 bytes, how many blocks does this require?
		- 4096 inodes * 64 bytes / 512 bytes/block = 512 blocks of inodes
- example 2
	- blocks are 8192 bytes, inodes are 128 bytes
	- how many bits fit in a block?
	- how much data can we have in a block group?
	- what is the maximum number of inodes?
	- how many blocks will it take to hold our max number of inodes
	
## Per-File Metadata 
### Data blocks (on-disk)
- hybrid index (15 pointers total)
	- 12 direct pointers
	- 1 indirect, 1 double, 1 triple

## Directory Entries
```c
struct ext2_dir_entry {  
	__le32 inode; /* Inode number */  
	__le16 rec_len; /* Directory entry length */  
	__le16 name_len; /* Name length */  
	char name[256]; /* File name */  
}
```
- min dirent size: 4 + 2 + 2 = 8 bytes
	- cannot actually be 8 bytes, filenames can't be nothing (0 bytes)
- **Directory entries are 4-byte aligned**
	- enforced through padding
	- no guarantee of a null terminator at end of name
	- e.g. 13-bytes gets rounded up to 16
