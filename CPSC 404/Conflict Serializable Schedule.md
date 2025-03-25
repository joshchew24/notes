# Conflict Serializable Schedule
- **Conflict Equivalence**
	- schedules are **conflict equivalent** if every pair of conflicting actions is **ordered the same way**
		- i.e. S1 can be transformed into S2 by swapping **non-conflicting actions**
- **conflict serializable**
	- if S is conflict equivalent to some serial schedule
	- conflict-serializable schedules can be converted into serial schedules by swapping adjacent non-conflicting actions
- **swapping** 2 [[Conflicts|Conflicting]] actions (from different transactions) changes the results of the schedule
- **swapping** 2 non-conflicting actions does not affect the result of the schedule