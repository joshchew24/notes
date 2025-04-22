# Exercise Sheet 1 Solution

**Submission Deadline:** Friday 17th January by 5pm Vancouver time. See Canvas.

**What to submit:** Written solutions to Questions 1-4 in any reasonably-standard text/document format is fine.

### Assessed Learning Objectives

1. describe the sources of complexity in a given interface. **(Q1, Q2)**
2. assess how proposed changes to an interface would impact a given modular software design **(Q1, Q2)**
3. contrast deep vs. shallow software modules. **(Q1)**
4. identify modules within a larger software system **(Q1, Q2)**
5. compare and contrast what can be tested at the unit, module, and integration level **(Q2, Q4)**
6. assess the challenges of testing at a module boundary **(Q2, Q3, Q4)**


## Installing ANTLR

This and future exercise sheets will require that you generate tokenizers and parsers for small domain-specific languages using the ANTLR Parser Generator. You use ANTLR for your course projects as well.

To get started, you need to download and install ANTLR for Java. Please make sure that you use at least ANTLR 4.10. You can follow the Quick Start instructions on their website to use ANTLR from the command line, but we recommend that you also install one of the plug-ins for IntelliJ IDEA or Eclipse. These will not only make your workflow easier, but also add features like syntax highlighting and basic error checking for grammars to your IDE. 

