---
layout: page
title: Hypothesis Testing - The t-test
permalink: /stat/hypothesis_testing2/
---
*Based on the “Statistical Consulting Cheatsheet” by Prof. Kris Sankaran*

If I had to make a bet for which test was used the most on any given day,
I'd bet it's the t-test. There are actually several variations, which are used to
interrogate different null hypothesis, but the statistic that is used to test the
null is similar across scenarios.

Because it is so common, this test is customisable:
+ "one sided vs two sided": this requires to answer the question: am I interested in a specific side of the difference (ie, I want to test $$H_0: \Delta \leq 0$$ vs  $$H_a: \Delta > 0$$) or just the existence of a difference ( $$H_0: \Delta \-0$$  vs  $$H_a: \Delta \neq 0$$) ?
+ "one sample vs two-sample":  
+ "single vs paired": this requires answering the question: are my samples paired (ie, am I observing samples from the same subjects "before" and "after" treatment)

We provide a little bit more information on these tests in the subsequent subsections.

## One sided/ two sided


*An example: Two-sided test of the mean*  
<img src="{{ site.baseurl }}images/both-sided.png"  width="400" alt="pval" style="float: right; margin-right: 5em;"/>

Is the mean flight arrival delay statistically equal to 0?


**Two-sided test of the mean: Test the null hypothesis:**

$$H_0: \mu = \mu_0 = 0$$ \\
$$H_a: \mu \ne \mu_0 = 0$$ \\
where $$\mu$$ is where $$\mu$$ is the average arrival delay.

```{r}
library(tidyverse)
library(nycflights13)
mean(flights$arr_delay, na.rm = T)
```
<img src="{{ site.baseurl }}images/lec7-1.png"  width="800" alt="pval"/>



Is this statistically significant?

```{r}
( tt = t.test(x=flights$arr_delay, mu=0, alternative="two.sided" ) )
```
<img src="{{ site.baseurl }}images/lec7-2.png"  width="800" alt="pval"/>


The function t.test returns an object containing the following components:

```{r}
names(tt)
```
<img src="{{ site.baseurl }}images/lec7-3.png"  width="800" alt="pval"/>


```{r}
# The p-value:
tt$p.value
```
<img src="{{ site.baseurl }}images/lec7-4.png"  width="800" alt="pval" />


```{r}
# The 95% confidence interval for the mean:
tt$conf.int
```
<img src="{{ site.baseurl }}images/lec7-5.png"  width="800" alt="pval" />





*An example: One-sided test of the mean*  
<img src="{{ site.baseurl }}images/left-sided.png"  width="400" alt="pval" style="float: right; margin-right: 5em;"/>

**Test the null hypothesis:**

$$H_0: \mu = \mu_0 =0$$ \\
$$H_a: \mu < \mu_0 = 0$$

In __R__: Is the average delay 5 or is it lower?


```{r}
( tt = t.test(x=flights$arr_delay, mu=5, alternative="less" ) )
```
<img src="{{ site.baseurl }}images/lec7-6.png"  width="800" alt="pval"/>


Failure to reject is not acceptance of the null hypothesis.


<br/>
<br/>


## Single vs Paired t-test

+ __The one-sample t-test is used to measure whether the mean of a sample
is far from a preconceived population mean.__
+ The two-sample t-test is used to measure whether the difference in sample
means between two groups is large enough to substantiate a rejection of
the null hypothesis that the population means are the same across the two
groups.

## What needs to be true for these t-tests to be valid?

+ Sampling needs to be __(1) independent and identically distributed (i.i.d.)__, and __(2)
in two-sample setting, the two groups need to be independent__. If this is
not the case, you can try pairing or developing richer models, see [the next page]().
+ In the two sample case, depending on the the sample sizes and population
variances within groups, you would need to use different estimates of the
standard error.
+ If the sample size is large enough, we don't need to assume normality in the
population(s) under investigation. This is because the central limit kicks
in and makes the means normal. In the small sample setting however,
you would need normality of the raw data for the t-test to be appropriate.
Otherwise, you should use a [nonparametric test]({{ "" }}{% link _pages/nonparametrictests.md %}).



## Paired t-tests
Pairing is a useful device for making the t-test applicable in a setting where
individual level variation would otherwise dominate effects coming from treatment vs. control. See Figure 1 for a toy example of this behavior.
 Instead of testing the difference in means between two groups, test for
whether the per-individual differences are centered around zero.

<p style="color:grey;font-size:13px;" align="center">
<img src="{{ site.baseurl }}/images/paired_t_test.png" alt="Paired t-test" height="250"/>
<i> Figure 1: Pairing makes it possible to see the effect of treatment in this toy
example. The points represent a value for patients (say, white blood cell count)
measured at the beginning and end of an experiment. In general, the treatment
leads to increases in counts on a per-person basis. However, the inter-individual
variation is very large { looking at the difference between before and after without the lines joining pairs, we wouldn't think there is much of a difference.
Pairing makes sure the effect of the treatment is not swamped by the varia-
tion between people, by controlling for each persons' white blood cell count at
baseline.</i>
</p>

 For example, in Darwin's Rhea Mays data, a treatment and control plant
are put in each pot. Since there might be a pot-level effect in the growth
of the plants, it's better to look at the per-pot difference (the differences
are i.i.d).

Pairing is related to a few other common statistical ideas:
+ __Difference in differences:__ In a linear model, you can model the change
from baseline
+ __Blocking:__ tests are typically more powerful when treatment and control
groups are similar to one another. For example, when testing whether two
types of soles for shoes have different degrees of wear, it's better to give
one of each type for each person in the study (randomizing left vs. right
foot) rather than randomizing across people and assigning one of the two
sole types to each person.
