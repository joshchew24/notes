# Exercise Sheet 6 Questions Symbolic + Concolic Execution 

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

(b)  *What information do we need, in adding to failure conditions, to check for potential assertion violations and/or to show their absence? How do we use this extra information to  check for potential assertion violations and/or to show their absence? (Briefly) How does symbolic execution get this information?*


---

Recall the over-approximate rule for loops we defined in our Symbolic Execution technique.

(c)  *Apply the Symbolic Execution technique to the code provided, showing an abstract state (symbolic state + path conditions) for every non-empty line of code up to and including the last assert statements. Although it is the case that for an if-then-else construct one needs to split off two separate symbolic executions for the remainder of the program, since you are allowed to stop at the last assert statements you can show all of your answers next to a single copy of the code.*

(d) *Explain how to use your results to check for potential assertion violations for each of the assert statements: describe each of the checks that needs to be made, and its result. 
Do your results suggest that there are any potential assertion violations or not?*


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


(b) *Continuing from (a) above. Is it possible to generate an assignment to the symbolic variables that satisfies the path conditions for reaching (B), and results in a test case where `x < 10`? If so, give an example of such an assignment, and the corresponding test case (value of `x` passed to `foo`). Does that test case reach (B)? If no such assignment exists, explain why not.*


(c) *Continuing from (a) above. Is it possible to generate an assignment to the symbolic variables that satisfies the path conditions for reaching (B), and results in a test case where `x > 100`?  If so, give an example of such an assignment, and the corresponding test case (value of `x` passed to `foo`). Does that test case reach (B)? If no such assignment exists, explain why not.*


(d) *Run concolic execution on the piece of code above, starting with `x=34`. At the end of execution, what are the concrete values of `x` and `z`? What about the symbolic values? What path path conditions are collected on this run?*


(e) *How do the path conditions in (d) differ from those we would get from reaching the same assert statement with our original simple Symbolic Execution? Does this affect the accuracy of test generation from these constraints? Why or why not?* 


(f) *Continue the concolic execution from (d), generating a new test by flipping the last unexplored (i.e., both sides have not been seen with a concolic run) path condition. Follow the procedure in class, where if flipping the Nth path condition gives unsatisfiable constraints, we move to the (N-1)th path condition. What satisfiable path conditions do you obtain (let's call that set of conditions `C'`)? Generate an input that satisfies the new path conditions (`C'`). If you run concolic execution with this new input, do you get the new path conditions you expected? If they differ from `C'`, why?*


(g) *How does the concolic execution approach for loops relate to the bounded loop unrolling method in Slide Deck 13? How does it relate to the loop invariant approach in Slide Deck 13?*
