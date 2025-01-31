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
(c) We do need to 