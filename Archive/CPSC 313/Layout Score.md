---
tags:
  - cpsc313
---

# Layout Score
Undefined for 1-block files
`layout_score = num_contiguous_blocks/(num_total_blocks - 1)`

Not perfect representation of quality of block allocation
- worse
	- two blocks may appear contiguous but live on adjacent cylinders (i, i+1)
	- blocks that are further apart may require longer seek time
- better
	- two blocks may not be contiguous but live on same cylinder/track, requiring no seek
	- blocks that are closer together may require shorter seek time

`seq_access_time = num_total_blocks * (transfer_time + (1 - layout_score)*(seek_time + rotational_delay))`
