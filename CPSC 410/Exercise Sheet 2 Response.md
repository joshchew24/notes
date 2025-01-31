# Exercise Sheet 2 Response
## Question 1
(a) It is advantageous to consider this whole sequence a token because it represents the only acceptable way to use this sequence in the program. The user must match this string exactly, so tokenizing the sequence makes this easier to check as ANTLR handles it for us. It is disadvantageous to consider this sequence a token if we wanted the user to be able to have more flexibility in whitespace, because they are restricted to only one between each word. 
(b) Tokens:
- `Create a turtle named`
- `Show me`
- `\'s drawing`
- `Start writing`
- `Stop writing`
- `Face`
- `Walk`
- `pixels`
- `UP`
- `DOWN`
- `LEFT`
- `RIGHT`
## Question 3
(a) Without ANTLR modes, `speedyTest` fails because the assertion on the number of tokens is incorrect. An example of this is the `TEXT` token (`~[[|\]\r\n']+`), which also captures the `START` string (`'Create a turtle named' WS*`), because ANTLR will choose the longest possible matching string for a rule. So, the line `Create a turtle named Speedy` gets captured as a single token under the `TEXT` rule. Every line will be captured as a single token except the last one, which is split because the `TEXT` rule disallows apostrophes. 
(b) Without ANTLR modes, `walkerTest` also fails for the same reasons as `speedyTest`. The line `Create a turtle namedWalker` is captured as a single token. 
(c) We shouldn't need to modify the lexer, as the tokens haven't changed. 
## Question 5
(a) The `statement` rule is not locally deterministic, because each option starts with the same value `TEXT`. 
```
program: ('Create a turtle named' TEXT)+ statement* ('Show me' TEXT '\'s drawing')+ EOF ;
statement: TEXT ': ' op ;
op: penop | dirop | walkop ;
penop: 'Start writing' | 'Stop writing' ;
dirop: 'Face' DIR ;
walkop: 'Walk' NUM 'pixels' ;
DIR: 'UP' | 'DOWN' | 'LEFT' | 'RIGHT' ;
TEXT: [a-zA-Z]+ ;
NUM: [0-9]+ ;
```

(b) The grammar does not enforce the well-labelled turtle restriction. It simply requires that the every `statement` starts with `TEXT`, regardless of if it was previously defined in `program`. 
(c) It would not make sense to enforce this restriction in the tokenization/parsing stage of our DSL. It would make more sense to enforce this rule in the evaluation, where it is actually possible to check that the turtle has been "declared" before we try to give it directions and view its drawing. 
## Question 6
(a) We would need to maintain a mapping of turtle labels to turtle objects. `program` would create the mapping, `statement` would search the map for the turtle object and call the corresponding method, and `'Show me'` would search the map for the turtle object and render its drawing. If we search the map and cannot find the labelled turtle, then we can error to enforce the restriction. 