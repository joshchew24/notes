# Exercise Sheet 1: Reducing Complexity through Modular Design

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

This exercise sheet will ask you to examine the code in the [tinyHTML](https://github.students.cs.ubc.ca/CPSC410-2024W-T2/tinyHTML) repository. Download the repository and familiarize with the code in it.

Recall that in class we defined a module as:
> a subset of a software system that has a specific purpose, (relatively) independent of the other modules in the system, encompassing an *interface* and and *implementation*.

Refer first to the main function provided by `tinyHTML/src/ui/Main.java`. Like the example in class, there is a `Node` data structure, representing an Abstract Syntax Tree used to connect the parsing code (Lines 18-29) to the evaluator code (Lines 31-34).
This `Node` data structure is defined in `tinyHTML/src/libs/Node.java`, and subclasses of it are defined in `tinyHTML/src/ast`. 

**Question 1**

*(a) Can we consider the `Node` data structure a module? Why or why not? Should such a module include only the code in `tinyHTML/src/libs/Node.java`, or any (subset of) the files in `tinyHTML/src/ast`? Why or why not?*

*(b) Let us suppose we consider  `tinyHTML/src/libs/Node.java` and the classes implemented `tinyHTML/src/ast` part of the `AST Module`. Looking at the classes in `tinyHTML/src/ast`, would you say the `AST Module` is relatively shallow, or relatively deep? Why or why not?*

*(c) Consider the notion of deep vs. shallow modules, as discussed in the first lecture. Is it __possible__ to make the AST Module deeper/shallower? Is it __desirable__ (especially from a "reducing complexity" standpoint) to make the AST Module deeper/shallower? Why or why not?*

*(d) Consider again the `AST Module` from part b. How does inheritance (i.e. the `Node` class requiring the `evaluate` method to be implemented) affect the cost of the interface to this module?*


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

*(b) In class we suggested writing tests only for the first parsing phase as a whole (Lines 18-29 of `Main.java`) and for the evaluator module as a whole (Lines 31-34 of `Main.java`). Can you think of a possible disadvantage of not testing the lexing, parsing, and conversion stages individually? What about an advantage?*

*(c) What do you think is the purpose of the code on Lines 19-21 of `Main.java` (printing all of the lexer-provided tokens)? What would be a better way to achieve this purpose than printing the tokens in the main function?*

------

Now take a look at the (only!) test in the `test/parser/ParserTest.java` file. Instead of testing all three parsing stages end-to-end, this is an attempt to test only **Stage 1** and **Stage 2** together. This means that we need to write a test that takes a `String` as input and examines the corresponding ANTLR-generated parse tree (represented by a `TinyHTMLParser.ProgramContext` object). Unfortunately, these generated classes don't provide simple constructors; each constructor requires additional arguments that are connected to the underlying parse algorithm's logic. This makes writing simple `equals()` checks in tests (as we did in our first `modularTests` project) impossible (furthermore, it doesn't seem that ANTLR-generated classes have a suitable definition of an `equals()` method).

Instead, this test takes the result of parsing via the ANTLR-generated code and then checks various expected properties of the result. Read through the test; you might also want to look at some of the ANTLR-generated classes involved in representing the parse tree.

If you look at some of the checks in the test code itself, they may be surprising: in particular the number of `children` that each node in ANTLR's parse tree has depends on the number of elements each rule in its input grammar expects; this includes e.g. explicit representations of the operators used to start and separate table rows.

**Question 3**

*(a) Try running this test (it should pass; if not, you might need to make sure your project is set up to compile, and that you've run ANTLR on the `TinyHTMLParser.g4` file once, so that it generates the appropriate classes).*

*(b) Right now, the rules in `TinyHTMLParser.g4` contain a trick that causes the generated code to produce objects of a different class for the first row in a table (which makes it easy for us to change its behaviour later and have it printed in bold). Let's imagine we change this design and instead decide to simplify the grammar as follows: comment-out or delete the line `boldrow: row;` and, in the line above it, change the usage of `boldrow` to be `row`. Now rerun ANTLR on this file, and then try to rerun the test. What happens, and why?*


-----

Writing tests against unstable interfaces can be quite frustrating; if an interface's tests need changing too often, this can be sign that this is either not a good interface design, or not a good choice of module boundary. In this case, testing against generated ANTLR parse trees is problematic. 

**Question 4**

_(a) Suppose that instead of testing **Stage 1** and **Stage 2** as above, that we combine two other kinds of parser tests: Firstly, end-to-end tests that check that input strings result in expected AST `Node` instances after all three Stages of parsing have completed, and secondly, tests that check the functionality of only **Stage 3**: i.e. testing the functionality of the `ParseToASTVisitor.java` file. Is there any value in combining these two kinds of tests?_

_(b) Imagine you are building a different software project that uses a library whose interface changes frequently from version to version. How is this situation similar to the issues we saw here with testing the ANTLR-generated **Stages 1 and 2**?_

*(c) Suppose that during your projects you do not cleanly separate modules in your own project teams, and your code depends on the internal details of your teammates. How could this create similar problems to those that we've seen here? Are these problems limited to writing convenient tests?*