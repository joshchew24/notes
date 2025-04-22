# Exercise Sheet 5 Solution

Submission Deadline: Friday 21st March by 5pm (Vancouver time!)

**What to submit:** Upload files to Canvas for Q1, Q2: as usual any reasonably-standard text/document format is fine. You don't need to submit any code you write.

### Assessed Learning Objectives

(38). explain the key ideas of the Static Program Slicing technique. (**Q2**, **Q3**)

(39). apply the Static Program Slicing technique to produce program slices for simple imperative programs. (**Q1**, **Q3**)

(40). explain the causes of approximation for static program analyses, giving examples. (**Q1**, **Q2**, **Q3**)

(41). contrast dynamic program analyses with their static counterparts, explaining the typical trade-offs. (**Q1**)

(42). contrast the pros and cons of the main implementation methods for dynamic program analyses (when instrumentation is performed) (**Q3**)

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

**Sample Answer: see below.**

```
1	int a = 4;         (a->{1})                    []
2	int b = 0;         (a->{1}, b->{2})            []
3	b = b + 3;         (a->{1}, b->{2,3})          []
4	a = a + a;         (a->{1,4}, b->{2,3})        []
5	if (a < a - 1) {   (a->{1,4}, b->{2,3})        [{1,4}]
6	  b = 17;          (a->{1,4}, b->{1,4,6})      [{1,4}]
7	}                  (a->{1,4}, b->{1,2,3,4,6})  []
8	print b;           (a->{1,4}, b->{1,2,3,4,6})  []
```


(b) *According to our Static Program Slicing Technique, what program slice would we get if we were only interested in preserving the value of b at line 8?.*

**Answer: see below (the whole program is preserved).**

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


(c) *Write a new copy of the same original code and show alongside it the results of applying instead our Dynamic Program Slicing technique to the program (showing the values of the sets and lists as they would be computed for one particular run of the program), in a similar style to the examples we have seen in the lectures.*

**Answer: see below.**

```
1	int a = 4;         (a->{1})              []
2	int b = 0;         (a->{1}, b->{2})      []
3	b = b + 3;         (a->{1}, b->{2,3})    []
4	a = a + a;         (a->{1,4}, b->{2,3})  []
5	if (a < a - 1) {   
6	  b = 17;          (nothing shown here as it is not executed)
7	}                  (a->{1,4}, b->{2,3})  []
8	print b;           (a->{1,4}, b->{2,3})  []
```


(d) *If you use the results from Dynamic Program Slicing (i.e. the dependencies in part (c)), what program slice would you get as a result (again for the value of b at line 8)?*


**Answer: see below**

```
1	int a;
2   int b = 0;	
3   b = b + 3;	
8   print b;	
```

**You might notice that we keep the declaration of `a` on line 1 even though this line wasn't in the relevant set. Our rule from the lectures is to always keep declarations (but remove any initializer) when a variable declaration line is not found to be relevant; this is to avoid cases where the variable value at this line does not matter but the variable is reassigned later (we wan't the code to still compile!). However, you might prefer an improvement of our technique which looks at the resulting program slice and also deletes declarations of variables that are *never* used in the program.**


(e) *What is the underlying reason for the difference between your answers in parts (a) and (c) (and analogously, for the difference between the slices in parts (b) and (d))? Your answer should include at least some reflection on relevant features of this specific program.*

**Answer: Static program slicing over-approximates control flow paths, while dynamic program slicing explores a single such path precisely while the program runs. For this program, the if-condition is always false (in Java), so the dynamic program slicing will not detect any dependencies due to it. The static program slicing still treats this as a possible path, and the resulting over-approximation makes the difference.**



(f) *As we discussed in the lectures, simply taking a program slice based on a __single execution__ of our Dynamic Program Slicing technique is not very useful/meaningful for general programs. What is special about this particulary program that makes even this single result actually keep just the right lines in the slice?*


**Answer: This program only has a single possible execution, so the dynamic program slicing actually captures all executions of the program.**


--- 
### Extending Static Program Slicing

<a name="ex2"></a>
**Question 2**

In the lectures we've introduced Static Program Slicing rules to handle __for__ loops (and applied them to various examples).

(a) *Suppose instead that you wanted a rule to handle __while__ loops. What differences would you propose in the corresponding rules?*


**Answer: The main points about handling a while loop are extremely similar to those for for loops; the only differences is that there is no need to specially deal with the code (typically assignments) corresponding to the first and third parameters of a for-loop construct.**


(b) *Suppose that you wanted to extend the rules to handle a Java `do-while` loop. What would the main differences be from handling while loops?*


**Answer: Importantly, the first iteration of a `do-while` loop happens unconditionally; it is guaranteed to execute independently of the particular loop condition. To reflect this accurately, the analysis should traverse the loop body once *without* pushing an element onto the head of the list. We can then iterate the normal loop rules (similarly to a while loop), but using the state after this first iteration as our initial state; we will need to visit the loop body at least once more, and potentially keep on iterating in the same way as for while and for loops.**


(c) *In the simple Java subset supported by our Static Program Slicing technique, we haven't considered class types; in particular, these allow for potential aliasing between variables. How would possible aliasing between variables potentially complicate the rules for this static analysis (hint: think about the current rule for handling variable assignments)?*

**Answer: The main difficulty that aliasing adds is that it is no longer straightforward to keep track of the value dependencies between assignments and variables: an assignment to a variable `x` might update the value (and therefore the correct dependencies) for other variables too. Without good aliasing information (which could perhaps be tracked to an extent by another static program analysis), our rules for assignments would be incorrect in the presence of aliasing.**

--- 
### Static Program Slicing for TinyVars

In the master branch of our TinyVars repo there's now a version of the language with all of the features developed in the course so far. In this question, we'll consider adapting our Static Program Slicing technique to this language.

<a name="ex3"></a>
**Question 3 (Unassessed)**

(a) *Implement static program slicing for TinyVars __without__ the `alias` or `undef` statement (i.e. your implementation can generate an error for a program which uses these features). Your implementation should operate on the TinyVars AST (likely using a visitor).*

**Answer: see the `Slicer.java` in the TinyVars repo, in the `slice-impl` branch. It is not a fully-tested implementation; feel free to report bugs.**

(b) *Supporting the ```alias``` statement is challenging for the same reasons as for aliasing in Java (cf. Question 2(c)). Suppose we implemented basic functions in TinyVars and function calls. Why would this complicate the dependency analysis we do for program slicing. (Hint: what similarity is there between aliasing and handling function calls?)*

**Answer: The fact that procedure parameters and return values might create an implicit flow from the parameter values to the return values means that supporting procedure calls without inlining the bodies in some way is challenging (we could otherwise miss dependencies between the variable values returned and the variable values passed). Note that over-approximating by simply considering the return value to depend on all input parameters is not enough, since (without proper scoping) a procedure body can read and modify other variables, too.**

(c) *Why would implementing Dynamic Program Slicing via source code instrumentation be difficult for this language?*

**TinyVars' features are too limited: it would not be possible to perform suitable instrumentation of an input program, because (for example) there is no way to express the sets and maps involved in the analysis' abstract states.**