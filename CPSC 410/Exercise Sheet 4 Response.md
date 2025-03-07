# Exercise Sheet 4 Response
## Question 1
- (a) threat to construct validity
	- the study is based on the user's self-survey and their final grade. The user's opinion is subjective, and their grade is dependent on a number of factors aside from code quality, so these measures may not be representative of the original research question.
- (b) threat to internal validity
	- since the user groups were self-selected, there could be a bias in which groups opted to use JavaTest+ (e.g. stronger students may decide against using it), which could have influenced the outcome
- (c) threat to external validity
	- the study was only perfomed in a single section of a single semester of a single university course. With such a small sample size, the results are unlikely to generalize to the entire population of users that write Java code.
- (d) risks
	- students in set B may blame their lower performance on their decision to opt-in to using the tool, and this may also harm the credibility of the tool
## Question 2
- (a) we performed an observational study, where we gave users a prompt and observed their experience writing programs with our DSL
- (b) one key observation from our first user study was that the syntax for our "types" and "counter" system was confusing. This helped us realize this syntax could be more "visual", instead of something akin to typical programming language syntax.
- (c)
	- threat to external validity: our first user study was with two students who were not computer science majors. Our target user group ended up being game developers, so these users may not be an accurate evaluation of the DSL.
	- threat to internal validity: our second user study was with two computer science students. This study was conducted using our more refined DSL. Since the user's background of this study was closer to our target user group than our first study, we cannot say if any improvements were due to our refinements, or the stronger programming background of the group compared ot the first.
- (d)
	- had we conducted the second study with the same participants as the first, this would introduce a threat to internal validity, because these users already have some prior experience working with the DSL, possibly resulting in stronger performance than a first-time user.
## Question 3
- (a) "What is the set of constant strings passed to `System.out.println`"
	- this is a program analysis question. This can be analyzed statically, because it is only asking about the **constant** strings, and not strings contained in variables, which could possibly be only determinable at runtime.
- (b)
	- Assuming there exists a library to parse the program into an AST, we can traverse the tree and find all `System.out.println()` nodes. Then, we parse their children, constructing a set of the nodes that are constant string values. 
- (c) "Which function parameters __can__ end up being involved in `System.out.println` statements?"
	- this is also a program analysis question. This can be analyzed statically, because we are tracking how data flows through the program, and not inputs that are determined at runtime. We can statically determine what variables end up being printed, even if they are in a nested function call. 
- (d)
	- Again, parse the program into an AST. For each function, we can maintain a list of which function parameters have been printed. If a function contains a nested call, we can use a stack to continue the analysis. We should probably have an early termination condition to prevent an infinite analysis if a function is self-recursive. 
- (e) You could argue that `password` is involved with the print statement because it determines whether the print statement is actually called in the `foo` function, whereas `lst` has no effect and is not affected by the print statements whatsoever.
- (f) To support the broader definition of "involved in", you could also add any function parameters that were found as part of the same scope as the print statement. For example, any `if` statements or loops that condition on a function parameter and contain a print statement would consider the parameter to be "involved". 