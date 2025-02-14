# Exercise Sheet 3 Response
## Question 1
a. The methods called before the first table row is printed are: `t.evaluate(writer)`, `color.evaluate(writer)`. The first table row will not be printed in bold because the loop that evaluates the rows does not dispatch to the `BoldRow.evaluate()` method.
b. The methods called before the first table row is printed are: `Evaluator.evaluate(Table ta, PrintWriter writer)`, `Evaluator.evaluate(ta.getColour(), writer)`