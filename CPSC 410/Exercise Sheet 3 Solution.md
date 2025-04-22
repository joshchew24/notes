# Exercise Sheet 3 Solution
Submission Deadline: Friday 14th February by 5pm (Vancouver time!)

**What to submit:** Written solutions to Questions 1,2,3,5 in any reasonably-standard text/document format, on Canvas.

### Assessed Learning Objectives

*Visitors + Evaluation*

(14). implement evaluation methods for an AST (or similar data structure). (**Q2**, **Q4**)

(15). contrast the two evaluation approaches here (AST methods vs. Visitors). (**Q2**)

(16). judge when AST functionality should be refactored using the Visitor Pattern.

(17). explain how the Visitor pattern simulates double-dispatch, and why double-dispatch is important for the use (and reuse) of the pattern. (**Q1**)

*Variables, Binding and Memory*

(23). implement language support for standard variable operations. (**Q4**)

(24). select appropriate representations for states used during evaluation. (**Q4**)
 

*Static + Dynamic Checking*

(26). contrast the trade-offs between static and dynamic checking, identifying the consequences of each (or neither) for users and language implementers. (**Q3**)

(27). justify whether a correctness property can be checked statically / dynamically. (**Q3**)

(28). implement static and dynamic checkers for particular correctness properties. (**Q5**)

(30). design test-cases to illustrate relevant corner-cases for a correctness property. (**Q5**)


**Question 1**

In this exercise we will examine how the Visitor pattern simulates double-dispatch. We will examine the TinyHTML code examined in class in more detail.

In the TinyHTML AST, a `Table` contains a list of `Row`s. This list can contain both regular `Row`s, as well as `BoldRows`s, a subclass whose contents will be printed in bold font. In the current construction, the first row of a table is a `BoldRow`.

