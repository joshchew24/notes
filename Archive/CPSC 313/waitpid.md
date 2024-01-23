# waitpid
`pid_t waitpid(pid_t pid, int *stat_loc, int options)`
- `pid` indicates which process to wait on
- upon return, `stat_loc` contains info about terminated process
- `options` is commonly used to either wait for child to terminate (blocking), or checks for termination without waiting (`WNOHANG`, i.e. polling)