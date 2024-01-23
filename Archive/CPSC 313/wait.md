# wait
`pid_t wait(int *stat_loc))`
- calling process suspends execution until a child process terminates
- returns `pid` of the terminated process
- upon return, `stat_loc` contains info about terminated process