The following questions all deal with the [TinyHTMLColoured](https://github.students.cs.ubc.ca/CPSC410-2024W-T2/tinyHTMLColoured) repository. We will ask you the sequence of methods that are called before a particular program point is reached when evaluating the tinyHTML program:

**EXAMPLE INPUT:**

```
Title: My Day
Table:
[Date | Time | Activity ]
[Sept 1 | 9am  | Get to school ]
[Sept 1 | 11am | Have lunch ]
```

Here is an example of the expected answer format:
>Q. "Say we are evaluating the **EXAMPLE INPUT** above. Starting from the `Program` `evaluate` [method in commit 9830cf2](https://github.students.cs.ubc.ca/CPSC410-2024W-T2/tinyHTMLColoured/blob/9830cf20b698c30f2b78eb0c523bc81925664048/src/ast/Program.java#L46), which methods are dynamically called before the title is printed? List only methods from the `tinyHTMLColoured` project, ignoring getters.".
>
>A. The methods called before the title are printed are: `Program.evaluate(PrintWriter writer)` and `Title.evaluate(PrintWriter writer)`.



*(a) First, consider the [original evaluator code](https://github.students.cs.ubc.ca/CPSC410-2024W-T2/tinyHTMLColoured/tree/9830cf20b698c30f2b78eb0c523bc81925664048/src/ast) (commit 9830cf2), where each AST Node owns its own `evaluate` method. Say we are evaluating the __EXAMPLE INPUT__ above. Starting from the `Table` `evaluate` method (i.e. just after the calls mentioned in the example answer format above), which methods are dynamically called before the first table row is printed? List only methods from the `tinyHTMLColoured` project, ignoring getters. Will the first table row be printed in bold? Briefly explain why/why not.*

**Sample Answer:
The list of methods called dynamically is `Table.evaluate(PrintWriter writer)`, and `BoldRow.evaluate(PrintWriter writer)`.
The first row will be printed in bold; the `evaluate` method is dynamically dispatched and so the `BoldRow` implementation will be the one chosen.**

*(b) Now, consider the following [refactored Evaluator code](https://github.students.cs.ubc.ca/CPSC410-2024W-T2/tinyHTMLColoured/blob/634b4eead1081c60349a58e94c14b5ed0644f237/src/evaluator/Evaluator.java), where the evaluator code has been pulled out into its own class, without the visitor pattern (commit 634b4ee). Say we are evaluating the __EXAMPLE INPUT__ above. Starting from the `Evaluator ` `evaluate(Table t, PrintWriter writer)` method, which methods are dynamically called before the first table row is printed? List only methods from the `tinyHTMLColoured` project, ignoring getters. Will the first table row be printed in bold? Briefly explain why/why not.*

**Sample Answer:
The list of methods called dynamically is `Evaluator.evaluate(Table t, PrintWriter writer)`, and `Evaluator.evaluate(Row r, PrintWriter writer)`. The first row will __not__ be printed in bold; the choice betwene multiple *overloaded* definitions of the same method is made statically (based on the static types of the arguments).**

*(c) Now, consider the [Evaluator visitor](https://github.students.cs.ubc.ca/CPSC410-2024W-T2/tinyHTMLColoured/blob/5c22f93d72900630c29ae163f846c278203c4aee/src/evaluator/Evaluator.java), where the evaluator code follows the visitor pattern (commit 
5c22f93). Say we are evaluating the __EXAMPLE INPUT__ above. Starting from the `Evaluator ` `visit(Table t, PrintWriter writer)` method, which methods are dynamically called before the first table row is printed? List only methods from the `tinyHTMLColoured` project, ignoring getters. Will the first table row be printed in bold? Briefly explain why/why not.*

**Sample Answer: The list of methods called dynamically is `Evaluator.visit(Table t, PrintWriter writer)`, `BoldRow.accept(TinyHTMLVisitor<T, U> v, T t)`, `Evaluator.visit(BoldRow br, PrintWriter writer)`. The first row will be printed in bold; the `accept` method is dynamically dispatched with the row object as receiver: this means we run the definition of `accept` in `BoldRow`, which in turn selects the corresponding `visit` method for this specific argument type.**

*(d) Finally, consider this alternate implementation (based on commit 
[5495ac5](https://github.students.cs.ubc.ca/CPSC410-2024W-T2/tinyHTMLColoured/blob/5495ac5f4694a8934009814718cc18af274efd9c/src/evaluator/Evaluator.java#L40)) of the `visit(Table t, PrintWriter writer)` method:*

```
public Void visit(Table t, PrintWriter writer) {
    writer.print("<table");
    if (t.getColour() != null){
       visit(t.getColour(), writer); // this is different
    }
    writer.println(" bgcolor=black width=600>");
    for (Row r : t.getRows()){
       visit(r, writer); // this is different
    }
    writer.println("</table>");
    return null;
}
```
*Say we are evaluating the __EXAMPLE INPUT__ above. Starting from the `Evaluator ` `visit(Table t, PrintWriter writer)` method, which methods are dynamically called before the first table row is printed? List only methods from the `tinyHTMLColoured` project, ignoring getters. Will the first table row be printed in bold? Briefly explain why/why not.*

**Sample Answer:
The list of methods called dynamically is `Evaluator.visit(Table t, PrintWriter writer)`, and `Evaluator.visit(Row r, PrintWriter writer)`. The first row will __not__ be printed in bold; because we "skip" the `accept` call usually used, there is no dynamically-dispatched call with the row object as receiver; we do not select the more-specific impl.**

**Question 2**

One advantage of the Visitor pattern is that a base visitor class can be implemented that has "default" behavior (e.g. doing nothing but passing on the visiting to the children of the current node). With this base visitor class, one can implement new visitors without having to implement `visit` methods for every AST class. 

Say we want to add a feature to tinyHTMLColoured to choose a nice background colour for our webpage, based on the colours of the titles/tables. For this, we should first collect all the colours that appear in the given program. We have added started code for a `ColourCollector` visitor that should gather all the colours used in a program in the set of strings passed to it, and added the following code to `main`:

```
ColourCollector cc = new ColourCollector();
Set<String> colours = new HashSet<>();
parsedProgram.accept(cc, colours);
if (!colours.isEmpty()){
    System.out.println("Colours in this program:" + colours.toString());
}
```

*(a) We have added starter code for the [ColourCollector visitor](https://github.students.cs.ubc.ca/CPSC410-2024W-T2/tinyHTMLColoured/blob/main/src/analyses/ColourCollector.java).
What is the minimum number of visit methods you need to override to correctly collect all colours used in the given tinyHTML Program? What should those methods do?* 


**Sample Answer: You need only implement `visit(Colour c, Set<String> colours`. 
That method should add the colour name field to its set parameter. Here's an example implementation:**

```
@Override
public Void visit(Colour c, Set<String> colours) {
    colours.add(c.getColour());
    return null;
}
```

*(b) Suppose that we forgot to implement a method that needs a specific definition for `ColourCollector` to work correctly. What is a potential downside of our choice to extend `TinyHTMLBaseVisitor` (compared with choosing only to implement the `TinyHTMLVisitor` interface)?*

**Sample Answer: Given that the base visitor provides default implementations for all visit methods, if we forget to implement a case then (unlike if we were implementing the `TinyHTMLVisitor` interface directly) we won't get a compilation error; the default case defined in `TinyHTMLBaseVisitor` will be run instead.**

_(c) The `ColourCollector visitor` is intended to accumulate colours by mutating a pre-allocated `Set<String>` object, passed in as a parameter. Suppose instead that we went for a different design, and decided to pass no parameter, but instead have the visitor **return** a `Set<String>` representing the results of visiting this node (i.e. we would change the `extends` declaration to `extends TinyHTMLBaseVisitor<Void, Set<String>>`). Looking at the `TinyHTMLBaseVisitor` definition, would we still be need to implement the same number of methods for as in your answer to part (a)? Briefly explain your answer._

**Sample Answer: If we pass around the results in a `Set<String>` as a return value, we need to coordinate the return values through all the program visitors. The current default base visitor simply returns `null` everywhere, so this won't work. We'd need to modify at least the `Program`, `Table`, and `Title` visitors to return the set of strings gotten from visiting the children. And, to avoid null-pointer exceptions, we'd need to modify the other visitors to return an empty set of strings rather than a null value.**

*(d) What change could you make to the design of `TinyHTMLBaseVisitor` to make it more convenient for defining specific visitors that return values (e.g. the alternative visitor design described in part (c))?*

**Sample Answer: This is a bit of a tricky question. We could make `TinyHTMLBaseVisitor` an *abstract class* and declare two abstract methods: `U defaultValue()` and `U combine(U first, U second)`. These play the role of defining a suitable return value of the appropriate type `U` (e.g. an empty `Set<String>` in our specific example) and a way of combining two such values together while iterating through children (e.g. computing the union of two `Set<String>` values). Now we can rewrite the default implementations in these terms: using `defaultValue()` instead of `null` for individual values, and aggregating the results for children together using `combine(x,y)` calls. Try out this refactoring yourself; it would be a great bonus exercise!**

**Question 3**

In class (and in the repo) we have seen the extension of `tinyVars` to include support for if-conditionals and command-line arguments. Combining these, it is possible to write code such as:

```
if($0) {
    set x, 42;
} else {
    set x, 4;
}
print x;
```

*(a) Suppose we are interested in the question of whether, for a particular if-conditional, its "then" (first) branch or its "else" (second) branch will be evaluated. In general, is this something we can determine statically? What about dynamically? Briefly explain your answer.*

**Sample Answer: Which branch will actually be executed is in general only known dynamically (when we evaluate the expression used as if-conditional in a particular runtime state: these values are dynamically-bound). In particular, if the value of this expression depends on external factors such as command-line arguments in this language, we can't in general know the branch that will be taken; it will be different for different executions of the same code.**


*(b) Consider now the version of `tinyVars` with the if-conditional feature but __without__ command-line arguments as a feature (i.e. the current version with the latter feature removed). In this version of the language, is it possible to determine which branch of an if-conditional will be evaluated statically? Briefly explain your answer.*

**Technically this is statically always possible in this version of the language: without the ability to depend on external state, one can "step through" the AST just as easily statically as dynamically: each assignment statement will be executed exactly once and for exactly one value assigned to the variable. So we could track a perfect mapping between variables and their values with a simple one-pass traversal of the AST.**


**Question 4 (Unassessed)**

Add the following two features to `tinyVars`: a while loop, and a subtraction expression. For example, it should be possible to write the following input program, with your extension:

``` 
new x;
set x, $0;
while (x) {
  print 42;
  set x, x-1;
}
```
When evaluated, if we pass a (non-negative) integer command-line parameter, this program should print `42` that number of times. It would be good to write test cases to test your extensions!


**Sample Answer: Not assessed, but the `main ` branch of `tinyVars` contains a sample implementation.**

**Question 5**

We have seen three basic ideas for how to have a static checker deal with if-conditional control flow (see Slide Deck 5), which we considered with respect the correctness property: "variables are declared before being assigned".

The first of the three ideas can be seen to be "pessimistic": e.g. we only consider variables to be declared after an if-conditional if they are definitely declared at the end of *both* branches. This has the advantage that if the checker accepts the program, it will never violate the correctness property during evaluation.

*(a) What would an analogous "pessimistic" approach be for having a static checker for this correctness property check programs with while loops? In particular, is there a way to make this have the same advantage that __if__ the checker accepts the program, there is no way it could ever violate the correctness property?*

**Sample Answer: This is a very in-depth answer for explanation purposes; we don't expect you to have gone into such a level of detail as we do here.**

**We need to describe what the checker should do to handle while loops.**

**So, suppose the checker reaches a loop with a set of previously-declared variables it has recorded up to this point, say `S`. Since it might be that the loop body gets executed next, we should check the loop body statements with the checker, using `S` as the initial set. For example, this should raise an error (about `y`) for a program such as:**
``` 
new x;
set x, $0;
while (x) {
  print 42;
  set y, x;
  set x, x-1;
  new y;
}
```

**our checker should raise an error about `y`.**

**In general, by the end of the loop body, the set of variables considered declared by the checker (say, `S'` for the post-loop-body set) might have *more* (if the loop body contains `new` statements) or *fewer* entries (if the loop body contains `undef` statements).**

**But checking the loop body once is not sufficient. Suppose a variable was in `S` but is *not* in `S'`, such as `x` in the following program:**
``` 
new x;
set x, $0;
while (x) {
  print 42;
  set x, x-1;
  undef x;
}
```
**Note that in this case, the first assignment to an undeclared variable can only happen *after* the first loop iteration.**

**For our checker to work in such cases, *if* `S` contains any variables that `S'` does not, we need our checker to run through the loop body *once more*. For this second pass through the loop body, the checker must consider the initial set of declared variables `(S ∩ S')` (i.e. the variables in both sets, only). With this strategy, the checker would catch the error in the program above in the second pass.**

**Similarly, we let the checker analyse any statements *after* the loop starting from `(S ∩ S')` as the set of variables considered declared (reflecting that the loop body may or may not be executed at all, and the code afterwards needs to be checked to be correct in either case).**

**It's quite subtle to see why *two times* is really always enough for this particular checker, but it comes from the fact that the checker is pessimistic in its design overall, so:**

-  **Any variable that was in `S` and is *still* in `S'` (i.e. in  `(S ∩ S')`) must stay declared after *any* number of iterations of the loop (since if it could be undeclared, even conditionally, in any loop iteration, it wouldn't still be in `S'`).**
-  **Conversely, for any variable `x` which is either not in `S` or not in `S'` (i.e. *not* in `(S ∩ S')`) one of the following must hold: (1) our checker will complain about `x` on one of these two runs or (2) it *must not matter* whether `x` is declared at the start of a loop iteration (again, because the checker is pessimistic about how it treats the loop body, ensuring there is no path through it that needs `x` to be declared already).**

*(b) The __third__ approach outlined in the slide is to check every control flow path through the program separately. Could this approach be generalised for programs with loops? Briefly explain your answer.*

**Sample Answer: With loops (or e.g. features such as methods/procedures that could be recursive) there are unboundedly many paths through the program code: we can't in general predict statically how many times a loop will iterate (and the number of times may depend on inputs), and there are paths through the code that execute the loop body 0, 1, 2, 3, etc. times; our checker would need to check infinitely many cases to be *sure* of finding all the issues.**

**As a side-remark, a whole sub-area of program analysis is about *bounded checking* (e.g. bounded model checkers, etc.) which are techniques and tools which nonetheless explore loops but only up to some maximal number of iterations (or iteratively explore for more and more iterations until e.g. the analysis is killed); these tools can find bugs that manifest in executions with only this many iterations, which can still be very useful in practice!**