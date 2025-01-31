# Grammar Rules
- non-terminal symbol
- grammar rule operator
- grammar rule body
- token literals
- grammar rules end with simicolons
- EBNF from 310
## [[LL(1) Grammar]]
### Unambiguous
**Definition:** A grammar is ambiguous if there exists a string that can be derived in more than one way (i.e., it has multiple leftmost derivations or parse trees).
For the input `3 + 4 + 5`, multiple parse trees could exist, differing in how the additions are grouped.
```antlr
expr: term '+' expr | term;
```
### Cannot be Left-recursive
**Definition:** Left recursion occurs when a non-terminal appears as the leftmost symbol in one of its production rules, directly or indirectly.
### Locally-Deterministic
**Definition:** A grammar is **locally-deterministic** if the parser can decide which rule to apply by looking only at the next token (lookahead of 1).
**Implication:** For each non-terminal, the first terminal of every alternative must be distinct. This allows for unambiguous decision-making using just one token.