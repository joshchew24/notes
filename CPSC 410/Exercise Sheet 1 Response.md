# Exercise Sheet 1 Response
## Question 1

*(a) Can we consider the `Node` data structure a module? Why or why not? Should such a module include only the code in `tinyHTML/src/libs/Node.java`, or any (subset of) the files in `tinyHTML/src/ast`? Why or why not?*

(a) The `Node` data structure should probably not be considered a module since it is just an abstract class that is used to construct ASTs. The files in `tinyHTML/src/ast` are not implementations of the `Node` data structure 

*(b) Let us suppose we consider  `tinyHTML/src/libs/Node.java` and the classes implemented `tinyHTML/src/ast` part of the `AST Module`. Looking at the classes in `tinyHTML/src/ast`, would you say the `AST Module` is relatively shallow, or relatively deep? Why or why not?*
(b) I would say that the `AST Module` is relatively shallow, because it has a large interface while providing basic functionality of different elements of an AST. The user must construct the AST and its components by themself using the various classes and methods provided by the interface.

*(c) Consider the notion of deep vs. shallow modules, as discussed in the first lecture. Is it __possible__ to make the AST Module deeper/shallower? Is it __desirable__ (especially from a "reducing complexity" standpoint) to make the AST Module deeper/shallower? Why or why not?*
(c) it is possible to make the AST Module deeper. This could be achieved by abstracting the components of an AST behind a single "AST Factory" with methods for inserting/modifying an AST. It may be desirable as the user would not need as much background knowledge on ASTs to construct one correctly. A deeper module might contain checks and automatic operations to ensure correctness.  

*(d) Consider again the `AST Module` from part b. How does inheritance (i.e. the `Node` class requiring the `evaluate` method to be implemented) affect the cost of the interface to this module?*

## Question 2

*(a) 
Examine the file `ParseToASTVisitor.java` to get an idea of how the conversion stage is performed. How does the choice to include a conversion stage contribute to the implementation work needed overall? How do you think it affects the complexity of our code base overall?*

(a) The choice to include a conversion stage means an additional module needs to be implemented and tested. Overall, this should reduce code complexity of the system as we would work with our custom AST instead of ANTLR's generated parse tree.

*(b) In class we suggested writing tests only for the first parsing phase as a whole (Lines 18-29 of `Main.java`) and for the evaluator module as a whole (Lines 31-34 of `Main.java`). Can you think of a possible disadvantage of not testing the lexing, parsing, and conversion stages individually? What about an advantage?*
(b) A disadvantage of not testing these stages individually is if an error were to occur in the parsing stage, it might be harder to identify the exact source of the bug. An advantage of testing these phases as a whole is we can ensure that the intended purpose of the entire module is being fulfilled. In other words, we can black-box test the entire module. 

*(c) What do you think is the purpose of the code on Lines 19-21 of `Main.java` (printing all of the lexer-provided tokens)? What would be a better way to achieve this purpose than printing the tokens in the main function?*
(c) One reason to print all the lexer-provided tokens may be to simply reprint the input file to console to inform the user what they are trying to run the program against. A better way to achieve this would be to simply print the contents of the file to console.

**Question 3**

*(a) Try running this test (it should pass; if not, you might need to make sure your project is set up to compile, and that you've run ANTLR on the `TinyHTMLParser.g4` file once, so that it generates the appropriate classes).*

*(b) Right now, the rules in `TinyHTMLParser.g4` contain a trick that causes the generated code to produce objects of a different class for the first row in a table (which makes it easy for us to change its behaviour later and have it printed in bold). Let's imagine we change this design and instead decide to simplify the grammar as follows: comment-out or delete the line `boldrow: row;` and, in the line above it, change the usage of `boldrow` to be `row`. Now rerun ANTLR on this file, and then try to rerun the test. What happens, and why?*
(b) This change causes the ParsetoASTVisitor conversion module and the test code to fail compilation, because these files contain code that expect the existence of boldrows.

**Question 4**

_(a) Suppose that instead of testing **Stage 1** and **Stage 2** as above, that we combine two other kinds of parser tests: Firstly, end-to-end tests that check that input strings result in expected AST `Node` instances after all three Stages of parsing have completed, and secondly, tests that check the functionality of only **Stage 3**: i.e. testing the functionality of the `ParseToASTVisitor.java` file. Is there any value in combining these two kinds of tests?_
(a) I don't think there's value in combining these two kinds of tests, because we would still be testing against the ANTLR parse trees, which are prone to changes if we modify the underlying grammar definitions. 

_(b) Imagine you are building a different software project that uses a library whose interface changes frequently from version to version. How is this situation similar to the issues we saw here with testing the ANTLR-generated **Stages 1 and 2**?_
(b) This is similar because any time we would want to upgrade the version of the library used by our project, we would need to modify our own code that uses its interface.

*(c) Suppose that during your projects you do not cleanly separate modules in your own project teams, and your code depends on the internal details of your teammates. How could this create similar problems to those that we've seen here? Are these problems limited to writing convenient tests?*
(c) This is problematic because my code would be dependent on my teammates code, which is prone to changes. This coupling is not just limited to tests, as changes in one module may cause rippling changes in the entire project.