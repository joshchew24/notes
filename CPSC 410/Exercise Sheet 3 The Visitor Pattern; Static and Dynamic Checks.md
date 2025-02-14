# Exercise Sheet 3: The Visitor Pattern; Static and Dynamic Checks
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

In the TinyHTML AST, a `Table` contains a list of `Row`s. This list can contain both regular `Row`s, as well as `BoldRow`s, a subclass whose contents will be printed in bold font. In the current construction, the first row of a table is a `BoldRow`.

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

*(b) Now, consider the following [refactored Evaluator code](https://github.students.cs.ubc.ca/CPSC410-2024W-T2/tinyHTMLColoured/blob/634b4eead1081c60349a58e94c14b5ed0644f237/src/evaluator/Evaluator.java), where the evaluator code has been pulled out into its own class, without the visitor pattern (commit 634b4ee). Say we are evaluating the __EXAMPLE INPUT__ above. Starting from the `Evaluator ` `evaluate(Table t, PrintWriter writer)` method, which methods are dynamically called before the first table row is printed? List only methods from the `tinyHTMLColoured` project, ignoring getters. Will the first table row be printed in bold? Briefly explain why/why not.*

*(c) Now, consider the [Evaluator visitor](https://github.students.cs.ubc.ca/CPSC410-2024W-T2/tinyHTMLColoured/blob/5c22f93d72900630c29ae163f846c278203c4aee/src/evaluator/Evaluator.java), where the evaluator code follows the visitor pattern (commit 
5c22f93). Say we are evaluating the __EXAMPLE INPUT__ above. Starting from the `Evaluator ` `visit(Table t, PrintWriter writer)` method, which methods are dynamically called before the first table row is printed? List only methods from the `tinyHTMLColoured` project, ignoring getters. Will the first table row be printed in bold? Briefly explain why/why not.*

*(d) Finally, consider this alternate implementation (based on commit 
[5c22f93](https://github.students.cs.ubc.ca/CPSC410-2024W-T2/tinyHTMLColoured/blob/5c22f93d72900630c29ae163f846c278203c4aee/src/evaluator/Evaluator.java#L46) of the `visit(Table t, PrintWriter writer)` method:*

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

*(b) Suppose that we forgot to implement a method that needs a specific definition for `ColourCollector` to work correctly. What is a potential downside of our choice to extend `TinyHTMLBaseVisitor` (compared with choosing only to implement the `TinyHTMLVisitor` interface)?*

_(c) The `ColourCollector visitor` is intended to accumulate colours by mutating a pre-allocated `Set<String>` object, passed in as a parameter. Suppose instead that we went for a different design, and decided to pass no parameter, but instead have the visitor **return** a `Set<String>` representing the results of visiting this node (i.e. we would change the `extends` declaration to `extends TinyHTMLBaseVisitor<Void, Set<String>>`). Looking at the `TinyHTMLBaseVisitor` definition, would we still be need to implement the same number of methods for as in your answer to part (a)? Briefly explain your answer._

*(d) What change could you make to the design of `TinyHTMLBaseVisitor` to make it more convenient for defining specific visitors that return values (e.g. the alternative visitor design described in part (c))?*

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

*(b) Consider now the version of `tinyVars` with the if-conditional feature but __without__ command-line arguments as a feature (i.e. the current version with the latter feature removed). In this version of the language, is it possible to determine which branch of an if-conditional will be evaluated statically? Briefly explain your answer.*

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

**Question 5**

We have seen three basic ideas for how to have a static checker deal with if-conditional control flow (see slides labelled "(★)" in Slide Deck 5), which we considered with respect the correctness property: "variables are declared before being assigned".

The first of the three ideas (i.e., "1. Variables declared in both branches") can be seen to be "pessimistic": e.g. we only consider variables to be declared after an if-conditional if they are definitely declared at the end of *both* branches. This has the advantage that if the checker accepts the program, it will never violate the correctness property during evaluation.

*(a) What would an analogous "pessimistic" approach be for having a static checker for this correctness property check programs with while loops? In particular, is there a way to make this have the same advantage that __if__ the checker accepts the program, there is no way it could ever violate the correctness property?*

*(b) The __third__ approach outlined in the slide is to check every control flow path through the program separately. Could this approach be generalised for programs with loops? Briefly explain your answer.*