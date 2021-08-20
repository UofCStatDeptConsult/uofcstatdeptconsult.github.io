---
layout: page
title: Hypothesis Testing - an Overview
permalink: /stat/hypothesis_testing/
---

Many problems in consulting can be treated as elementary testing problems.
First, let's review some of the philosophy of hypothesis testing. Testing provides a principled framework for filtering away implausible scientific claims.
 __It's a mathematical formalization of Karl Popper's philosophy of falsification: " Reject the null hypothesis if the data are not consistent with it, where
the strength of the discrepancy is formally quantified through the notion of p-value".__


### The Objective: Computing a p-value

Consequently, the main goal of hypothesis testing is to compute a p-value. A p-value can be defined as __"the probability of observing an event as extreme as what I am observing under the null"__, where the null is the default, "chance" scenario.\\
*Example:For instance, suppose that I want to assess if Soda A is better than Soda B. I could do a survey, and ask people to give a score to the soda, and average my results. Suppose there is no difference between the two: the difference between the two averages is a random variable, centered at 0. I could then compute the difference between a p-value of 0.04 means that, just by chance, only 4% of events would have seen a difference this big: I can thus reject the null.* 

While testing is fundamental to much of science, and to a lot of our work as
consultants, there are some limitations we should always keep in mind:
+ __The different types of errors:__  There are two kinds of errors we can make: (1) Accidentally falsify when
true (false positive / type I error) and (2) fail to falsify when actually false
(false negative / type II error).

+ __Defining "the null":__ In order to build a test, we need to be able to articulate the sampling behavior of the system
under the null hypothesis:
++ Often, describing the null can be complicated by particular structure
present within a problem (e.g., the need to control for values of other
variables). This motivates inference through modeling, which is reviewed
below.
++ We need to be able to quantitatively measure discrepancies from the null.
Ideally we would be able to measure these discrepancies in a way that
makes as few errors as possible { this is the motivation behind optimality
theory.
+ __Practical significance is not the same as statistical significance.__ A p-value
should never be the final goal of a statistical analysis. They should be
used to complement figures / confidence intervals / follow-up analysis
that provide a sense of the effect size.






### The Recipe




To find the right hypothesis test, we need to answer a minimum of four questions:


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

If you're looking for a [power analysis]({{ "" }}{% link _pages/power.md %}).

