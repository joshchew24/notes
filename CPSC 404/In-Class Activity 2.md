# In-Class Activity 2
## Question
You are given a file of $40\times10^6$ records, each 200 bytes long. You wish to built a B+ Tree index for this file on the primary key, using alt1 data entries (DEs). Each key is 32 bytes, and a pointer is 8 bytes. Each node of the B+ Tree is implemented as a 4k page (i.e. $4\times2^{10}$ bytes).
(a) The max # (key, pointer) pairs that a node can store
(b) Order of this B+ Tree
(c) Max # records that would fit in a page
(d) Min possible height of this B+ Tree
(e) Max possible height of this B+ Tree
*Note: for (d) & (e), do not use formulas. Instead, start with the # leaf pages needed to hold the DEs, work bottom up 