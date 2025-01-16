# Lecture 2 - External Sorting Example
## # of bytes
- $10 \times 2^{20}$ records @ 100 bytes per record
- $100 \times 10 \times 2^{20} = 1,048,576,000$ bytes
## # of pages
- $4 \times 2^{10} = 4096$ bytes per page
- $\frac{100 \times 10 \times 2^{20}}{4 \times 2^{10}} = 250 \times 2^{10} = 256,000$ pages
## # of cylinders
- 32 pages per track
- 8 tracks per cylinder
- 256 pages per cylinder
- $\lceil \frac{256000}{256} \rceil = 1000$ cylinders (size of the file)
## # of pages in Buffer Pool (BP) or RAM size
- 50 MB of RAM
- $\frac{50 \times 2^{10} \times 2^{10}}{2^2 \times 2^{10}} = 50 \times 2^{8} = 50$
- \frac{12800}