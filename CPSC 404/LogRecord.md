# LogRecord
## LogRecord
- most of these details are abstracted for simpler analysis
### Types
- update
- commit
- abort
- end (signifies end of commit/abort)
- compensation log records (CLR)s
	- for UNDO actions
### Fields
- `prevLSN`
	- ommitted in examples
- `transID`
- `type`
	- ommitted in examples
- `pageID`$^{1,2}$
- `length`$^{1,2}$
- `offset`$^{1,2}$
- `before-image`$^{2,3}$
- `after-image`$^{2,3}$

- 1: identifies the DB element modified
- 2: only for `update` log records
- 3: data values before/after the update