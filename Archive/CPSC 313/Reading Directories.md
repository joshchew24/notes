---
tags:
  - cpsc313
---
# Reading Directories
directories (folders) as structured files: we impose structure on top of the byte-stream abstraction that files provide
i.e. collection of file system objects 
### opendir
`DIR *opendir(const char *name);`
opens a directory, returning a handle
### readdir
`struct dirent *readdir(DIR *dirp);`
reads a directory entry from given directory handle

### `struct dirent`
```c
struct dirent {  
	ino_t d_ino; /* file number of entry */  
	__uint16_t d_reclen; /* length of this record */  
	__uint8_t d_type; /* file type, see below */
	__uint8_t d_namlen; /* length of string in d_name */  
	char d_name[255 + 1]; /* maximum name length */  
};
```

