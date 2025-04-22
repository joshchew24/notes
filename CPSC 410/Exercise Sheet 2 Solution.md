# Exercise Sheet 2 Solution

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

![](simple-turtle.png) 


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

**Sample Solution: An advantage of tokenizing 'Create a turtle named' as a single token is that this reduces the need for checks later. If each word were tokenized as an individual token, our parser would need to ensure that the words occur in a correct order. A disadvantage is that this may result in our language being stricter about whitespace than we really want: we might want to allow the user to have arbitrary whitespace in these word sequences, e.g. 4 spaces between "Create" and "a". This could still be achieved with ANTLR but with a much more-complicated token definition than we'd naturally think of here.**


*(b) Assume for now that we consider the sequences such as 'Create a turtle named' as a single token. What are the tokens this grammar contains?* 

**Sample Answer: Assuming we are tokenizing those sequences as a single token, we would expect tokens that whose *definitions* match each of the following (of course, you might pick any *names* for your particular tokens): `Create a turtle named`, `Show me`, `'s drawing`, `Start writing`, `Stop writing`, `Face`, `Walk`, `pixels`, `UP`, `DOWN`, `LEFT`, `RIGHT`, `[a-zA-Z]+`, `[0-9]+`. The last two would correspond to tokens that represent turtle names and numeric constants for distances, respectively.**

## Using ANTLR as a Tokenizer

