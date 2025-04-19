# Cascaded Aborts
![[Pasted image 20250324174409.png]]
- if T1 **aborts** before committing change to A, T2 will use wrong data
- value read by T2 is not correct, because it was changed by T1 which was not committed
- T2 **must** be aborted as well