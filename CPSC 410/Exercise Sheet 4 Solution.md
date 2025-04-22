# Exercise Sheet 4 Solution
Submission Deadline: Friday 7th March by 5pm (Vancouver time!)

**What to submit:** upload files to Canvas: as usual any reasonably-standard text/document format is fine.

### Assessed Learning Objectives.

(30). design ethical empirical studies, appropriately identifying risks (and their importance) and strategies for mitigating risks when possible. (**Q2**)

(31). evaluate empirical studies for threats to validity, judge their likely impact on a study’s conclusion, and propose strategies for mitigating these when possible. (**Q1**, **Q2**)

(32). justify the classification of specific threats to validity (e.g. as internal) (**Q1**, **Q2**)

(VII). differentiate the main types of software engineering analysis (meta-property/syntactic vs. program analyses), comparing their potential applications, strengths and weaknesses. (**Q3**)

(VIII). design simple program analyses to extract and synthesise information to aid programmers with their daily work. (**Q3**)

(IX). contrast design choices concerning approximations made in static analyses, and their likely impacts on analysis results and usefulness. (**Q3**)



## Empirical Studies

### Empirical Study of JavaTest+

Imagine that you've built a tool *JavaTest+* that automatically generates test suites for Java code**.
*JavaTest+* automatically analyses a Java class and creates a corresponding JUnit test suite for that class. The Java class has to be runnable (i.e., implemented and no compile errors), in order for *JavaTest+* to run. 

You're interested in answering the research question: "Does using *JavaTest+* increase software code quality?".

You propose the following study design to answer the question. You will ask all project groups in CPSC 410, at the start of the Project1 implementation, whether they would like to use *JavaTest+* to test their Project 1 implementations. Groups that say no will be part of Set A (no *JavaTest+*). Groups that say yes will be part of Set B (using *JavaTest+*). You give a small *JavaTest+* tutorial to all the groups in Set B.

At the end of the Project1 implementation, you distribute a survey to all project groups, asking the groups to report their confidence in their software code quality (on a scale of 1-10) and the final grade they got. You find the groups in Set A have an average confidence of 7.4, and got average grades of 88%. The groups in Set B have an average confidence of 7.1, and got average grades of 79%. You conclude that *JavaTest+* does not increase software code quality.

