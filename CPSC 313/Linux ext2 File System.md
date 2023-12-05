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
- block group contents
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

## Directory Entries
```
struct ext2_dir_entry {  
__le32 inode; /* Inode number */  
__le16 rec_len; /* Directory entry length */  
__le16 name_len; /* Name length */  
char name[256]; /* File name */  
}
```
- min dirent size: 4 + 2 + 2 = 8 bytes
	- cannot actually be 8 bytes, filenames can't be nothing (0 bytes)
- dirents are 4-byte aligned
	- e.g. 13-bytes gets rounded up to 16