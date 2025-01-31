# Exercise Sheet 2: Advanced Tokenization and Parsing

Submission Deadline: Friday 31st of January by 5pm. See Canvas.

**What to submit:** Written solutions to Questions 1,3,5 in any reasonably-standard text/document format is fine. 

### Assessed Learning Objectives

7. describe the key stages of a typical DSL implementation, including their roles. 
8. explain trade-offs regarding whether to have particular stages do more or less work (e.g. fine-grained tokenization, grammar precision, validation).  **(Q1, Q3, Q5, Q6)**
9. propose a suitable tokenization for given input examples in a given language. **(Q1, Q3)**
10. design a corresponding grammar for a given language and tokenization. **(Q5)**
11. construct appropriate lexer and parser grammars for use as ANTLR inputs.  **(Q2, Q3)**
12. identify whether a grammar is ambiguous, left-recursive, and (not) locally deterministic **(Q5)**
13. rewrite a grammar to be unambiguous, no longer left-recursive, and locally deterministic **(Q5)**



If you have not followed the instructions for installing ANTLR from [Exercise Sheet 1](https://github.students.cs.ubc.ca/CPSC410-2024W-T2/Exercise1/blob/main/Exercise_1.md), you'll need to do that first.


## Tokenizing VerboseTurtle

[Turtle graphics](https://en.wikipedia.org/wiki/Turtle_graphics) are vector graphics produced by controlling the movement of a cursor (the "Turtle") across a canvas. The idea of Turtle graphics comes from the [LOGO](https://en.wikipedia.org/wiki/Logo_(programming_language)) programming language.

In this exercise sheet we will consider a simple language for expressing such graphics, VerboseTurtle. VerboseTurtle allows us to (verbosely) specify the path of the turtle cursor across the screen. For simplicity we will only allow the turtle to move in four directions: up, down, left, right. We will also allow the pen to be up or down, so that when the turtle "walks", we can either draw a line following its path, or not. 

The turtle starts in the middle of the canvas, facing right, with pen down. Here is an example of a program in VerboseTurtle:

```
Create a turtle named Speedy
Stop writing
Walk 20 pixels
Face UP
Start writing
Walk 20 pixels
Face LEFT
Walk 40 pixels
Face DOWN
Walk 40 pixels
Face RIGHT
Walk 40 pixels
Show me Speedy's drawing
```

it results in the following (almost complete) drawing of a square:
![[Pasted image 20250130233600.png]]


Here is a draft grammar for VerboseTurtle. This grammar mixes token definitions with parser rules, which we won't do for our eventual solution.

```
program: 'Create a turtle named' TEXT statement* 'Show me' TEXT '\'s drawing' EOF ;
statement: penop | dirop | walkop ;
penop: 'Start writing' | 'Stop writing' ;
dirop: 'Face' DIR ;
walkop: 'Walk' NUM 'pixels';
DIR: 'UP' | 'DOWN' | 'LEFT' | 'RIGHT' ;
TEXT: [a-zA-Z]+ ;
NUM: [0-9]+ ;
```

**Note:** Similarly to for TinyHTML in class, we assume here that our tokenizer will strip out any whitespace in-between tokens (we will see how this is done in the implementation itself).

<a name="ex1"></a>
**Question 1**

*(a) This grammar has multiple sequences of words that are only allowed in one order, e.g. 'Create a turtle named'. What is an advantage to considering this whole sequence as a token? What is a disadvantage of considering this whole sequence as a token?*

*(b) Assume for now that we consider the sequences such as 'Create a turtle named' as a single token. What are the tokens this grammar contains?* 

## Using ANTLR as a Tokenizer

While ANTLR can generate both tokenizers and parsers, we only use it for the former in this exercise and ask you to write your own parser by hand (as we will see, this isn't too hard if you define the grammar according to the guidelines from the course). This means that instead of a parse tree, you will work with a raw list of tokens that ANTLR has generated and build an AST with them.

For this exercise, we have provided you with a template project that contains the AST for VerboseTurtle with a fully functional evaluator. You can find the code in the "VerboseTurtle-starter" folder of the exercise repository.

Note that you have to call ANTLR at least once to generate code for the "VerboseTurtleLexer.g4" file (until you do so, the code that depends on the generated Java file will have errors). Then you should be able to compile the project even before you complete the remaining questions below.

<a name="ex2"></a>
**Question 2 [Unassessed: no need to submit the code, but a solution will be provided]**

*Based on the list of tokens you came up with in Question 1, complete the ANTLR lexer definition file (called `VerboseTurtleLexer.g4`). As with the in-class example, you will need to use modes to get things to parse correctly.*

*We have provided two test cases. Both check for the number of tokens assuming the tokenization described in 1(b), and assume that the token for the turtle name and 'Walk' should be different types of tokens. In a correct solution, you should also make sure you do not see any ANTLR token recognition errors printed to the console when running these tests.*


**Question 3**

The first test case, named **speedyTest**, for our tokenizer checked that the input:

```
Create a turtle named Speedy
Walk 10 pixels
Face UP
Walk 10 pixels
Show me Speedy's drawing
```
has 13 tokens, and that the tokens for the first `Speedy` and `Walk` are different.

Parts (a) and (b) ask you to consider what would happen if you had kept the lexer grammar very simple and *not used any ANTLR modes*, while still including rules to define all the tokens you defined in 1(b).

*(a) If we did not use ANTLR modes to implement a tokenizer in Question 2, would __speedyTest__ fail? If it does not fail, why are modes not necessary in this case? If it does fail, how? (i.e. would the assertion on the number of tokens fail, and/or the assertion that 'Speedy' and 'Walk' are different types of tokens fail?) Would the number of tokens generated be different? Explain briefly why/why not.*

The other test case, named **walkerTest** for our tokenizer checks that the input:

```
Create a turtle named Walker
Walk10pixels
FaceUP
Walk10pixels
Show me Walker's drawing
```
has 13 tokens, and that the tokens for the first `Walker` and `Walk` are different.

*(b) If we did not use ANTLR modes to implement a tokenizer in Question 2, would __walkerTest__ fail? If it does not fail, why are modes not necessary in this case? If it does fail, how? (i.e. would the assertion on the number of tokens fail, and/or the assertion that 'Walker' and 'Walk' are different types of tokens fail?) Would the number of tokens generated be different? Explain briefly why/why not.*

For the rest of the question, assume the lexer grammar is correctly specified using modes.

*(c) Suppose we want to extend our language to optionally recognize directions in walk statements, i.e.:*
`walkop: 'Walk' DIR? NUM 'pixels'`
*Do we need to change our lexer to support these statements? Why/why not?*


## Writing a Parser by Hand

Once you have completed the lexer (tokenizer) definition file, you can generate a Java lexer for VerboseTurtle with ANTLR. If you test your program now, you should see that it prints the list of tokens in the given example input. All that is left to do is to use these tokens to build an AST for VerboseTurtle.

For this exercise, we will use a relatively simple parsing technique called *recursive descent*. The idea of recursive descent is that as we iterate over the list of tokens, we start by parsing the ```Program``` node of the AST, then parse its child nodes, their children and so on.

For VerboseTurtle, we only have two levels: the program and its statements. We have already started writing the parsing functionality for the ```Program``` case for you (see ```parser/VerboseTurtleParser.java``` in the given files). As you can see, we are using a helper class called ```Tokens``` that simplifies working with the list of tokens that ANTLR produced. We recommend that you start by looking at the documentation of ```Tokens``` and how it is used in ```Program```.

<a name="ex3"></a>
**Question 4  [Unassessed: no need to submit the code, but a solution will be provided]**

*(a) Complete the parser for VerboseTurtle by adding support for edges to ```Program``` and implementing the parsing for ```Walk``` and ```ChangeDirection```. Think about how to translate the grammar rules into parsing code, using recursion wherever the rule requires a non-terminal as a part of the right-hand-side of a rule.*

*If your implementation is complete and correct, you should be able to run it on the given input file and get the generated image.*

*(b) Recall that in this course we require grammars not to have left-recursion: can you see what problem this would cause for writing a recursive descent parser in this simple way?*

## Grammar and Language Restrictions 

Recall that the parsing strategies we use in the lectures make three requirements on the grammar used:

1. The grammar cannot be ambiguous
2. The grammar cannot be left-recursive
3. The grammar must be locally-deterministic

ANTLR supports a fairly wide range of grammars, even if they violate some of these requirements. However, if the above requirements are violated the resulting code can be inefficient or produce unexpected results. For example when a grammar is ambiguous, ANTLR has to "guess" how you intended the grammar to be parsed, and may or may not give the parse tree you expected. 

VerboseTurtle as above has names for the turtles but these are not used anywhere. Suppose we want to change the language to allow multiple turtle definitions, and label the statements with the corresponding turtle. For instance:

```
Create a turtle named Speedy
Create a turtle named Shelly
Speedy: Stop writing
Speedy: Walk 20 pixels
Speedy: Face UP
Speedy: Start writing
Speedy: Walk 20 pixels
Shelly: Walk 20 pixels
Shelly: Face LEFT
Show me Speedy's drawing
Show me Shelly's drawing
```

We propose rewriting the grammar as follows:

```
program: ('Create a turtle named' TEXT)+ statement* ('Show me' TEXT '\'s drawing')+ EOF ;
statement: TEXT ': ' penop |  TEXT ': ' dirop | TEXT ': ' walkop ;
penop: 'Start writing' | 'Stop writing' ;
dirop: 'Face' DIR ;
walkop: 'Walk' NUM 'pixels' ;
DIR: 'UP' | 'DOWN' | 'LEFT' | 'RIGHT' ;
TEXT: [a-zA-Z]+ ;
NUM: [0-9]+ ;
```

<a name="ex4"></a>
**Question 5**

*(a) Which of the three requirements above do these new grammar rules no longer satisfy? Rewrite these rules so that they satify all three requirements (you only need to deal with the rules immediately above this question).* 

Somewhere in our eventual language implementation, we would like to restrict the space of valid programs to be those which label statements with a pre-defined turtle name. That is,  

```
Create a turtle named Speedy
Speedy: Stop writing
Show me Speedy's drawing
```

is a valid program, but 

```
Create a turtle named Speedy
Shelly: Stop writing
Show me Speedy's drawing
```
is not.

Let us call this restriction the *well-labelled turtle restriction*. 

*(b) Does the grammar above enforce the well-labelled turtle restriction? Why/why not?*  

*(c) Is it natural to enforce the well-labelled turtle restriction in the tokenization/parsing stage of our DSL? Why/why not?* 

<a name="ex5"></a>
**Question 6 [Unassessed]**

*(a) Is it possible to modify the code for our recursive descent parser (from Question 3) to enforce the well-labelled turtle restriction (from Question 5)? If not, justify why not. If yes, what would we have to add to the recursive descent parser enforce the well-labelled turtle restriction? You can describe the operations at a high level; you do not need to directly modify the code.*