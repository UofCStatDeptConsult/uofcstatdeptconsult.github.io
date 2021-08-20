---
layout: page
title: Nonparametric tests
permalink: /stat/nonparam/
---

There are reasons we may prefer a nonparametric test to a parametric one:
+ __The assumptions for a particular test statistic to be valid do not hold.__
This was the motivation for switching from the $$\chi^2$$-test to Fisher's exact
test when the counts in the contingency table are small. In this case, we will propose <a href="#subsection-1"> other test statistics and distributions </a>, that typically make fewer/ more robust assumptions. These provide several alternatives to the different $$t$$-tests when the sample size is so small that the central limit theorem cannot be expected to hold for the sample mean.
+ __The test statistic of interest has no known distribution.__ It is sometimes the
case that a test statistic is proposed for a problem, but that its distribution
under the null hypothesis has no analytical form.   For this second point, we will consider some general devices for simulating the distribution of a test statistic under its null distribution --- these methods typically substitute complicated mathematics with <a href="#subsection-2">intensive computation</a>.

<h2 id="subsection-1"> Section 1: Non-parametric, robust tests</h2>

These tests are typically rank-based: instead of the magnitude of the effect, we look at the rank of the observations/. Rank-based tests are often used as a substitute for $$t$$-tests in small-sample set-
tings. The most common are the __Mann-whitney (substitute for 2-sample t-test)__,
sign __(substitute for paired t-test)__, and __signed-rank tests__ (also a substitute for
paired t-test, but using more information than the sign test). Some details are
given below.
+ __Mann-Whitney test__:
 The null hypothesis is
$$H_0 : \mathbb{P} (X > Y ) = \mathbb{P} (Y > X) ;$$
which is a strictly stronger condition than equality in the means of $$X$$
and $$Y$$ . For this reason, care needs to be taken to interpret a rejection
in this and other rank-based tests : __the rejection could have been due
to any difference in the distributions (for example, in the variances),
and not just a difference in the means.__\\
 The procedure does the following: 
1.  Combine the two groups of data
into one pooled set, 
2.  Rank the elements in this pooled set, and 
 3.  See whether the ranks in one group are systematically larger than
another. If there is such a discrepancy, reject the null hypothesis.

+ __Sign test__:  This is __an alternative to the paired t-test__, when data are paired between the two groups (think of a change-from-baseline experiment). The null hypothesis is that the differences between paired measurements is symmetrically distributed around 0. The procedure  first computes the sign of the difference between all $$n$$ 
pairs. It then computes the number of times a positive sign occurs
and compares it with the a $$ Binomial(n, \frac{1}{2}) $$, which is how we'd expect this
quantity to be distributed under the null hypothesis.

Since this test only requires a measurement of the sign of the difference between pairs, it can be applied in settings where there is no
numerical data (for example, data in a survey might consist of "likes"
and "dislikes" before and after a treatment).

+ __Signed-rank test__:
 In the case that it is possible to measure the size of the difference
between pairs (not just their sign), it is often possible to improve the
power of the sign test, using the signed-rank test.\\
Instead of simply calculating the sign of the difference between pairs,
we compute provide a measure of the size of the difference between
pairs. \\
For example, in numerical data, we could just use $$ \big| x_{i \text{after}} = x_{i \text{before}}  \big | $$.\\
 At this point, order the difference scores from largest to smallest, and
see whether one group is systematically overrepresented among the
larger scores  (the threshold is typically tabulated, or a more generally applicable normal approximation
can be applied). In this case, reject the null hypothesis.

<h2 id="subsection-2"> Section 2: Computational methods </h2>

### 2.1. Permutation tests


Permutation tests are a kind of computationally intensive test that can be used
quite generally. The typical setting in which it applies has two groups between
which we believe there is some difference. The way we measure this difference
might be more complicated than a simple difference in means, so no closed-form
distribution under the null may be available.

The basic idea of the permutation test is that we can __randomly create artificial groups in the data, so there will be no systematic differences between the
groups. This replicates a "null distribution" of your dataset. Then, computing the statistic on these many artiffcial sets of data gives
us an approximation to the null distribution of that statistic. Comparing the
value of that statistic on the observed data with this approximate null can give
us a p-value__. See figure for a representation of this idea.

<img src="{{ site.baseurl }}/images/nullpermut.png" alt="drawing1" width="400"/>

*<font size="2">Figure: A representation of a two-sample difference in means permutation
test. The values along the x-axis represent the measured data, and the colors
represent two groups. The two row gives the values in the observed data, while
each following row represents a permutation in the group labels of that same
data. The crosses are the averages within those groups. Here, it looks like in
the real data the blue group has a larger mean than the pink group. This is
reflected in the fact that the difference in means in this observed data is larger
here than in the permuted data. The proportion of times that the permuted
data have a larger difference in means is used as the p-value.</font>*

