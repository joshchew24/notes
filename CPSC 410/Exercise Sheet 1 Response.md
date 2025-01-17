# Exercise Sheet 1 Response
**Question 1**

*(a) Can we consider the `Node` data structure a module? Why or why not? Should such a module include only the code in `tinyHTML/src/libs/Node.java`, or any (subset of) the files in `tinyHTML/src/ast`? Why or why not?*

(a) The `Node` data structure should probably not be considered a module since it is just an abstract class that is used to construct ASTs. The files in `tinyHTML/src/ast` are not implementations of the `Node` data structure 

*(b) Let us suppose we consider  `tinyHTML/src/libs/Node.java` and the classes implemented `tinyHTML/src/ast` part of the `AST Module`. Looking at the classes in `tinyHTML/src/ast`, would you say the `AST Module` is relatively shallow, or relatively deep? Why or why not?*
(b) I would say that the `AST Module` is relatively shallow, because it has a large interface while providing basic functionality of different elements of an AST. The user must construct the AST and its components by themself using the various classes and methods provided by the interface.

*(c) Consider the notion of deep vs. shallow modules, as discussed in the first lecture. Is it __possible__ to make the AST Module deeper/shallower? Is it __desirable__ (especially from a "reducing complexity" standpoint) to make the AST Module deeper/shallower? Why or why not?*
(c) it is possible to make the AST Module deeper. This could be achieved by abstracting the components of an AST behind a single "AST Factory" with methods for inserting/modifying an AST. It may be desirable as the user would not need as much background knowledge on ASTs to construct one correctly. A deeper module might contain checks and automatic operations to ensure correctness.  

*(d) Consider again the `AST Module` from part b. How does inheritance (i.e. the `Node` class requiring the `evaluate` method to be implemented) affect the cost of the interface to this module?*
