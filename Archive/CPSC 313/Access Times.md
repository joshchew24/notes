---
tags:
  - cpsc313
---

# Access Times
## Sequential
`access_time = num_non_contiguous_blocks * (seek_time + rotational_delay) + num_total_blocks * transfer_time`

## Random
`access_time = num_total_blocks * (seek_time + rotational_delay + transfer_time)`