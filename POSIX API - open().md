# POSIX API - open()
## FLAGS
### O_CREAT
will create the file if _pathname_ doesn't exist
### O_EXCL
used together with O_CREAT. guarantees that the file being opened is being newly created. if the _pathname_ exists, returns an error. on Linux 2.6 and later, **O_EXCL** can be used without **O_CREAT** if _pathname_ refers to a block device.
### O_TRUNC
will truncate the file to length 0 if _pathname_ exists