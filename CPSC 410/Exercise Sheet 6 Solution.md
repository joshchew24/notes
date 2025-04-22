# Exercise Sheet 6 Solution

Submission Deadline: Friday 4th April by 5pm (Vancouver time!)

**What to submit:** Upload files to Canvas: as usual any reasonably-standard text/document format is fine.

### Assessed Learning Objectives

(44). derive appropriate Failure Conditions for given statements and correctness properties of interest. (**Q1**)

(45). define the key ideas of the symbolic execution techniques, and their meanings in the context of checking correctness properties. (**Q1**, **Q2**)

(46). apply the symbolic execution technique (with basic loop handling) to simple imperative programs. (**Q1**, **Q2**)

(47). contrast the precision of Symbolic Execution with value-agnostic analyses

(48). contrast the handling of loops in basic Symbolic Execution, with loop invariants provided, and with bounded loop unrolling. (**Q2**)

(49). apply Symbolic Execution to find test suites for simple programs. (**Q2**)

(50). explain why this technique works well for path coverage of simple programs, but not for programs with loops (**Q2**)

(51). motivate the problems with using an over-approximate analysis to generate test cases, using examples (**Q2**)

(52). contrast concolic execution with symbolic execution, giving examples that illustrate their differences (**Q2**)

(53). apply the concolic execution technique for a given program and concrete input, to generate the next input(s) (**Q2**)



--- 
### Applying Symbolic Execution

<a name="ex1"></a>
**Question 1**
Consider the following program, on which we will use our Symbolic Execution technique for detecting any potential for assertion violations (runtime errors caused by assert statements):

```
void foo(int x) {
  int z = x + 7;

  while(z < 42) {
    z = z + 1;
    assert z <= 42; // (I)
  }
  if(z == 12) {
    assert false;   // (II)
  } else {
    assert z <= 12;  // (III)
  } // stop before this line for 1(c)!
}
```
(a) *What are the Failure Conditions for each of the lines labelled with (I), (II) and (III)? You can write these in terms of program variables.*

**Answer: (I) `z > 42` (II) `true` (III) `z > 12`**


(b)  *What information do we need, in adding to failure conditions, to check for potential assertion violations and/or to show their absence? How do we use this extra information to  check for potential assertion violations and/or to show their absence? (Briefly) How does symbolic execution get this information?*

**Answer: We additionally need potential path conditions describing under which conditions executions can reach each assertion. To check for potential assertion violations, we need to collect path conditions for all paths that possibly reach each assertion, and check whether the conjunction of each path condition + the failure condition for the assertion is satisfiable. Symbolic execution can provide these path conditions by keeping track of a symbolic state and updating the path conditions whenever a branch is explored. The symbolic state is updated at each assignment statement (and also for loops, depending on the rule used), and the path conditions are updated at each branching statement.**


---

Recall the over-approximate rule for loops we defined in our Symbolic Execution technique.

(c)  *Apply the Symbolic Execution technique to the code provided, showing an abstract state (symbolic state + path conditions) for every non-empty line of code up to and including the last assert statements. Although it is the case that for an if-then-else construct one needs to split off two separate symbolic executions for the remainder of the program, since you are allowed to stop at the last assert statements you can show all of your answers next to a single copy of the code.*

**Answer: See code below.**

```
                        Symbolic State          Path Conditions
void foo(int x) {		(x -> U)			    {}
  int z = x + 7;		(x -> U, z -> U+7)	    {}
  while(z < 42) {		(x -> U, *z -> V)		{V < 42}
	z = z + 1;		    (x -> U, z -> V+1)		{V < 42}
	assert z <= 42;	    (x -> U, z -> V+1)		{V < 42}
  }				        (x -> U, *z -> W)	    {W >= 42}
  if(z == 12) {			(x -> U, z -> W)		{W >= 42, W == 12}
	assert false;  		(x -> U, z -> W)		{W >= 42, W == 12}
  } else {			    (x -> U, z -> W)		{W >= 42, W != 12}
	assert z <= 12; 	(x -> U, z -> W)		{W >= 42, W != 12}
  }             	    // Stop before this line for this question!
}
```

***where V and W are introduced, any choice of symbolic variable name is fine so long as it's (a) different from the original two, and (b) different from the one chosen at the other fresh point.**

(d) *Explain how to use your results to check for potential assertion violations for each of the assert statements: describe each of the checks that needs to be made, and its result. 
Do your results suggest that there are any potential assertion violations or not?*

**Answer: At (I) we symbolically evaluate z > 42 and get V+1 > 42. After adding the path conditions to this failure condition, we need to check {V+1 > 42, V < 42} for satisfiability. These are not satisfiable, so we don't find assertion violations for this assertion.**

**At (II) we symbolically evaluate true and get true. After adding the path conditions to this failure condition, we need to check {true, W >= 42, W == 12} for satisfiability. These are not satisfiable, so we don't find assertion violations for this assertion.**

**At (III) we symbolically evaluate z>12 and get W>12. After adding the path conditions to this failure condition, we need to check {W>12, W >= 42, W != 12} for satisfiability. These are satisfiable (for any W >= 42), so we find potential assertion violations at this line.**

**Overall, one potential assertion violation is found.**


--- 
### Test Case Generation and Loops

<a name="ex2"></a>
**Question 2**

The simple rule for Symbolic Execution of loops (from Slide Deck 12) over-approximates the possible states the analysed program can actually reach.

Consider the following variant of the previous code:

```
void foo(int x) {
  int z = x + 7;

  while(z < 42) {
    z = z + 1;
  }
  if(z == x + 7) {
    assert false;   // (A)
  } else {
    assert false;   // (B)
  }
}
```

