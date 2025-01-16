# Chapter 2 - External Sorting - Learning Goals
- Explain the importance of external sorting for DB applications.
- Argue that, for many DB applications, I/O time dominates CPU time when measuring complexity using elapsed time.
- Explain the difference between random I/O and sequential I/O. Compute the number of I/Os (assuming one I/O per disk request) that are required to sort a file of size N using general multiway (k-way) external mergesort, where N is the number of pages in the file, and k ≥ 2.
- Compute the number of sorted runs (or sublists) that are required to sort a large file using multiway external mergesort, including the sizes of the sorted runs (sublists) as we progress.
- Analyze the complexity and scalability of externally sorting a large file with external mergesort.
- Using the parameters for a given disk geometry, compute the number of I/Os, or estimate the elapsed time, for sorting a large file with external mergesort.