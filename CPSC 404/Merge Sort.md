# Merge Sort
- Idea: given $B$ buffer pages of main memory (RAM)
- sort phase:
	- read in $B$ pages of data each time and sort in memory
	- suppose entire data is stored in $m \times B$ pages
	- sort phase produces $m$ **sorted runs (sublists)** of $B$ pages each
		- each **sorted sublist (SSL)** is locally sorted
- merge phase:
	- merge $m$ SSLs into single sorted file using repetition
	- process:
		- pass 0: sort phase
		- pass 1: produces $\frac{m}{2}$ sorted runs of $2B$ pages each
		- pass 2: produces $\frac{m}{4}$ sorted runs of $4B$ pages each
		- continue until 1 sorted run of $m\times B$ pages produced
	- 2-way merge can be optimized to k-way merge
		- k can be as large as $B-1$
## Example
- what is the min # of pages of RAM needed to sort a file with 102 pages in only two passes?
- 