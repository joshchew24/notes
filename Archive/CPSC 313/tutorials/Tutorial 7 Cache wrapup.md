# Tutorial 7: Cache wrapup
In this tutorial, we cover write caching, cache coherence, and maybe a bit of file wrangling.
## Write caching
1. We would like to write value 0x123 to memory at address 0x4000. We check our cache, and it is a **hit**. In each of the following scenarios, where is 0x123 written? (to cache, to memory, both, none)
   - (A) If we have a write-through cache
	   - to cache and memory
   - (B) If we have a write-back cache
	   - to cache
 
2. Now imagine we have a cache with a **write-allocate policy**.
   - (A) We would like to write value 0x123 to the data storage at address 0x8000, and this time we     experience a **miss**. Where is 0x123 written? (to cache, to memory, both, none)
	   - writethrough: to cache and memory
		   - one implementation would write the value to memory, and then pull the updated value into the cache
		   - another implementation would pull the memory to cache, write the new value, then push the value back through to memory
	   - writeback: to cache
   - (B) Does having a write-allocate or no-write-allocate policy affect where we write on a **write hit**?
	   - no because on a hit, the memory addresses being updated already exist on the cache, so no allocation occurs, and the allocation policy does not affect
	   - write-back:
		   - write-allocate: cache only
		   - no-write-allocate: cache only
	   - write-through:
		   - write-allocate: cache and memory
		   - no-write-allocate: cache and memory
   - (C) Can having a write-back or write-through policy affect where we write on a **write miss**?
	   - no-write-allocate
		   - both policies write straight through to memory
	   - write-allocate
		   - write-back: only write cache (after allocating)
		   - write-through: only write memory

2. Assume our workload involves many sequential writes to one small part of memory. What are the best strategies to use?
	1. write-back with write-allocate
		1. after loading the chunk of memory into the cache, we will update it multiple times. with write-back, we don't have to propagate the changes to memory every single time, and with write-allocate, the first miss will allocate cache space for the memory chunk to be worked with. 

4. Which of these write policies necessitates a cache coherence protocol when multiple cores are touching the same data?
	1. All policies need a coherence protocol. 
	2. write-back
		1. cache can be changed separately from memory, requiring some form of coherency protocol between cores
	3. write-through
		1. if one core updates a piece of memory, even though the changes are pushed all the way through, they still most notify other cores

  

## Cache coherence: MSI, MESI
1. Suppose we have a CPU with two cores (core 0 and core 1) that adheres to the MSI protocol. **What are the state combinations for a shared cache line that are not possible?**  Hint: there are three.
	1. M1, S2
	2. M1, M2
	3. S1, M2
2. MESI?
	1. M1, E2
	2. S1, E2
	3. E1, M2
	4. E1, S2
	5. M1, S2
	6. M1, M2
	7. S1, M2

4. Assume some dual-core CPU uses the MSI protocol. The table below asks you to consider a sequence of accesses to the same memory block. **Fill in the table with the appropriate cache metadata.**

   Recall that for MSI the states are M for modified, S for shared, and I for invalid.

|     | Event         | Core 0 | Core 1   |
| --- |:------------- |:------ |:-------- |
| (A) | Initial state | I      | I        |
| (B) | Core 0: read  | S      | I        |
| (C) | Core 1: read  | S      | S        |
| (D) | Core 1: write | I      | M        |
| (E) | Core 0: read  | S      | I (or S) |
| (F) | Core 0: write | M      | I        |
| (G) | Core 1: write | I      | M        |

3. Fill in the following table using the MESI protocol. Recall that for MESI the states are M for modified, E for exclusive, S for shared, and I for invalid.

|     | Event         | Core 0 | Core 1 |
| --- |:------------- |:------ |:------ |
| (A) | Initial state | I      | I      |
| (B) | Core 0: read  | E      | I      |
| (C) | Core 0: write | M      | I      |
| (D) | Core 1: read  | S (or I)     | S (or E)      |
| (E) | Core 0: read  | S      | S      |
| (F) | Core 1: write | I      | M      |
## Using the Linux file API
In class, we built a program to copy one file to another. In that program you were able to both read and write sequentially. Let's take a look at the `lseek` system call and then use it to write a program that **reverses** a file. 

Recall that every fd has an implicit offset, which gets updated every time you read or write. You can also manipulate that offset directly using the `lseek` system call. Read the man page for `lseek`. Then, starting with the file `copy.c`, copy it into a new file `rev.c` and transform it into a program that reads a file and writes a new file containing the contents of the first file in reverse form. (You can use the copy program provided to make a copy of copy.c .....)
A file containing:
```
The quick brown fox jumped over the lazy sleeping dog\n
```
Should produce:
```
\ngod gnipeels yzal eht revo depmuj xof nworb kciuq ehT
```
Hint: It's easier to read the input file backwards and write the output file forwards than the other way around!