Suppose that we are interested in test-case generation for this program: in particular, we would like to use Symbolic Execution to try to find test cases that reach each of the assert statements marked `(A)` and `(B)` above (this is what is meant by "appropriate test cases" in the questions below).

(a) *Suppose we use the simple rule for Symbolic Execution of loops to analyze this piece of goal. Say we are interested in exploring whether the assertion at line (B) can be triggered. When the symbolic execution reaches line (B), what symbolic variables are involved in the value for `x`? What symbolic variables are involved in the value for `z`? Are these sets of variables distinct, or do they overlap? What path conditions do you have to reach (B)? Which symbolic variables are involved in the path conditions reaching (B)?*


**Answer: At reaching (B), `x` will still map to its original symbolic value, say `U`: only that variable is involved. `z` will map to a new symbolic variable, allocated after exiting the while loop: say `S`. These variables are distinct. The path conditions will be `S>=42, S != U + 7`(you may have chosen a different name for the fresh symbolic variable): both `S` and `U` are involved in the path conditions for reaching (B).** 


(b) *Continuing from (a) above. Is it possible to generate an assignment to the symbolic variables that satisfies the path conditions for reaching (B), and results in a test case where `x < 10`? If so, give an example of such an assignment, and the corresponding test case (value of `x` passed to `foo`). Does that test case reach (B)? If no such assignment exists, explain why not.*

**Answer: Yes, say `S=70`, `U=9`. This would give the test case `x=9`, since U represents `x`'s initial value. In the first line executed in the test, we set `z=16`. After the loop is executed by this test case will see values `z=42`, `x=9`, so (B) is reached.**

(c) *Continuing from (a) above. Is it possible to generate an assignment to the symbolic variables that satisfies the path conditions for reaching (B), and results in a test case where `x > 100`?  If so, give an example of such an assignment, and the corresponding test case (value of `x` passed to `foo`). Does that test case reach (B)? If no such assignment exists, explain why not.*

**Answer: Yes, say `S=70`, `U=109`. This would give the test case `x=109`, since U represents `x`'s initial value. In the first line executed in the test, we set `z=116`. After the loop is executed by this test case will see values `z=116`, `x=109`, so (B) is not reached; this assignment of values yields a spurious test case (it's generated based on the constraints for reaching (B), but it doesn't). Essentially, we need to get lucky in the choice of satisfying values, here.**

(d) *Run concolic execution on the piece of code above, starting with `x=34`. At the end of execution, what are the concrete values of `x` and `z`? What about the symbolic values? What path path conditions are collected on this run?*

**Answer: With `x=34`, we will (at the first line) get `z=41`. This means we go through the body of the while loop once, after which `z=42`. Since `42!=34+7`, we end up taking the else branch of the if statements. Our symbolic values will be `x->U, z->U+8` at the end of execution. The path constraints are `{U+7<42, U+8>=42, U+8 != U+7}`**


(e) *How do the path conditions in (d) differ from those we would get from reaching the same assert statement with our original simple Symbolic Execution? Does this affect the accuracy of test generation from these constraints? Why or why not?* 

**Answer: Following the same path in our original symbolic execution is what we did in (a), which gave us the path constraints {`S>=42, S != U + 7`} (in which `U` is the original symbolic value for `x`, and `S` is the one freshly picked after the loop). With concolic execution we don't generate fresh symbolic variables; we use the concrete execution to decide how many times to execute the loop, collecting path constraints *each time* we branch at the loop head; by the end, we have `{U+7<42, U+8>=42, U+8 != U+7}`. The constraints with concolic execution are entirely over the symbolic value that `x` started with, so we get more accurate test generation from them. By contrast, the constraints with our over-approximate symbolic execution rule for loops end up mostly constraining the value to `S`, which we can't directly control for test generation. As we saw in (b) and (c), it was possible to get inaccurate test generation from the symbolic constraints; this won't happen for concolic execution.**



(f) *Continue the concolic execution from (d), generating a new test by flipping the last unexplored (i.e., both sides have not been seen with a concolic run) path condition. Follow the procedure in class, where if flipping the Nth path condition gives unsatisfiable constraints, we move to the (N-1)th path condition. What satisfiable path conditions do you obtain (let's call that set of conditions `C'`)? Generate an input that satisfies the new path conditions (`C'`). If you run concolic execution with this new input, do you get the new path conditions you expected? If they differ from `C'`, why?*



**Answer: If we flip the last condition, we get `{U+7<42, U+8>=42, U+8 == U+7}`. This is unsatisfiable. So we'll flip the second-last condition again, giving `C'`=`{U+7<42, U+8<42}`, which is satisfiable. An input that satisfies `C'` is, for example `U=33`, which gives an initial input `x=33` (we chose a value here to keep the execution short, but others would be possible!).**

**When we run concolic execution with this input, we don't get exactly the path conditions `C'`. Instead, we get `{U+7<42, U+8<42, U+9>=42, U+9 != U+7}`, because flipping the loop exit condition gives us (at least) one extra loop iteration, which adds an extra path condition, and alters the symbolic value of `z` again before the end of execution.**


(g) *How does the concolic execution approach for loops relate to the bounded loop unrolling method in Slide Deck 13? How does it relate to the loop invariant approach in Slide Deck 13?*

**Answer: The approach we use for handling loops in concolic execution is essentially the bounded loop unrolling method described in Slide Deck 13.** 

**On the other hand, concolic exeuction is not too related to the loop invariant approach. We could use concolic execution to check (partially) whether the loop invariants are correct, however. We could also replace the concolic execution gathered constraints with (manually-provided) loop invariants: this might reduce some of the over-constraining and unsatisfiable path exploration we saw in the big concolic execution example.**