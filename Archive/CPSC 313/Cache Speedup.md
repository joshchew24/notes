---
tags:
  - cpsc313
---

# Cache Speedup
Assume you have a speedup value from before and after you added a cache to a system. 
$T_{old} = T_{mem}$ = time to read memory
$T_{hit}$ = time to read from cache on hit
$T_{miss}$ = time to read from cache (and memory) on miss
If $T_{miss} = T_{old} + T_{hit}$ then given a speedup value, $cache\ hit\ rate = \frac{T_{miss}}{T_{old}} - \frac{1}{speedup}$
