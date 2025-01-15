# In-Class Activity 1
1. You have 11 pages of available RAM space. What is the maximum size of a file, in # pages, that you can sort using the available space such that the entire sorting can be accomplished in just 2 passes (i.e., one pass for producing SSLs and one pass for merging all the SSLs together)? Show your work.

- 11 pages of RAM == max SSL size
- 10 input buffers + 1 output buffer
- can only merge 10 SSLs at once
$10 \text{ SSLs} \times 11 \text{ pages per SSL} = 110 \text{ pages}$ 

2. You have 500 pages of available RAM space which you are using to sort a file of 25,000 pages.
	1. How many SSLs would be produced from the sort phase (i.e., phase 1) if you were to use “cylindrification”, whereby each SSL is made as large as possible?
	2. Assume we do a 10-way merge. How would you allocate space for the input (aka SSL) buffers and the output buffer in phase 2 in order to minimize the #random I/Os? Show your work.

a)
$25000/500 = 50 \text{ SSLs } (500 \text{ pages each})$

b) 
size of 
