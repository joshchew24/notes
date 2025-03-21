# CPSC 410 - Exercise Sheet 5: Program Slicing

Submission Deadline: Friday 21st March by 5pm (Vancouver time!)

**What to submit:** Upload files to Canvas for Q1, Q2: as usual any reasonably-standard text/document format is fine. You don't need to submit any code you write.

### Assessed Learning Objectives

(38). explain the key ideas of the Static Program Slicing technique. (**Q2**, **Q3**)

(39). apply the Static Program Slicing technique to produce program slices for simple imperative programs. (**Q1**, **Q3**)

(40). explain the causes of approximation for static program analyses, giving examples. (**Q1**, **Q2**, **Q3**)

(41). contrast dynamic program analyses with their static counterparts, explaining the typical trade-offs. (**Q1**)

(43). apply the Dynamic Program Slicing technique to produce program slices for simple imperative programs. (**Q1**)




--- 
### Applying Program Slicing

<a name="ex1"></a>
**Question 1**

(a) *Apply the dependency analysis from the Static Program Slicing technique to the following code-snippet (in Java, with line numbers). 
Write your answers next to (a copy of) the code below (you don’t need to also compute any actual program slices):*

```
1	int a = 4;
2	int b = 0;
3	b = b + 3;
4	a = a + a;
5	if (a < a - 1) {
6	  b = 17;
7	}
8	print b;
```


(b) *According to our Static Program Slicing Technique, what program slice would we get if we were only interested in preserving the value of b at line 8?.*


(c) *Write a new copy of the same original code and show alongside it the results of applying instead our Dynamic Program Slicing technique to the program (showing the values of the sets and lists as they would be computed for one particular run of the program), in a similar style to the examples we have seen in the lectures.*


(d) *If you use the results from Dynamic Program Slicing (i.e. the dependencies in part (c)), what program slice would you get as a result (again for the value of b at line 8)?*


(e) *What is the underlying reason for the difference between your answers in parts (a) and (c) (and analogously, for the difference between the slices in parts (b) and (d))? Your answer should include at least some reflection on relevant features of this specific program.*


(f) *As we discussed in the lectures, simply taking a program slice based on a __single execution__ of our Dynamic Program Slicing technique is not very useful/meaningful for general programs. What is special about this particulary program that makes even this single result actually keep just the right lines in the slice?*

--- 
### Extending Static Program Slicing

<a name="ex2"></a>
**Question 2**

In the lectures we've introduced Static Program Slicing rules to handle __for__ loops (and applied them to various examples).

(a) *Suppose instead that you wanted a rule to handle __while__ loops. What differences would you propose in the corresponding rules?*

(b) *Suppose that you wanted to extend the rules to handle a Java `do-while` loop. What would the main differences be from handling while loops?*

(c) *In the simple Java subset supported by our Static Program Slicing technique, we haven't considered class types; in particular, these allow for potential aliasing between variables. How would possible aliasing between variables potentially complicate the rules for this static analysis (hint: think about the current rule for handling variable assignments)?*

--- 
### Static Program Slicing for TinyVars

In the master branch of our TinyVars repo there's now a version of the language with all of the features developed in the course so far. In this question, we'll consider adapting our Static Program Slicing technique to this language.

<a name="ex3"></a>
**Question 3 (Unassessed)**

(a) *Implement static program slicing for TinyVars __without__ the `alias` or `undef` statement (i.e. your implementation can generate an error for a program which uses these features). Your implementation should operate on the TinyVars AST (likely using a visitor).*

(b) *Supporting the ```alias``` statement is challenging for the same reasons as for aliasing in Java (cf. Question 2(c)). Suppose we implemented basic functions in TinyVars and function calls. Why would this complicate the dependency analysis we do for program slicing. (Hint: what similarity is there between aliasing and handling function calls?)*

(c) *Why would implementing Dynamic Program Slicing via source code instrumentation be difficult for this language?*