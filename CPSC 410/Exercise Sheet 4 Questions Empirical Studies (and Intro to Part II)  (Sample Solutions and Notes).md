# Exercise Sheet 4 Questions Empirical Studies (and Intro to Part II)  (Sample Solutions and Notes)

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

(b) *Can you see any threats to Internal Validity in this study design? If so, briefly explain one.*

(c) *Can you see any threats to External Validity in this study design? If so, briefly explain one.*

(d) *Can you see any risks involved in this study design? If so, briefly explain one.*


### Project 1 User Studies as Empirical Studies

<a name="ex2"></a>
Pick one of the user studies you performed for your project (ideally whichever provided you with the most feedback), and answer the following questions about it:

**Question 2**

(a) *What type of study (e.g. observational, controlled experiment, …) were you performing?*


(b) *Briefly describe the key results you observed from your user study, and what conclusions you drew about your language design at that time.*


(c) *Considering these conclusions as judgements about a new user's likely experience with using your DSL, what possible, briefly describe two different threats to validity you can see with respect to the conclusions of your user study. For each threat, identify the category (internal validity, external validity, …) to which it belongs.*

(d) *Suppose that you conducted your second user study with the same participants as your first user study. What threat to validity would this introduce, which category of threat to validity is it?*


---
## Program Analyses

We've just started on Part II of the course, and have so-far discussed a few high-level classifications of analysis tools for Software Engineering:
1. Meta-property Analyses (which can be static or dynamic)
2. Program Analyses (which can also be static or dynamic)



**Question 3**

*(a) Suppose we wanted to analyse a Java program and answer the question: "What is the set of constant strings passed to System.out.println?". For example, given, `System.out.println("My variable:" + x)`, we want to collect the string `"My variable:"`. Which kind of analysis (meta-property or program) best describes how you would analyse this property? Would you use a static or a dynamic analysis? Briefly justify your answers.*


*(b) Briefly outline (roughly 2-4 sentences) how you think you would implement such an analysis.*


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


*(d) Briefly outline (roughly 2-4 sentences) how you would implement such an analysis.*



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

*(f) Briefly, how would you have to change the analysis described in part (d) to support the broader definition of variables involved in print statements? Give 2-4 sentences.*