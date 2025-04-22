---
aliases:
  - Value-Agnostic Program Slicing
---
# Static Program Slicing
## Information
### Control Flow Dependency Stack
- tracks lines which **might** have affected values of all conditional branches/lops currently being analysed
## Analysis Rules
### If-then-else branches
1. Push onto the current CFD stack the set $TD(y_0)∪TD(y_1)∪…∪TD(y_n)$ where $y_0,y_1,…,y_n$ are the variables read in the if-condition 
2. Copy the map from **before** the if-then-else to the beginnings of both blocks.
3. Step through the block statements recursively to get maps for the ends of both blocks
	1. The TD map after if-then-else maps each variable to the union of its mappings at the end of each block
4. Pop the head of CFD to get the CFD for after the if-then-else