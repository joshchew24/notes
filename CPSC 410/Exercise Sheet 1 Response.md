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
(c) One reason to pri
The purpose of printing all the lexer-provided tokens may be to provide insight on how our program is first interpreting the raw input before applying more drastic processing. It could also just be