[https://www.antlr.org]()

Note: You might have to manually add ANTLR to each of your projects as a library, even if you have the plug-in. In IDEA, this is done by opening the "Project Structure" window, selecting "Libraries" and adding the "antlr-4.1X.X-complete.jar" file via the "+" button.


## The Basics of Using ANTLR

ANTLR adds an additional pre-processing step to working on your projects. It uses a grammar file (with the suffix ".g4") as input and generates a number of ".java" files. While the generated code can be a bit cryptic, it provides APIs that you can use in your own code, similar to how you would use any other library. 

To use ANTLR manually via the command line, please have a look at the examples on the official website. If you have the IntelliJ IDEA plug-in installed, you can right-click on a grammar file and select "Generate ANTLR Recognizer". The generated files should then automatically appear in the "gen" directory as part of your project. 

**Note that you might need to add this as a sources directory in the Project Structure settings of your IntelliJ project.** If IntelliJ IDEA does not automatically detect the generated files (i.e. the starter code does not compile after running ANTLR), you can right-click on the "gen" directory, select "Mark directory as..." and then "Generated Sources Root".

To see how you can use the files that ANTLR generates in your code, we recommend that you have a look at the TinyHTML example from class (Lecture 3), or the official documentation on the ANTLR website.

<a name="ex0"></a>
**Question 0**

*To make sure you're prepared for future classes and exercises (including the rest of this sheet!), set up the `tinyHTML` project in your favoured IDE and install ANTLR; make sure that at the very least you get to a point where you can build and run `tinyHTML` on the example input from class (or a similar small example) yourself. You can then play around with the language and have a look at the code to understand how the project is organised, and to use ANTLR.*

## Q1 - Modules for tinyHTML

This exercise sheet will ask you to examine the code in the tinyHTML repository. Download the repository and familiarize with the code in it.

Recall that in class we defined a module as:
> a subset of a software system that has a specific purpose, (relatively) independent of the other modules in the system, encompassing an *interface* and and *implementation*.

Refer first to the main function provided by `tinyHTML/src/ui/Main.java`. Like the example in class, there is a `Node` data structure, representing an Abstract Syntax Tree used to connect the parsing code (Lines 18-29) to the evaluator code (Lines 31-34).
This `Node` data structure is defined in `tinyHTML/src/libs/Node.java`, and subclasses of it are defined in `tinyHTML/src/ast`. 

**Question 1**

*(a) Can we consider the `Node` data structure a module? Why or why not? Should such a module include only the code in `tinyHTML/src/libs/Node.java`, or any (subset of) the files in `tinyHTML/src/ast`? Why or why not?*


**Sample Solution: There is no single right answer to this question. The `Node.java` file and the `ast` package could be considered a module in the following sense: it is the part of the software system responsible for modelling the intermediate data structure (specific purpose).**

**On the other hand, one could argue is not an ideally organized module, as the interface in `Node.java` already combines two purposes: the representation of the AST data structure (the `Node` class and its subclasses, their getters/setters) *and* the evaluation of these nodes (via the `evaluate` method). With just the one extra concern, this isn't too poorly organized, but this idea of adding features to the `Node` hierarachy should not be continued: one could imagine that if `Node` had an `evaluate` method, `check` method, `print` method, etc., this would be much messier as a module (and more-clearly violate the Single Responsibility Principle).**

**Although this approach of adding all functionality to the AST classes is poor design, one could argue that the design is still modular in some sense: the `evaluate`, `check`, and `print` methods could be seen as the interfaces of separate `evaluate`, `check`, and `print` modules, and the implementations for these are then striped across the implementations of `Node` and all its subclasses. While this is a reasonable argument, files are unavoidably a natural point of abstraction when editing code, and the fact that changing two ``modules'' in this sense, would involve modifying the same source files is a clear disadvantage.**


*(b) Let us suppose we consider  `tinyHTML/src/libs/Node.java` and the classes implemented `tinyHTML/src/ast` part of the `AST Module`. Looking at the classes in `tinyHTML/src/ast`, would you say the `AST Module` is relatively shallow, or relatively deep? Why or why not?*

**Sample Solution: Let's consider the interface to be the union of all public methods in the `AST Module`. The `AST Module` is relatively shallow: many of the methods implemented in classes in the AST package are getters/setters, and are not very complex in their implementation (except for `evaluate`). The function `.getRows()` also does not hide much complexity compared to simply accessing the `rows` field (there are of course other design reasons we may want to avoid exposing fields directly: preventing outside actors from arbitrarily changing the fields of a class is a very sane design decision).**

*(c) Consider the notion of deep vs. shallow modules, as discussed in the first lecture. Is it __possible__ to make the AST Module deeper/shallower? Is it __desirable__ (especially from a "reducing complexity" standpoint) to make the AST Module deeper/shallower? Why or why not?*

**Sample Solution: We could make the AST module shallower by factoring out the `evaluate` functionality (e.g. using a Visitor; we'll have a reminder of this pattern in the coming classes). While this remains the only real functionality in the AST classes it's somewhat a matter of taste as to whether this is desirable: on the one hand, separating the `evaluate` concerns from the AST representation itself more-clearly adheres to the Single Responsibility Principle, but it makes the module very shallow; the classes are essentially *only* data with almost no functionality. For a language that had more-complex `evaluate` functionality this might be worth doing.**

**Recall that depth of a module concerns the ratio between the *internal* complexity/functionality that its interface abstracts over, and the size/complexity of the *interface itself*. We could therefore try to make the AST module deeper by shrinking its interface; e.g. by providing only a single accessor method for all kinds of `Node` children. For example, instead of having `.getTables()` in `Program` and .getRows() in `Table`, we could have a single `.getChildren()` method, implemented by all subclasses of `Node`.**

**This would shrink the interface provided by the AST classes overall, but may actually make the interface *harder to use*: any code that uses the getters may have to rely on dynamic type checks to know whether it is dealing with, for example, Tables or Rows. On the other hand, it would make it easy to write generic functions to traverse AST elements in a uniform way. Ultimately, the intended use cases of these classes should influence this design decision; while deeper modules is a good *general principle*, this needs balancing with considerations of how client code will interact with the interface itself.**

*(d) Consider again the `AST Module` from part b. How does inheritance (i.e. the `Node` class requiring the `evaluate` method to be implemented) affect the cost of the interface to this module?*

**The implementations for the `AST Module` interface are not particularly complex on their own. This led us to argue in part (b) That the `AST Module` was relatively shallow, if we simply consider the complexity hidden behind each method in the interface.**

**However, the use of inheritance significantly deepens the `AST Module`, by reducing the overall cost of the interface. Rather than relying on the developer to use a different `evaluate` method per subclass (e.g., evaluateRow), inheritance will make sure the single `evaluate` method that will dispatch to the correct subclass. In a way, we can see that the `evaluate` method hides the complexity of the *class inheritance structure*: one does not need to know that BoldRow is an instance of Row, and Row an instance of Node, to use the `evaluate` method correctly.**

**Overall, this shows that a complicated function implementation is not the only aspect of complexity that can be addressed with good software design.**



## Q2, Q3, Q4 - Parsing stages for tinyHTML

For the `modularTests` repository discussed in class, we discussed creating modular tests for (a) the parser module and (b) the evaluator module. In Main.java in the `tinyHTML` project, we see that the overall logic is broken down into more stages than these two. In particular, the "parsing" task (getting from a string input to some usable data structure) is broken down into three distinct *stages*:

  1. **Stage 1: Lexing/Tokenization**: taking a string and producing a sequence of "tokens" (meaningfully-atomic elements of the language syntax).
  2. **Stage 2: Parsing**: taking a sequence of tokens and grouping them via rules that define the language grammar into a "parse tree".
  3. **Stage 3: Conversion** (turning the parse tree output of `TinyHTMLParser` into our own choice of data representation: an AST `Node`).

As will be discussed in Lectures 3 and 4, the lexing and parsing in our projects is performed by Java code that is *automatically generated* from ANTLR grammar specifications.

Conversion (**Stage 3** in the list above), is not strictly necessary: we could instead work directly with ANTLR's generated parse tree classes instead of writing our own AST. However, conversion is a recommended stage of any DSL implementation using ANTLR (or a similar parser generator) for two main reasons. Firstly, the data representation consisting of classes generated by ANTLR is somewhat cumbersome and can be difficult to work with. Secondly, the names/structure/hierarchy of classes generated may *change* if we make changes to the ANTLR input files; such a change will likely break compilation of all code that uses these generated classes.

**Question 2**

*(a) 
Examine the file `ParseToASTVisitor.java` to get an idea of how the conversion stage is performed. How does the choice to include a conversion stage contribute to the implementation work needed overall? How do you think it affects the complexity of our code base overall?*

**Sample Solution: The extra visitor class adds to the size (and so, somewhat to the complexity) of the overall code base by adding a new module. Having extra code e.g. may in general increase the likelihood of bugs overall.**

**However, on closer examination of the code of ParseToASTVisitor, you might notice that the program data structure returned by the TinyHTMLParser is significantly more complex to deal with than our handwritten `Node` (and subclasses) representation of ASTs. For example, check the implementation of `visitTable()` in `ParseToASTVisitor.java` and you'll see that iterating over all children is more awkward than the simple `getRows()` method in our AST). In addition, ANTLR's classes have no useful notion of `equals()` defined, and we cannot modify these generated classes.**

**Overall, we are trading off some extra implementation work (the extra module performing conversion) for a better-designed class representation of our language that is both easier to program against and more-aligned with the real features of the language rather than the details of how the parser (and its grammar) was constructed. This trade-off is worth it in terms of complexity of our codebase overall.**

*(b) In class we suggested writing tests only for the first parsing phase as a whole (Lines 18-29 of `Main.java`) and for the evaluator module as a whole (Lines 31-34 of `Main.java`). Can you think of a possible disadvantage of not testing the lexing, parsing, and conversion stages individually? What about an advantage?*

**Sample Solution: An advantage is that it is a annoying to deal with the ANTLR-generated data structures (in particular, the lack of a suitable `equals()` method makes testing awkward), so not writing tests for the lexer/parser would reduce some developer burden. Also, the structure of the classes generated by ANTLR might change substantially when changes are made to grammar rules, making maintaining these tests difficult.**

**On the other hand, reducing effort in testing code may increase debugging effort down the line. A disadvantage of not separately testing the lexer, parser, and conversion states is that it can be hard to pin-point the exact source of a bug. If a `tinyHTML`` source file does not become the expected Node object, how do we know, from a failing test, whether the issue is in the lexer spec, parser spec, or our implementation of conversion? Separate tests would make it much clearer what needs to be looked at.**

*(c) What do you think is the purpose of the code on Lines 19-21 of `Main.java` (printing all of the lexer-provided tokens)? What would be a better way to achieve this purpose than printing the tokens in the main function?*

**Sample Solution: The loop over Lines 19-20, which prints out all the tokens output by the lexer appears to be an informal kind of *exploratory testing*: not a formally written out test, but an output that is examined by the developer to check whether the lexer "looks like it's behaving as intended".**

**To make this more robust, and avoid cluttering the main function and console output, we could write tests for the lexer. While there are some difficulties here in testing the exact expected sequence of tokens (notably due to the complexity of the ANTLR-generated lexer), we could write tests that check at least basic properties of the lexed tokens. For instance, we could check that the length of a token list is what we would expect for a given tinyHTML input.**

------

Now take a look at the (only!) test in the `test/parser/ParserTest.java` file. Instead of testing all three parsing stages end-to-end, this is an attempt to test only **Stage 1** and **Stage 2** together. This means that we need to write a test that takes a `String` as input and examines the corresponding ANTLR-generated parse tree (represented by a `TinyHTMLParser.ProgramContext` object). Unfortunately, these generated classes don't provide simple constructors; each constructor requires additional arguments that are connected to the underlying parse algorithm's logic. This makes writing simple `equals()` checks in tests (as we did in our first `modularTests` project) impossible (furthermore, it doesn't seem that ANTLR-generated classes have a suitable definition of an `equals()` method).

Instead, this test takes the result of parsing via the ANTLR-generated code and then checks various expected properties of the result. Read through the test; you might also want to look at some of the ANTLR-generated classes involved in representing the parse tree.

If you look at some of the checks in the test code itself, they may be surprising: in particular the number of `children` that each node in ANTLR's parse tree has depends on the number of elements each rule in its input grammar expects; this includes e.g. explicit representations of the operators used to start and separate table rows.

**Question 3**

*(a) Try running this test (it should pass; if not, you might need to make sure your project is set up to compile, and that you've run ANTLR on the `TinyHTMLParser.g4` file once, so that it generates the appropriate classes).*

**Sample Solution: nothing to say, here!**

*(b) Right now, the rules in `TinyHTMLParser.g4` contain a trick that causes the generated code to produce objects of a different class for the first row in a table (which makes it easy for us to change its behaviour later and have it printed in bold). Let's imagine we change this design and instead decide to simplify the grammar as follows: comment-out or delete the line `boldrow: row;` and, in the line above it, change the usage of `boldrow` to be `row`. Now rerun ANTLR on this file, and then try to rerun the test. What happens, and why?*

**The original test file no-longer compiles; the structure of the generated classes has changed as a result of our parser grammar change, and these classes are no-longer compatible with the test code we wrote (in particular, the class corresponding to the `boldrow` rule doesn't exist any more).**


-----

Writing tests against unstable interfaces can be quite frustrating; if an interface's tests need changing too often, this can be sign that this is either not a good interface design, or not a good choice of module boundary. In this case, testing against generated ANTLR parse trees is problematic. 

**Question 4**

_(a) Suppose that instead of testing **Stage 1** and **Stage 2** as above, that we combine two other kinds of parser tests: Firstly, end-to-end tests that check that input strings result in expected AST `Node` instances after all three Stages of parsing have completed, and secondly, tests that check the functionality of only **Stage 3**: i.e. testing the functionality of the `ParseToASTVisitor.java` file. Is there any value in combining these two kinds of tests?_

**This combination helps us to *partially* localise bugs: if one of our end-to-end tests fails, this indicates that something is not behaving as expected in at least *one* of the three stages; the tests of *Stage 3 only* then help us to work out whether this is a problem in this stage or in the ANTLR-generated code (in which case, we should focus on our grammar definitions). Furthermore, by avoiding testing Stages 1 and 2 individually, we minimise the amount of testing performed against the potentially-unstable ANTLR-generated classes.**

_(b) Imagine you are building a different software project that uses a library whose interface changes frequently from version to version. How is this situation similar to the issues we saw here with testing the ANTLR-generated **Stages 1 and 2**?_

**Sample Solution: The challenges of building code that works against an unstable API (e.g. the interface to a library) are related in that we would like to test our code that interacts with the API (and perhaps also write some tests which sanity-check that the API methods are behaving as we expected). However, these tests will be brittle: since they inevitably involve usages of elements of the library interface, changes to this interface will likely stop these tests from even compiling.**

*(c) Suppose that during your projects you do not cleanly separate modules in your own project teams, and your code depends on the internal details of your teammates. How could this create similar problems to those that we've seen here? Are these problems limited to writing convenient tests?*

**Sample Solution: Code which depends on the internal details of other modules will have similar issues as changes are made to the implementations of those modules. For example, depending on the names/precise types of fields in other classes is brittle to changes in the internal class design that might refactor these choices. In turn, this means that changes in one module can easily break the compilation and functionality of many other modules.**

**This problem is sometimes inevitable if we make a major refactoring which forces even the interfaces of the modules (or the data sent across them) to fundamentally change (cf. the previous part of the question), but if we make sure that dependencies between different modules are limited to clearly defined interfaces, then internal implementation changes that *don't* change these interfaces (or the behaviours they promise) are guaranteed not to break other modules. This applies to the tests written for those modules, but equally the actual implementation code and what the modules do; it is a very good principle to cleanly separate module interactions into well-defined and stable interfaces, as much as we can.**