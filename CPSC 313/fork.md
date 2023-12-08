# fork
- creates new child process that is almost identical to parent
	- parent's return value will be pid of child process
	- child process return value will be 0
	- call once, return twice
- immediately after fork call, both processes should check their return values and proceed accordingly
	- child almost always calls a member of exec family
	- parent might wait on child or go on to do something else