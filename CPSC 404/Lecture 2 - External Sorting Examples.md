# Lecture 2 - External Sorting Examples
## Example 1
### # of bytes
- $10 \times 2^{20}$ records @ 100 bytes per record
- $100 \times 10 \times 2^{20} = 1,048,576,000$ bytes
### # of pages
- $4 \times 2^{10} = 4096$ bytes per page
- $\frac{100 \times 10 \times 2^{20}}{4 \times 2^{10}} = 250 \times 2^{10} = 256,000$ pages
### # of cylinders
- 32 pages per track
- 8 tracks per cylinder
- 256 pages per cylinder
- $\lceil \frac{256000}{256} \rceil = 1000$ cylinders (size of the file)
### # of pages in Buffer Pool (BP) or RAM size
- 50 MB of RAM
- $\frac{50 \times 1024K}{4K} = 50 \times 256K = 12800$ pages
### # of SSLs in sort phase of 2PMMS (two phase multi-way merge sort)
- input file is 256000 pages
- BP is 12800 pages
- $\lceil\frac{256000}{12800}\rceil = 20$ SSLs
### merge phase of 2PMMS
![[Pasted image 20250115205951.png]]
- input buffers need at least one page each
- buffer pool is 50 cylinders
- use two cylinders for each SR$_i$ 
- 10 cylinders for output buffer
- #question why does RAM have cylinders?
## Example 2
- suppose a database file containing 10,240,000 records @ 100 bytes/record
### # of records per page
- 4K pages
- cannot split pages
- $\lfloor \frac{4096}{100} \rfloor = 40$ records per page
### # of pages in file
- $\lceil \frac{10240000}{40} \rceil = 256000$ pages
- ceiling because cannot use partial page
### rest is same as [[#Example 1]]