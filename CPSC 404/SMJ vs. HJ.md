# SMJ vs. HJ
e# SMJ vs. HJ
- both **cost** $3(M+N)$ I/Os
- minimum buffer size:
	- [[Sort Merge Join|SMJ]] simple sufficient condition
		- for efficient SMJ to work, minimum buffer size required
		- $B > \sqrt{max(M,N)}$
	- [[Hash Join]] simple sufficient condition
		- $B > \sqrt{min(M,N)}$
