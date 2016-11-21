---
layout: post
title: Syntax & Semantics
---

UW has a really cool programming languages class called [CSE505](https://courses.cs.washington.edu/courses/cse505) which has some fascinating ideas about programming languages and their construction. My notes are drawn from that course, most recently taught by Zach Tatlock or Dan Grossman.

One other thing-- forgive my formatting issues -- I'm still learning latex :)

#### Ok, so let's get started!

Let's define a formal language -- think more "logic language" than programming language like Java or C++.

First, some notation:

* := means variable assignment (i.e. x := 3)

* ::= means meta-assignment (or meta-variable assignment) (uh) (i.e. expression ::= constant)


2 key aspects of a programming language are the language's _syntax_ and _semantics_.
The syntax consists of the meaningless symbols that make up statements. The semantics take the symbols and give them meaning.

##### Syntax
We're going to define our syntax "recursively":
(If you've seen abstract syntax trees before, this might look familiar)

<p class="asts">
v (values (like constants)) ::= true | false | Z. (where Z is the set of integers)

e (expressions) ::= v
                    | not e
                    | neg e
                    | e + e   
                    | e * e.   

<br>

s (statements) ::= nop
                  | x := e  
                  | s; s  
                  | if e then s else s  
                  | while e s.

</p>

A couple of takeaways from the definitions above:
1. All these definitions are inductive. That is, they have at least one base case and several inductive cases.
2. Constants and variables are types of expressions. They don't need to be part of larger expressions.
3. The letters like e, s, c and so on are *meta-variables.* They're describing behaviors about variables.

This baby language contains most of the stuff in any old programming language! So, we're basically done, right?

###### Not quite.
Right now, our definitions allow us to say things like:  

<code> if 3 then 4 else 5 </code>  

and  

<code> while x, y </code>

Which doesn't really make sense! If you tell your computer to "do 4" then who knows what it's going to do. (Side note, we're going to come back to the second example)

So, we need to add more to this language.

As a heads up, the "s; s" line is supposed to act as stringing 2 statements together. Since we don't have any semantics defined yet, "s; s" is meaningless but I just wanted to mention what it will mean eventually.

##### Semantics

So our second example was:

<code> while x, y </code>

But what is "x" here? Ideally, in our code we'll have some kind of assignment
to x before we actually reference x, but we need something that will keep track of those assignments.

To fix that problem, let's introduce a *heap*. Our heap is just going to be a dictionary/map/set of key-value pairs. We'll let the variable be the key, and the assigned value be the value.

**Note:**  We haven't said anything about functions or anything that introduces a notion of global vs. local scoping. Once we do have functions in our language, our heap implementation will get more complicated but for now we don't have to worry about it.

Let's define a function LookUp that (wait for it) looks up the assigned values in the heap.
What kind of behaviors do we want?

1. If we've seen "x := 3" before this point (where we're looking it up) in the code,
LookUp(H, x) should return 3.

2. If we've seen "x := 3; x := 4" before this point in the code,
LookUp(H, x) should return 4 (we want to be able to overwrite previous entries).

3. If we haven't seen any assignment to x,
LookUp(H, x) should return a value, i.e. nil.

In math, we could just define LookUp to have the domain of variables that have an assigned value. Here, since we want our program to compile and run, we should define LookUp to be a valid function for all possible inputs.

Ok, so now we've got variable storage! Now, we need to define our rules -- what we can do with it.


###### (Big Step) Inference Rules

Sidenote: I'm using "Inference Rules" and "Semantics" interchangeably in this section.

First, some more notation:

H; e $\Downarrow$ c

English: "e maps to c in the heap H"

I don't know why a down arrow is used here but it's the notation used in Software Foundations and in my class so  ¯\\_(ツ)_/¯

The inductively defined heap is:
<p class="asts">

H := $\cdotp$
      | H; x $\mapsto e$

</p>

Ok, back to the inference rules.

{% include mathjaxmacros.html %}
{% raw %}{::nomarkdown}  
$
\inferrule[]
          {}
          {H; \ c \ \Downarrow \ c}

$
{:/}{% endraw %}  

{% raw %}{::nomarkdown}
$
          \inferrule[]
                    {}
                    {H; \ x \Downarrow \ LookUp(H, x)}

$
{:/}{% endraw %}  


{% raw %}{::nomarkdown}
$
          \inferrule[]
                    {H; \ e_{1} \ \Downarrow \ c_{1} \quad H; \ e_{2} \ \Downarrow\ c_{2} }
                    {H; \ e_{1} + e_{2} \ \Downarrow \ c_{1} + c_{2}}


$
{:/}{% endraw %}

And the multiplication rule is similar.

TODO: if else case, sequence case, while case.


You read this notation by if all the statements in the top hold, then the statement below the line holds (the conclusion).

The rules with no hypotheses (rules above the line) are axioms.

*Important Takeaway:* Statements update the heap while expressions look up values in the heap.

<!-- These inference rules define a proof system -->

2 Important Theorems:  
1. Progress (All heaps and expressions (not statements) evaluate to a constant)  
2. Determinacy (There is only one constant an expression + heap can evaluate to)


Let's go back to the while-case inference rule. Does it work in all cases?
What happens if the loop is infinite?

(Ans: Turns out our big step semantics don't work in that case)

Our inference rules defined here assume (require) that the programs we're talking about terminate.

This sucks! Either we restrict ourselves to non-terminating programs (which is a small subset of all possible programs), or we need to do something else.

###### Small Step Inference Rules

Small step semantics are as-advertised -- we take "small steps" while evaluating programs.






And that's it! See you next time :)
