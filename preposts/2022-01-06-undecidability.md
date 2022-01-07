---
layout: post
title: Undecidability
---

This post is my take on what undecidability really means, explained short and simple. In my experience, people who have taken introductory courses on the ZFC Set Theory or First Order Logic reach the wrong conclusion that within an axiomatic theory there are statements that are neither true nor false and, as far as I understand, this is not the case.

### Intuitive definitions. Gödel theorems

Let's begin at the beginning. For the purposes of this post, we will understand a **formal system** as a set of axioms and symbols and chosen logic rules to deduce further truths from the axioms. A system is said to be **consistent** if it doesn't contain contradictory statements, and it is said to be **complete** if every statement that is true within the system (anything that is true given the axioms) can be proved using only truths deduced from the axioms. 

As you probably know, Gödel's first incompleteness theorem (explained brilliantly in [this article in Quanta Magazine](https://www.quantamagazine.org/how-godels-incompleteness-theorems-work-20200714/)) states that *any consistent formal system $F$ within which a certain amount of  elementary arithmetic can be carried out is incomplete*, i.e. there are  statements of the language of $F$ which can neither be proved nor disproved in $F$. The second incompleteness theorem is an extension of the first and says that such a system cannot demonstrate its own consistency, that is, it cannot be proved within the system that the system doesn't lead to contradictory statements. How much elementary arithmetic is required is a question beyond our interest, let us just think our usual arithmetical operations can be carried out.

### An undecidable statement

If you read the article I linked to above you will find an example of an undecidable statement, because Gödel's prove is constructive in that sense: given any system he shows that provides a statement within the system that can't be proved. But probably the most famous undecidable is this the **continuum hypothesis** in the [axiomatic theory of ZFC](https://en.wikipedia.org/wiki/Zermelo%E2%80%93Fraenkel_set_theory):

>  Any infinite set which is not equipotent to $\N$ must have cardinality greater or equal to that of $\R$.

We know thanks to Gödel and Cohen that the previous statement **cannot be refuted nor proved** within ZFC. We say that the continuum hypothesis is an statement **independent** of the axioms of ZFC.

### The catch

During the previous paragraphs at no point did I say that undecidable statements are neither true nor false. In fact, **the proof of Gödel's incompleteness theorem relies on finding a true statement that cannot be proved to be true**. But we do not have to go that far to see that within mathematics there are no statements that are neither true nor false: in mathematics we follow the **law of excluded third**, i.e. the truth value of any statement that we can formulate (within a system suitable to do the math we are used to) must be either true or false.

A similar philosophical example is the existence of God. In our universe God either exists or doesn't exist. A believer would say God does exist. An atheist would say God does not exist. And an agnostic person would say that they don't know, or that it is impossible to tell. But they would not say that God is neither existent nor inexistent, mainly because that would make much sense to our logic. Therefore, given our universe, only one of the two options is the true one, even if it is beyond our ability to know or to prove it.

### The wrap up

In ZFC we have 9 axioms. These are the starting truths and every theorem we can prove was already true before we proved it. And when someone proves a theorem they are just saying: this is something that I know is true and here is how I know it. But just as planets orbited the sun before we knew it, and blood flowed through our veins before we discovered it, in ZFC once we say that there exists some entities called "sets" with these basic properties which are our everything else follows automatically (in the true sense of this word, that is, on its own, without our intervention). Any real differentiable function verified the mean value theorem before it was stated, and any real positive definite symmetric matrix had a basis of real positive eigenvalues. In the very same way, the continuum hypothesis is true or false from the very beginning, as soon as we set up the pieces of the game. And this is absolutely different to the fact that we can prove it to be true or refute it.
