---
layout: page
title: Hypothesis Testing - The t-test
permalink: /stat/hypothesis_testing2/
---


If I had to make a bet for which test was used the most on any given day,
I'd bet it's the t-test. There are actually several variations, which are used to
interrogate different null hypothesis, but the statistic that is used to test the
null is similar across scenarios.

+ __The one-sample t-test is used to measure whether the mean of a sample
is far from a preconceived population mean.__
+ The two-sample t-test is used to measure whether the difference in sample
means between two groups is large enough to substantiate a rejection of
the null hypothesis that the population means are the same across the two
groups.

## What needs to be true for these t-tests to be valid?

 Sampling needs to be independent and identically distributed (i.i.d.), and
in two-sample setting, the two groups need to be independent. If this is
not the case, you can try pairing or developing richer models, see [the next page]().
