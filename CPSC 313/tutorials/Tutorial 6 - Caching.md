# Tutorial 6 - Caching  

In this tutorial, you already know what a cache is! Now we can discuss hit rates, access speed, and cache design.
## Computing hit/miss rates 

Imagine that we have a cache with 64-byte cache lines.

1. Compute the following:
    1. What hit rate do we experience if we read the entire cache line in units of 2-byte short integers?
	    2. $\frac{62}{64}=\frac{31}{32}$
    2. What hit rate do we experience if we read the entire cache line in units of 4-byte integers?
	    1. $\frac{60}{64}=\frac{15}{16}$
    3. What hit rate do we experience if we read the entire cache line in units of 8-byte long integers?
	    1. $\frac{56}{64}=\frac{7}{8}$
2. From (1), find a general formula for hit rate in terms of the data size being accessed and the cache line size.
	1. let $s$ = data size being accessed
	2. let $n$ = cache line size
	3. then hit rate $h = \frac{n-s}{n}$ 
3. What hit rate do we experience if we read 64 bytes, one byte at a time, beginning at the address 0xC8? Remember alignment.
	1. 0xC8 = 16\*12 + 8 = 192 + 8 = 200
	2. 192 == 64\*3, i = 2
	3. we cross from cache line 2 into cache line 3, therefore 2 misses out of 64 reads
	4. $\frac{62}{64}=\frac{31}{32}$
## Average access times
1. Consider a cache with a hit time of 1 cycle and a miss time of 5 cycles. Compute the following:
    1. If the hit rate is 50%, what is the average access time?
	    1. 0.5\*1 + 0.5\*5 = 3 cycles
    2. If the hit rate increases by 20% (from 50% to 60%) what is the average access time?
	    1. 0.6\*1 + 0.4\*5 = 2.6 cycles
    3. When we "increased the hit rate by 20%", how much speedup did that produce in average access time?
	    1. $speedup = \frac{3}{2.6} = 1.15$
2. We have a job that takes 50 seconds to run. Of that time, we spend 25% of the time processing hits and 75% of the time processing misses. Determine which is the better choice:
    1. Investing in a technology Blue that improves the hit time by a factor of 100, or
	    1. $50 \times (\frac{0.25}{100} + 0.75) = 37.625$
    2. Investing in a technology Red that improves miss time by a factor of 2
	    1. $50 \times (0.25 + \frac{0.75}{2}) = 31.25$ 
		    1. faster
## Cache associativity and organization
1. Why do we organize caches into lines?
	1. We want to leverage the cost of going to the slower media by bringing multiple items in at once (it is often the case that the cost of transferring N items from a storage medium is less than N\*the cost of transferring a single item). Items in the cache need metadata (e.g., tags) to identify them; if a single tag describes multiple data items, then our data:metadata ratio is better.
2. Why not make all caches fully associative?
3. What advantage does a multi-way set associative cache offer?
4. Come up with a workload that:
    1. Will work significantly better with a 4-way set associative cache than it will with either direct mapped or 2-way set associative.
    2. Might work better with a direct mapped cache than it would with a 2-way set associative cache (using random replacement).
5. If you increase the set associativity and leave the cache line size and total cache size the same, what changes in how the bits of an address are used to access the cache?
  
## Extra practice
1. The access time for a hit on the level 1 cache is 4 cycles. The cost to access something that is in the L2 cache is 12 cycles. If the caches all miss and the access has to go to main memory the cost is 48 cycles. Suppose that the hit rate for the L1 cache is .99 and the hit rate on the L2 is .99 as well. What is the average access time for this system? Give your answer in cycles to 4 decimal places.
2. Let's see how a small change in hit rate can induce a significant performance change. Given the cache and memory from the previous question, if the L2 hit rate remains unchanged, but the L1 hit rate drops to .90, what is the average access time for this system? Give your answer in cycles and to 3 decimal places.