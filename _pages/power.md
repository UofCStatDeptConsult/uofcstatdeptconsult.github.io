---
layout: page
title: Hypothesis Testing - Power Analysis
permalink: /stat/power/
---
*Based on the “Statistical Consulting Cheatsheet” by Prof. Kris Sankaran*

Before performing an experiment, __it is important to get a rough sense of how
many samples will need to be collected in order for the follow-up analysis to have
a chance at detecting phenomena of interest__. This general exercise is called a
power-analysis, and it often comes up in consulting sessions because many grant
agencies will require a power analysis be conducted before agreeing to provide
funding.


### Analytical Power Analysis


Traditionally, power analysis have been done by deciding in advance upon the
type of statistical test to apply to the collected data and then using basic statistical theory to work out exactly the number of samples required to reject the
null when the signal has some assumed strength.
 For example, if the true data distribution is assumed to be  $$ N(\mu, \sigma^2)$$ and we are testing against the null   $$ N(0, \sigma^2) $$
using a one-sample t-test,
then the fact that $$ \bar{X} = \frac{\sum_{i=1}^n X_i }{N} \sim N(\mu, \frac{\sigma^2}{N}) $$
can be used to analytically calculate
the probability that the observed mean will be above the t-test rejection
threshold. __The size of the signal $$ \mu$$ is assumed known (smaller signals require larger
sample sizes to detect)__. Of course this is the quantity of interest in the
study, and if it were known, there would be no point in doing the study:
+ The idea though is to get a __rough estimate of the number of samples required
for a few different signal strengths.__
+ Sometimes, __a pilot study__ has been conducted previously, which can give __an approximate
range__ for the signal strength to expect.
+ We can also use similar studies in the literature to determine what a "reasonable effect size"
could be.

 There are [many power calculators available](http://powerandsamplesize.com/), these can be useful to share
/ walk through with clients.


### Computational Power Analysis

When more complex tests or designs are used, it is typically impossible to work
out an analytical form for the sample size as a function of signal strength – we can't invert the t-test/z-test formula to assess the appropriate number of samples. 

In
this situation, it is common to set up a simulation experiment to approximate
this function. 
+ 1: The __client needs to specify a simulation mechanism under which the data
can be plausibly generated__, along with a description of which knobs change
the signal strengths in what ways.
+ 2: The client needs to __specify the actual analysis__ that will be applied to these
data to declare significance.
+ 3:  From here, many simulated datasets are generated for every configuration
of signal strengths along with a grid of sample sizes. The number of times
the signal was correctly detected is recorded and is used to estimate of the
power under each configuration of signal strength and sample size.

