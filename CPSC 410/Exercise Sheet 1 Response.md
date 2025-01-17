# Exercise Sheet 1 Response
## Question 1

(a) The `Node` data structure should probably not be considered a module since it is just an abstract class that is used to construct ASTs. The files in `tinyHTML/src/ast` are not implementations of the `Node` data structure 

(b) I would say that the `AST Module` is relatively shallow, because it has a large interface while providing basic functionality of different elements of an AST. The user must construct the AST and its components by themself using the various classes and methods provided by the interface.

(c) it is possible to make the AST Module deeper. This could be achieved by abstracting the components of an AST behind a single "AST Factory" with methods for inserting/modifying an AST. It may be desirable as the user would not need as much background knowledge on ASTs to construct one correctly. A deeper module might contain checks and automatic operations to ensure correctness.  


(d) Inheritance from the `Node` class means that any changes to this parent class would require modifying the subclasses in the module, which can be costly, especially if `Node` is updated frequently.
## Question 2

(a) The choice to include a conversion stage means an additional module needs to be implemented and tested. Overall, this should reduce code complexity of the system as we would work with our custom AST instead of ANTLR's generated parse tree.

(b) A disadvantage of not testing these stages individually is if an error were to occur in the parsing stage, it might be harder to identify the exact source of the bug. An advantage of testing these phases as a whole is we can ensure that the intended purpose of the entire module is being fulfilled. In other words, we can black-box test the entire module. 

(c) One reason to print all the lexer-provided tokens may be to simply reprint the input file to console to inform the user what they are trying to run the program against. A better way to achieve this would be to simply print the contents of the file to console.

## Question 3

(b) This change causes the ParsetoASTVisitor conversion module and the test code to fail compilation, because these files contain code that expect the existence of boldrows.

## Question 4

(a) I don't think there's value in combining these two kinds of tests, because we would still be testing against the ANTLR parse trees, which are prone to changes if we modify the underlying grammar definitions. 

(b) This is similar because any time we would want to upgrade the version of the library used by our project, we would need to modify our own code that uses its interface.

(c) This is problematic because my code would be dependent on my teammates code, which is prone to changes. This coupling is not just limited to tests, as changes in one module may cause rippling changes in the entire project.