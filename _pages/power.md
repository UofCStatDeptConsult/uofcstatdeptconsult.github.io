---
layout: page
title: Hypothesis Testing - Power Analysis
permalink: /stat/power/
---

Before performing an experiment, it is important to get a rough sense of how
many samples will need to be collected in order for the follow-up analysis to have
a chance at detecting phenomena of interest. This general exercise is called a
power-analysis, and it often comes up in consulting sessions because many grant
agencies will require a power analysis be conducted before agreeing to provide
funding.


### Analytical Power Analysis
Traditionally, power analysis have been done by deciding in advance upon the
type of statistical test to apply to the collected data and then using basic sta-
tistical theory to work out exactly the number of samples required to reject the
null when the signal has some assumed strength.
 For example, if the true data distribution is assumed to be  $$ N(\mu, \sigma^2)$$ and we are testing against the null   $$ N(0, \sigma^2) $$
using a one-sample t-test,
then the fact that $$ \bar{X} = \frac{\sum_{i=1}^n X_i }{N} \sim N(\mu, \frac{\sigma^2}{N}) $$
can be used to analytically calculate
the probability that the observed mean will be above the t-test rejection
threshold.
 The size of the signal is assumed known (smaller signals require larger
sample sizes to detect). Of course this is the quantity of interest in the
study { if it were known, there would be no point in doing the study. The
idea though is to get a rough estimate of the number of samples required
for a few dierent signal strengths8.
 There are many power calculators available, these can be useful to share
/ walk through with clients.


### Computational Power Analysis
When more complex tests or designs are used, it is typically impossible to work
out an analytical form for the sample size as a function of signal strength. In
this situation, it is common to set up a simulation experiment to approximate
this function.
 The client needs to specify a simulation mechanism under which the data
can be plausibly generated, along with a description of which knobs change
the signal strengths in what ways.
 The client needs to specify the actual analysis that will be applied to these
data to declare signicance.
 From here, many simulated datasets are generated for every conguration
of signal strengths along with a grid of sample sizes. The number of times
the signal was correctly detected is recorded and is used to estimate of the
power under each conguration of signal strength and sample size.
 Appropriate sample size calculations
 Land use and forest cover
 t-test vs. Mann-Whitney