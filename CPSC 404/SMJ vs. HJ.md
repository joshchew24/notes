# SMJ vs. HJ
- both **cost** $3(M+N)$ I/Os
- minimum buffer size:
	- [[Sort Merge Join|SMJ]] simple sufficient condition
		- $B > \sqrt{max(M,N)}$
	- [[Hash Join]] simple sufficient condition
		- $B > \sqrt{min(M,N)}$