While ANTLR can generate both tokenizers and parsers, we only use it for the former in this exercise and ask you to write your own parser by hand (as we will see, this isn't too hard if you define the grammar according to the guidelines from the course). This means that instead of a parse tree, you will work with a raw list of tokens that ANTLR has generated and build an AST with them.

For this exercise, we have provided you with a template project that contains the AST for VerboseTurtle with a fully functional evaluator. You can find the code in the "VerboseTurtle-starter" folder of the exercise repository.

Note that you have to call ANTLR at least once to generate code for the "VerboseTurtleLexer.g4" file (until you do so, the code that depends on the generated Java file will have errors). Then you should be able to compile the project even before you complete the remaining questions below.

<a name="ex2"></a>
**Question 2 [Unassessed: no need to submit the code, but a solution will be provided]**

*Based on the list of tokens you came up with in Question 1, complete the ANTLR lexer definition file (called `VerboseTurtleLexer.g4`). As with the in-class example, you will need to use modes to get things to parse correctly.*

*We have provided two test cases. Both check for the number of tokens assuming the tokenization described in 1(b), and assume that the token for the turtle name and 'Walk' should be different types of tokens. In a correct solution, you should also make sure you do not see any ANTLR token recognition errors printed to the console when running these tests.*


**Sample Answers**

**Please see the solution code in the `verboseTurtle-solution` folder.**

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

**Sample Solution: The answer actually almost certainly *depends* on the *order in which you wrote your tokenizer rules*! If you put a token declaration such as `TEXT: [a-zA-Z]+` near the *end* of your rules, your tokenizer likely runs as you wanted even without modes. However, if you put it e.g. second in your list of rules, you most-likely have problems (and the test fails).**

**The reason is that, without ANTLR modes, a `TEXT: [a-zA-Z]+` lexer rule also matches e.g. the `Speedy` and `Walk` strings, which we intend to be tokenized as other tokens; we have overlapping token definitions in our rules. In practice, ANTLR resolves these overlaps by (a) seeing which rule matches the *longest* string from where we are, and, if tied (as in this case), (b) choosing the token whose *rule is first* in the list of lexer rules. This is why, depending on how you wrote the file, you might have "gotten lucky" and the test still passes, or not (as `TEXT` will be grabbing these strings when you intended other tokens).**

**In general, it's a bad idea to rely on ANTLR's rules for handling these overlaps, as this is very brittle: as we see here, changing the order of the rules may dramatically change the behaviour of your tokenizer, and other subtle changes (such as allowing space charaters in `TEXT`) might also change this behaviour (if your test didn't fail before, see what happens if you do this!). Modes are a more readable and robust mechanism for being explicit about how to resolve these overlaps.**


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

**Sample Answer: The test here will fail on the number of tokens, because `FaceUP` will be captured as a single token by TEXT. In particular there will be 12 instead of 13 tokens (but not even fewer because the `TEXT` rule cannot match `Walk10pixels` as a single token).**

For the rest of the question, assume the lexer grammar is correctly specified using modes.

*(c) Suppose we want to extend our language to optionally recognize directions in walk statements, i.e.:*
`walkop: 'Walk' DIR? NUM 'pixels'`
*Do we need to change our lexer to support these statements? Why/why not?*

**Sample Answer: Assuming we put at most `TEXT` into its own special mode, we won't need to change the lexer definition to recognize these statements. Both `DIR` and `NUM` can be recognized in default mode.**

## Writing a Parser by Hand

Once you have completed the lexer (tokenizer) definition file, you can generate a Java lexer for VerboseTurtle with ANTLR. If you test your program now, you should see that it prints the list of tokens in the given example input. All that is left to do is to use these tokens to build an AST for VerboseTurtle.

For this exercise, we will use a relatively simple parsing technique called *recursive descent*. The idea of recursive descent is that as we iterate over the list of tokens, we start by parsing the ```Program``` node of the AST, then parse its child nodes, their children and so on.

For VerboseTurtle, we only have two levels: the program and its statements. We have already started writing the parsing functionality for the ```Program``` case for you (see ```parser/VerboseTurtleParser.java``` in the given files). As you can see, we are using a helper class called ```Tokens``` that simplifies working with the list of tokens that ANTLR produced. We recommend that you start by looking at the documentation of ```Tokens``` and how it is used in ```Program```.

<a name="ex3"></a>
**Question 4  [Unassessed: no need to submit the code, but a solution will be provided]**

*(a) Complete the parser for VerboseTurtle by adding support for edges to ```Program``` and implementing the parsing for ```Walk``` and ```ChangeDirection```. Think about how to translate the grammar rules into parsing code, using recursion wherever the rule requires a non-terminal as a part of the right-hand-side of a rule.*

*If your implementation is complete and correct, you should be able to run it on the given input file and get the generated image.*


**Sample Answer: Please see the sample code. Rendering the example input with Main.java should give you:**

![](solution-turtle.png)

**Our test suite is not complete; please report any bugs or strange behaviors in our sample code :)**

*(b) Recall that in this course we require grammars not to have left-recursion: can you see what problem this would cause for writing a recursive descent parser in this simple way?*

**Sample Answer: The natural implementation of a parser rule in this style would lead to a method whose first step would be to call itself without having consumed any input. That is, the parser implementation would loop infinitely in this case.**


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

**Sample Answer: The grammar is no longer locally deterministic: each of the three rules for statements starts with the same (two) tokens. We can rewrite the relevant part of the grammar as follows:**

```
statement: TEXT ": " operation ;
operation: penop | dirop | walkop ;
[rest of grammar is the same]
```


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

**Sample Answer: The grammar does not enforce the well-labelled turtle restriction: it accepts arbitrary text for any turtle name; there is no attempt to keep track of which have been previously "declared".**

**Note that it is not actually possible to enforce this restriction at the parsing stage based on the kinds of parser grammars we use in the course: these are *context free grammars*. *Context-free* means that each parsing rule depends only on what comes *next* in the input, not on what came before (we don't need any "contextual knowledge" to decide whether a rule can be applied next). However, we cannot keep track of which turtle names are valid without rembering which have already been declared: this requires additional context.**

*(c) Is it natural to enforce the well-labelled turtle restriction in the tokenization/parsing stage of our DSL? Why/why not?* 

**Sample Answer: Arguably, it is not natural to enforce this restriction at the lexing/parsing stage of our DSL: for one thing (as discussed above re: context-free grammars) it isn't really possible with this kind of parser definition. But if e.g. writing a parser by hand, another reason is that tracking mutable state as part of the logic of the parser itself will likely make the parser implementation even more complex to get right and to debug. Instead, if we make the parser ignore this extra condition on valid programs, we can easily separate this concern into a second checker: arguably a more-modular approach to ultimately deciding which inputs are OK in all senses.**

<a name="ex5"></a>
**Question 6 [Unassessed]**

*(a) Is it possible to modify the code for our recursive descent parser (from Question 3) to enforce the well-labelled turtle restriction (from Question 5)? If not, justify why not. If yes, what would we have to add to the recursive descent parser enforce the well-labelled turtle restriction? You can describe the operations at a high level; you do not need to directly modify the code.*

**Sample Answer: Although (as discussed in the previous part), this complicates the code and strategy for writing a parser by hand, it is possible to do so if we are writing our own: we can in principle use arbitrary code. The implementation would need to save the turtle names when parsing the "Create a turtle named Speedy" part of the program. It could save these into some internal data structure (say, a set of `validTurtleNames`). Then when parsing statements, check that the token before the `": "` in the label is in the set `validTurtleNames`.**