**such tools actually exist, see e.g. [EvoSuite](https://www.evosuite.org/).


<a name="ex1"></a>
**Question 1 (2 pts)**

(In each part below, you only need to explain a single answer, even if you can see more than one issue)

(a) *Can you see any threats to Construct Validity in this study design? If so, briefly explain one.*

**Sample Answer(s): The biggest issue is that the two metrics measured don't necessarily address the research question. Neither the grade the groups got, nor the self-reported confidence of the users, actually measures (or even clearly approximates) the code quality. Note also that although we gave a tutorial to students on how to use *JavaTest+*, we didn't actually measure that they *used* it, so we may not be measuring the effect of  *JavaTest+* on anything.  You might have found other valid answers too!**

(b) *Can you see any threats to Internal Validity in this study design? If so, briefly explain one.*

**Sample Answer(s): One threat to internal validity is the potential for bias in which groups self-selected to use *JavaTest+*. This could influence the results measured: e.g. it is possible that students more confident in their testing/development abilities chose not to use JavaTest+, and that on average these students also produced higher quality code. Another theat is that we relied on self-reported grades: it is also possible that students mis-reported their grades (e.g. they weren't comfortable sharing the actual grades); this is perhaps less serious/likely than the first, but still a potential threat to validity. You might have found other valid answers too!**

(c) *Can you see any threats to External Validity in this study design? If so, briefly explain one.*

**Sample Answer(s): A clear threat to external validity is that we conducted our evaluation on CPSC 410 project groups, while our research question concerns software code quality broadly. CPSC 410 projects may not be representative of general software for a number of reasons (each is technically a threat to validity). First, they are created by students rather than professional developers. Second, they are written fully from scratch, while many professional projects are iterated upon rather than completely written from scratch: as *JavaTest+* requires runnable code, it may be less useful in software that is being written from scratch, as opposed to refactored or otherwise maintained. You might have found other valid answers too!**


(d) *Can you see any risks involved in this study design? If so, briefly explain one.*

**Sample Answer: Project groups in Set B may have spent time learning *JavaTest+* rather than working on core unit tests (or other tasks) for their projects. In particular, since *JavaTest+* requires an implemented Java class to be run on, they may have spent less time thinking about the specification of their code *in advance* in terms of unit tests, or had (perhaps unjustified) confidence that once that *JavaTest+* suite passes, their code is correct. This could perhaps have led to overall worse code quality, which may have negatively impacted their actual project performance and grade (bad if students are relying on grades for other life outcomes, or this otherwise causes personal stress).**


### Project 1 User Studies as Empirical Studies

<a name="ex2"></a>
Pick one of the user studies you performed for your project (ideally whichever provided you with the most feedback), and answer the following questions about it:

**Question 2**

(a) *What type of study (e.g. observational, controlled experiment, …) were you performing?*


**Sample Answer: These are observational studies: you go in without a specific effect you are trying to measure, and instead see what general data/feedback you can gather about the participants' behaviour on some relevant tasks.**


(b) *Briefly describe the key results you observed from your user study, and what conclusions you drew about your language design at that time.*

**Sample Answer: This part depends on your project/study/participants!**


(c) *Considering these conclusions as judgements about a new user's likely experience with using your DSL, what possible, briefly describe two different threats to validity you can see with respect to the conclusions of your user study. For each threat, identify the category (internal validity, external validity, …) to which it belongs.*

**Sample Answer(s): Note that some answers here might depend on your answer to the previous question, but two standard kind of threats to external validity would be the question of representativeness of your participants and tasks. Another issue is that the *number* of participants is very small, and so the influence of individual randomness (e.g. having one very unusual participant) is a lot larger; this should be considered as a threat to internal validity (as it is an extra factor that could skew the results measured). For the first user study, given that the users only get to experience a mock-up of your project rather than the project itself, the accuracy of their experience (via the mock up) vs. using the real project could also present a threat to construct validity (think: are you definitely measuring something relevant at all?).**

(d) *Suppose that you conducted your second user study with the same participants as your first user study. What threat to validity would this introduce, which category of threat to validity is it?*

**Sample Answer: The problem here is that you can't have the users objectively experience and evaluate your project for a second time; they have too much prior training and will be influenced by their earlier impressions and (maybe not even accurate any more) understanding of how your project works. Given the general nature of an observational study, this would be Internal Validity: we can still observe some potentially-relevant feedback, but these results may be skewed by this prior experience/discussion.**

**Note that for a different *Research Question* (e.g. "How understandable is our language for first-time users?") this kind of issue would be a threat to *Construct Validity* as it is possible we can't observe *any* relevant data in this case; the difference is in the potential effect *in combination with* the research question and experimental design.**

---
## Program Analyses

We've just started on Part II of the course, and have so-far discussed a few high-level classifications of analysis tools for Software Engineering:
1. Meta-property Analyses (which can be static or dynamic)
2. Program Analyses (which can also be static or dynamic)



**Question 3**

*(a) Suppose we wanted to analyse a Java program and answer the question: "What is the set of constant strings passed to System.out.println?". For example, given, `System.out.println("My variable:" + x)`, we want to collect the string `"My variable:"`. Which kind of analysis (meta-property or program) best describes how you would analyse this property? Would you use a static or a dynamic analysis? Briefly justify your answers.*


**Sample Answer: The intent here is that we capture the constant strings occurring as arguments to any calls to this method.This is a property of how the code is written: a Meta-Property Analysis or purely syntactic analysis. 
This property does not depend on the behaviour of the program, or require abstraction over multiple exectuions.** 

**To get the set of *all* such constant strings, it is natural to construct a static (meta-property) analysis: it's easy to find the set of relevant calls of this method statically (it's harder to determine which ones are *actually called*, but we don't require that in our problem statement). A dynamic analysis, on the other hand, would only naturally give us *some* of these relevant statements (those actually called in a particular execution).**

*(b) Briefly outline (roughly 2-4 sentences) how you think you would implement such an analysis.*


**Sample Answer: You could first write a visitor (say, `PrintConstantExtractor`) that visits all nodes in the AST, and ignores all statements other than `System.out.println` calls. On `System.out.println` statements, it collects all literal string constants occurring in arguments. Since the arguments themselves might have further structure to visit (e.g. the append of two expressions in `"My variable:" + x`), the visitor should track whether or not it is currently visiting AST structure inside an argument to a relevant (`println`) call, and only collect string constants found when this is true.**

**You could instead consider decomposing the work into one visitor that finds relevant calls, and a second `StringCollector` visitor that traverses a given AST Node, and all its children, and collects all literal string constants (similar to the `ColourCollector` from Exercise Sheet 3). You can then gather these results in the `PrintConstantExtractor` visitor.**

Suppose instead that you wanted to implement an analysis tool which tells us: which function parameters __can__ end up being involved in `System.out.println` statements? For instance, on the program:

```
public void foo(int a, int b, int c) {
    int d = a + b;
	if (d > 0) {
        System.out.println("My sum:" + String.valueOf(d));
	}
	bar(c);
}

public void bar(int x) {
    System.out.println("My call to bar");
    System.out.println(x);
    // probably do something else with x too, this is a boring function otherwise...
}

```
We would say the function parameters `a` and `b` of `foo` end up being involved in a println statement (through the printing of value `d`), and function parameters `c` of `foo` and `x` of `bar` end up being involved in a println statement (through the printing of `x` in bar).

*(c) Which kind of analysis (meta-property or program) best describes how you would analyse this property (which function parameters __can__ end up involved in a println statement)? Would you use a static or a dynamic analysis? Briefly justify your answers.*

**Answer: This would be a Program Analysis: it's concerned with the how the program runs, the semantics of different executions. In particular it is sensitive to *data-flow*: we care about which parameter values are used to create expressions that end up as arguments of `System.out.println` statements.** 

**We could design either static or dynamic analyses, here, depending on the compromises we are most comfortable with. If we want to be sure to calculate *at least* all the function parameters that ever *can* end up being involved in `System.out.println` statements, we should use a static analysis (and over-approximate the dependencies pessimistically, in the sense that we keep them if we are unsure, e.g. at join points of branching statements).**

**A dynamic analysis with `d <= 0`, for instance, might only report that c and x are involved in `System.out.println` statements); in general, it would only be able to identify the dependencies on parameters that occurred for *one particular run* of this code, but not all of those possible for all executions. On the other hand, any dependency found by such a dynamic analysis is guaranteed to be a real example (we might just be missing some).**


*(d) Briefly outline (roughly 2-4 sentences) how you would implement such an analysis.*


**Sample Answer: For a static analysis, we could track (as abstract state) a map from variable names (current variables in scope) to sets of parameter names (conceptually representing those parameters that the current value of the variable might contain some part of). The set mapped to for a variable `x` would be updated at each assignment `x = e;` of the variable in the program, taking all of the dependencies on parameters for any variables in `e` (unioned together) as the new mapping for `x`. Every time a call to `System.out.println(..)` is encountered, the analysis would take any variables in the argument `..` and log whatever the map currently stores for those variables a part of the final result. At join points in the control flow (e.g. when joining the ends of two blocks for if-then-else branching) we would take the pointwise union of these maps (each variable is mapped to a set of parameters containing those it was mapped to at the end of the analysis of *either* branch). For loops, we would iterate analysing the loop body, repeatedly taking the same pointwise union between the maps at the start and end of iterations until the result stabilises to some map (which would be used for after the map).**

**For grading, given that we asked for 2-4 sentences (:, we wouldn't require you to find a design for all of these features (if-conditions, loops, calls) but expect a discussion of some.** 

**If you're reading this after we've covered slicing, you might notice that the design above is actually very close to a simplification of our dependency analysis used for Static Program Slicing. The key differences are that we track a map to sets of *parameter names* rather than line numbers, and our rule for assignment statements only records one of the three sources of dependency used for slicing (can you see why?).**

**The design for a dynamic analysis could actually be very similar in terms of *state* tracked by the analysis (a map from variable names to sets of parameter names), but the difference would be where this state is kept and updated: for a dynamic analysis this map would need to be part of the state of the *program* and the program would need to be instrumented with extra statements to update the map every time an assignment statement is executed. Similarly, we'd instrument `System.out.println(..)` calls to log the relevant current entries in the map. There's no need for special handling for if- or while-constructs in a dynamic analysis; these simply run down which ever combinations of paths the execution naturally runs down. As usual for a dynamic analysis, this means we'll have precise information, but *only* about this *single, particular* execution of the program.**


The definition of "involved in" used in part (c) and (d) is not precise. Consider the following program:

```
class SillyClass {

   private final String correct_password = "1234isaweaksaucepassword";

	public void foo(int a, int b, String password, List<Integer> lst) {
	    int d = a + b;
		if (password == correct_password) {
	        System.out.println("My sum:" + String.valueOf(d));
		}
		if (d > 0) {
		    lst.add(d)
		}
	}

}
```

*(e) Refer to the above program. Again, we want to build an analysis that outpute which function parameters are involved in `System.out.println` statements. Give an argument as to why we might say that the parameter `password` is "involved in" the print statement in `foo`, but the parameter `lst` is not.*

**Sample Answer: Depending on the value of `password`, we might or might not execute the print statement. Thus, while `password` is not directly printed out, its value influences the presence of the print statement in the log; this is called a *control dependency*. On the other hand, `lst`'s value neither flows into the argument of a print statement, nor is it used in a condidtional statement whose execution influences a print statement.** 

*(f) Briefly, how would you have to change the analysis described in part (d) to support the broader definition of variables involved in print statements? Give 2-4 sentences.*

**Sample Answer: We could add onto our method in part (d) in a similar manner to how we handled control dependencies for static program slicing in class. Notably, in addition to the abstract state mapping variable names to parameters involved, we should also add a stack of sets of parameter names to the abstract state.
Every time we enter an if statement or loop, we push onto the stack the parameter names used in the condition of the if/loop: as we exit the corresponding block, we pop that set off the top of the stack. We modify the analysis rule for handling assignments to also include in newly-calculated dependency sets all of the parameter names showing up somewhere in our stack. Finally, when we encounter `System.out.println(..)`, in addition to what's described above, we also look through all the variables in the abstract stack, and add the parameters involved in those variables to the output (conceptually reflecting the fact that whether the `println` call is executed or not may depend on these parameters).**