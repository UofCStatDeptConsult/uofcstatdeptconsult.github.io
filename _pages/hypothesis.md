---
layout: page
title: Hypothesis Testing - an Overview
permalink: /stat/hypothesis_testing/
---

Many problems in consulting can be treated as elementary testing problems.
First, let's review some of the philosophy of hypothesis testing. Testing provides a principled framework for filtering away implausible scientific claims.
 __It's a mathematical formalization of Karl Popper's philosophy of falsification. The underlying principle is simple: " Reject the null hypothesis if the data are not consistent with it, where
the strength of the discrepancy is formally quantified through the notion of p-value".__


### The Objective: Measuring the strength of discrepancy by computing a p-value

Consequently, one of the main goals of hypothesis testing is to compute a p-value. A p-value can be defined as __"the probability of observing an event as extreme as what I am observing under the null"__, where the null is the default, "chance" scenario.\\
*Example: For instance, suppose that I want to assess if Soda A is better than Soda B. I could do a survey, and ask people to give a score to each of the soda, and average my results. Suppose there is no difference between the two: the difference between the two averages is a random variable, centered at 0. Conversely, if there is a true underlying difference (let's call it $$ \delta $$),  then  the difference between my two averages: $$ \Delta = \bar{X}_a -\bar{X}_b $$  is also a random variable, but centered at $$ \delta $$. The entire point of hypothesis testing becomes to quantify how extreme this difference $$ \Delta $$ has to be to "reject the null" --- i.e, to say that it is unlikely for $$ \Delta $$ to be this extreme if the null ("there is no difference in sodas") is true. This is the concept at the core of  p-values: a p-value of 0.04 means that, just by chance, only 4% of events would have seen a difference this big. If I am willing to accept that 4% is too small (statisticians usually abide by the convention that anything less than 5% chance is unlikely to happen by chance alone), I can reject the null.* 

### The Recipe

Of course, to determine what this p-value is, there are thwo essential steps:
+ __Step 1: Defining a test statistics__ that is, what I will be measuring (eg, the average score). This has to be tailored to the problem that I am really interested in.
+ __Step 2: Defining what the null looks and behaves like__: this is what will allow me to measure whether or not what I am observing in my own dataset is extreme or not.

While testing is fundamental to much of science, and to a lot of our work as
consultants, there are some limitations we should always keep in mind:
+ __The different types of errors:__  There are two kinds of errors we can make: (1) Accidentally falsify when
true (false positive / type I error) and (2) fail to falsify when actually false
(false negative / type II error). Different types of tests will allow to control for these errors, and finding the right hypothesis test thus becomes a matter of finding the test that is "the most sensitive" to the data that you are measuring.

+ __The problem of defining "the null":__ In order to build a test, we need to be able to articulate the sampling behavior of the system
under the null hypothesis:
   + Often, describing the null can be complicated by particular structure
present within a problem (e.g., the need to control for values of other
variables). This motivates inference through modeling, which is reviewed
in the inference in linear model section of this website.
   +  We need to be able to quantitatively measure discrepancies from the null.
Ideally we would be able to measure these discrepancies in a way that
makes as few errors as possible. This is in fact the motivation behind optimality
theory: how can I run my experiments in a way that allows the "purest" measures for the question that I would like to answer?
+ __Practical significance is not the same as statistical significance.__ A p-value
should never be the final goal of a statistical analysis. They should be
used to complement figures / confidence intervals / follow-up analysis
that provide a sense of the effect size.






### The Ingredients


To find the right hypothesis test, we need to select the right "ingredients". That requires to answer a minimum of four questions:


+ __Question 1: What type of data do I have?__
    + Categorical Data [Contingency Tables]()
    + Continuous Data: [t-tests]({{ "" }}{% link _pages/hypothesis2.md %}), [Anova]({{ "" }}{% link _pages/hypothesis2.md %})


+ __Question 2:  Can I assume my data points are independent?__
  + Are there some paired variables?
  + Is my data stratified into clusters?
  + Are there some potential batch effects? 

+ __Question 3:  Am I testing a single, or multiple hypotheses at the same time?__
  + If there are multiple hypotheses, then you will need to correct for [multiple hypothesis testing]({{ "" }}{% link _pages/mhs.md %}).

+ __Question 4:  How many datapoints do I have?__
  + This will help you determine whether or not you will need a [non-parametric model]().

__Prospective Measurements__ :Finally, if you haven't done your measurements yet and you're looking to assess how many samples you would need to answer your question, do look at our page on  [power analysis]({{ "" }}{% link _pages/power.md %}).

