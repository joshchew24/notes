# Tutorial 6: Caching  

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
	1. it's expensive to check every cache location on every read
	2. In a fully associative cache, you need to consider every cache location on every lookup. Either you do this sequentially which is very slow or you do it in parallel, which makes the hardware cost prohibitive.
3. What advantage does a multi-way set associative cache offer?
	1. in a direct-mapped cache, if you have a cache collision, the old data is evicted. if it is needed again in the near future, we have another cache miss, which is slow. multi-way set associative caches allow multiple pieces of data that belong to the same cache set exist simultaneously in the cache.
	2. If you are manipulating multiple data structures that will map to the same cache line (e.g., two arrays), the conflicts between the two data structures and produce a cache hit rate of 0. Having the flexibility of mapping multiple items to the same cache line can significantly improve your hit rate and therefore your performance.
4. Come up with a workload that:
    1. Will work significantly better with a 4-way set associative cache than it will with either direct mapped or 2-way set associative.
	    1. cache is 4-way set with 4 byte lines
	    2. array is exactly same size as cache consisting of 4 byte elements
	    3. program iterates over the array in 16 byte or 4 element stride, an arbitrarily large amount of times
    2. Might work better with a direct mapped cache than it would with a 2-way set associative cache (using random replacement).
	    1. ASSUMING the cache size is the same in both structures and we have an array that is equal size
		    1. direct mapped will evict all the old cache data, subsequently only containing the array
		    2. the cache may contain old data prior to the array access; thus random replacement may evict our array instead of the old data
5. If you increase the set associativity and leave the cache line size and total cache size the same, what changes in how the bits of an address are used to access the cache?
	1. number of index bits decrease (num tag bits increase)
	2. tag bits will be used to identify hits/misses within a cache set
  
## Extra practice
1. The access time for a hit on the level 1 cache is 4 cycles. The cost to access something that is in the L2 cache is 12 cycles. If the caches all miss and the access has to go to main memory the cost is 48 cycles. Suppose that the hit rate for the L1 cache is .99 and the hit rate on the L2 is .99 as well. What is the average access time for this system? Give your answer in cycles to 4 decimal places.
	1. L1 hit - 4 (0.99)
	2. L2 hit - 12 (0.99)
	3. full miss (48)
	4. $0.99\times4 + 0.01\times0.99\times(12+4) + 0.01\times0.01\times(48+12+4) = 4.1248$
	6. ** they did it differently in the soln -> 12 and 48 cycles are the entire cost of these operations **
		1. $0.99\times4 + 0.01\times0.99\times(12) + 0.01\times0.01\times(48) = 4.0836$
2. Let's see how a small change in hit rate can induce a significant performance change. Given the cache and memory from the previous question, if the L2 hit rate remains unchanged, but the L1 hit rate drops to .90, what is the average access time for this system? Give your answer in cycles and to 3 decimal places.
	4. $0.90\times4 + 0.10\times0.99\times(12) + 0.10\times0.01\times(48) = 4.836$