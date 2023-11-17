---
tags:
  - cpsc313
---

# File System Metadata
- POSIX provides two APIs to obtain information about a (mounted) system
	- via the passed `struct statfs`, returns metadata about the file system in which the object represented by path/fd appears
		- filesystem type
		- optimal transfer block size
		- total number of blocks
		- number of free blocks
		- free blocks avail to unprivileged user
		- total number of file nodes
		- number of free file nodes
		- file system ID
		- maximum length of filenames
		- fragment size
### statfs
`int statfs(const char *path, struct statfs *buf);`
takes a filename
### fstatfs
`int fstatfs(int fd, struct statfs *buf);`
takes the `fd` for an open file

## Superblocks
If given an unmounted file system, we don't know how to interpret it. The first block of a disk is typically a #superblock which provides metadata, though we need to figure out what kind of superblock it is. 
- file systems typically replicate the superblock many times in different places
- metadata in superblock is a SUPERSET of `struct statfs`:
	- number of inodes
	- number of blocks
	- number of free inodes/blocks