# Exercise Sheet 3 Response
## Question 1
a. The methods called before the first table row is printed are: `t.evaluate(writer)`, `color.evaluate(writer)`. The first table row will be printed in bold because the loop tells the `row` object to invoke its `evaluate()` method. The `BoldRow` will correctly call its own `evaluate` and print a bold row.
b. The methods called before the first table row is printed are: `Evaluator.evaluate(Table ta, PrintWriter writer)`, `Evaluator.evaluate(ta.getColour(), writer)`. The first table row will be printed in bold because 