+ More formally, __the null hypothesis tested by permutation tests is that the
group labels are exchangeable in the formal statistical sense (the distribution is invariant under permutations).__
+ For the same reason that caution needs to be exercised when interpreting rejections in the Mann-Whitney test, it's important to be aware that
a permutation test can reject the null for reasons other than a simple
difference in means.
+ The statistic you use in a permutation test can be whatever you want,
and the test will be valid. Of course, the power of the test will depend
crucially on whether the statistic is tailored to the type of departures from
the null which actually exist.
+ __The permutation p-value of a test statistic is obtained by making a histogram of the statistic under all the different relabelings, placing the observed value of the statistic on that histogram, and looking at the fraction
of the histogram which is more extreme than the value calculated from
the real data.__
+ A famous application of this method is to Darwin's Zea Mays data. In
this experiment, Darwin planted Zea Mays that had been treated in two
different ways (self vs. cross-fertilized). In each pot, he planted two of
each plant, and he made sure to put one of each type in each pot, to
control for potential pot-level effects. He then looked to see how high the
plants grew. The test statistic was then the standardized difference in
means, and this was computed many times after randomly relabeling the plants as self and cross-fertilized. The actual difference in heights for the
groups was then compared to this histogram, and the difference was found
to be larger than those in the approximate null, so the null hypothesis was
rejected.

###  2.2.  Bootstrap tests

While the bootstrap is typically used to construct confidence intervals (see Sec-
tion Inference in Linear Models), it is also possible to use the bootstrap principle to perform hypothesis
test. Like permutation tests, it can be applied in a range of situations where
classical testing may not be appropriate.

 The main idea is to simulate data under the null and calculate the test
statistic on these null data sets. The p-value can then be calculated by making a comparison of the real data to this approximate null distribution,
as in permutation tests.


As in permutation tests, this procedure will always be valid, but will
only be powerful if the test-statistic is attuned to the actual structure of
departures from the null.

 The __trickiest part of this approach is typically describing an appropriate
scheme for sampling under the null__. This means we need to estimate $$ \hat{F}_0$$
among a class of CDFs $$F_0$$ consistent with the null hypothesis.
 For example, in a two-sample difference of means testing situation, to
sample from the null, we center each group by subtracting away the mean,
so that $$H_0$$ actually holds, and then we simulate new data by sampling
with replacement from this pooled, centered histogram.


<img src="{{ site.baseurl }}/images/bootstrap.png" alt="drawing" width="400"/>

*<font size="2">Figure: To compute a p-value for a permutation test, refer to the permutation
null distribution. Here, the histogram provides the value of the test statistic
under many permutations of the group labeling { this approximates how the
test statistic is distributed under the null hypothesis. The value of the test
statistic in the observed data is the vertical bar. The fraction of the area of this
histogram that has a more extreme value of this statistic is the p-value, and
it exactly corresponds to the usual interpretation of p-values as the probability
under the null that I observe a test statistic that is as or more extreme.
.</font>*



### 2.3. Kolmogorov Smirnov


The Kolmogorov-Smirnov (KS) test is a test for either (1) comparing two groups
of real-valued measurements or (2) evaluating the goodness-of-fit of a collection
of real-valued data to a prespecified reference distribution.

+ In its two-sample variant, the empirical CDFs (ECFs) for each group
are calculated. The discrepancy is measured by the largest absolute gap
between the two ECDFs. This is visually represented in the next figure.



<img src="{{ site.baseurl }}/images/ks.png" alt="drawing" width="400"/>


*<font size="2">Figure: The motivating idea of the two-sample and goodness-of-fit variants of
the KS test. In the 2-sample test variant, the two colors represent two different
empirical CDFs. The largest vertical gap between these CDFs is labeled by the
black bar, and this defines the KS statistic. Under the null that the two groups
have the same CDF, this statistic has a known distribution, which is used in
the test. In the goodness-of-fit variant, the pink line now represents the true
CDF for the reference population. This test sees whether the observed empirical
CDF (blue line) is consistent with this reference CDF, again by measuring the
largest gap between the pair.
.</font>*

+ The distribution of this gap under the null hypothesis that the two groups
have the same ECDF was calculated using an asymptotic approximation,
and this is used to provide p-values.

+ In the goodness-of-fit variant, all that changes is that one of the ECDFs
is replaced with the known CDF for the reference distribution.
