# Coverage
## Line
- *executable* lines
	- function definitions (for parameter assignment)
	- implicit returns
	- excludes whitespace, comments, lines with only braces, etc.
## Statement
- stronger than line coverage
- requires all statements on a line to be executed to satisfy the coverage condition
	- i.e. if a program consists of one line with two statements on it, and only one statement is executed
		- line coverage is 100%
		- statement coverage is 50%
		- e.g.
			- `if ('a' == 'b') { console.log('hello world'); }`
			- one line, two statements
## Branch
- fraction of branched paths that are executed
- e.g. both the true and false branches of an if statement
## Path
- fraction of *program paths* that are executed
	- i.e. all possible combinations of all conditional paths

For example, given the test and code snippet below:
```
1: eval(x: number, c1: boolean, c2: boolean): number {
2:  if (c1)
3:    x++;
4:  if (c2) 
5:    x--;
6:  return x;
7: }
```
And the following test:
```
1:	eval(0, false, false);
```
The code has 67% line and statement coverage (lines 1, 2, 4, 6 are covered while lines 3 and 5 are not), 50% branch coverage (only the false branches of the if statements on lines 2 and 4 are covered), and only 25% of possible program paths are covered. The following suites would provide 100% coverage for each of the different coverage criteria:
```
// 100% line coverage
eval(0, true, true);

// 100% statement coverage
eval(0, true, true);

// 100% branch coverage
eval(0, true, true);
eval(0, false, false);

// 100% path coverage
eval(0, true, true);
eval(0, false, false);
eval(0, true, false);
eval(0, false, true);
```
