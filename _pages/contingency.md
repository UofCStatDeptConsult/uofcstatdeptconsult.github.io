---
layout: page
title: Inference in Linear Models
permalink: /stat/contingency/
---
*Based on the “Statistical Consulting Cheatsheet” by Prof. Kris Sankaran*

<h2 id="top"> </h2>

Contingency tables are a useful technique for studying the relationship between
categorical variables. Though it's possible to study K-way contingency tables
(relating K categorical variables), we'll focus on 2  $$\times$$ 2 tables, which relate two
categorical variables with two levels each. These can be represented like in the
table (see example below). 

 | |  $$A_1$$ | $$A_2$$ | total|
 |$$B_1$$| $$n_{11}$$| $$n_{12}$$ |$$n_{1\cdot}$$|
 |$$B_1$$ |$$n_{21}$$ |$$n_{22}$$ |$$n_{2\cdot}$$|
 |total| $$n_{\cdot 1}$$| $$n_{\cdot 2 }$$| $$n_{\cdot\cdot}$$|
 
We usually imagine a sampling mechanism that leads to this
table, where the probability that a sample lands in cell $$ij$$ is $$p_{ij}$$ . Hypotheses
are then formulated in terms of these $$p_{ij}$$.



A few summary statistics of 2 $$\times$$2 tables are referred to across a variety of
tests:
+ __Difference in proportions:__ This is the difference $$p_{12} -  p_{22}$$. If the columns
represent the survival after being given a drug, and the rows correspond to
treatment vs. control, then this is the difference in probabilities someone
will survive depending on whether they were given the treatment drug or
the control / placebo.
+ __Relative Risk:__ This is the ratio $$p_{12}/p_{22}$$. This can be useful because a small
difference near zero or near one is more meaningful than a small difference
near 0.5.
+ __Odds-Ratio:__ This is $$\frac{p_{12}p_{21}}{p_{11}p_{22}}$$. It's referred to in many tests, but it is sometimes useful
to transform back to relative risk whenever a result is state in terms of
odds ratios.


The most common tests for contingency tables such as this one include:
+ The $$\chi_2$$ <a href="#chisq"> test  </a>
+ <a href="#fisher">  Fisher's Exact test </a>
+ <a href="#cochran">  Cochran-Mantel-Haenzel test </a>
+ <a href="#mcnemars"> McNemar's test </a>
<br/>




<h2 id="chisq">  1. The Chi-square test</h2>


The $$\chi_2$$  test is often used to study whether or not two categorical variables in a
contingency table are related. More formally, it assesses the plausibility of the
null hypothesis of independence:

$$H_0 : p_{ij} = p_{i\cdot}p_{\cdot j}$$

The two most common statistics used to evaluate discrepancies the Pearson
and likelihood ratio $$\chi_2$$ statistics, which measure the deviation from the expected
count under the null:
+ __Pearson__: Look at the squared absolute difference between the observed
and expected counts, using $$\sum_{ij}  \frac{(n_{ij} -\hat{\mu}_{ij})^2}{\hat{\mu}_{i}}$$
+ __Likelihood-ratio:__ Look at the logged relative difference between observed
and expected counts, using $$ 2 \sum_{ij} n_{ij} \log(\frac{n_{ij}}{\hat{\mu}_{ij}})$$.
<br/>

Under the null hypotheses, and assuming large enough sample sizes, these
are both $$\chi_2$$ distributed, with degrees of freedom determined by the number of
levels in each categorical variable.
A useful follow-up step when the null is rejected is to see which cell(s) con-
tributed to the most to the $$\chi_2$$-statistic. These are sometimes called Pearson
residuals.

<a href="#top" class="back-to-top">
  Back to Top &uarr;
</a>

<h2 id="fisher">  2. Fisher's Exact test</h2>

Fisher's Exact test is an alternative to the  $$\chi_2$$-test that is useful when the
counts within the contingency table are small and the  $$\chi_2$$-approximation is not
necessarily reliable.


 It tests the same null hypothesis of independence as the  $$\chi_2$$-test

 Under that null, and assuming a binomial sampling mechanism (condition
on the row and column totals), the count of the top-left cell can be shown
to follow a hypergeometric distribution (and this cell determines counts
in all other cells).

 This can be used to determine the probability of seeing tables with as
much or more extreme departures from independence.

 There is a generalization to $$I \times J$$ tables, based on the multiple hypergeometric distribution.

<a href="#top" class="back-to-top">
  Back to Top &uarr;
</a>


<h2 id="cochran">  3. Cochran-Mantel-Haenzel test </h2>


The Cochran-Mantel Haenzel test is a variant of the exact test that applies
when samples have been stratified across $$K$$ groups, yielding K $$2\times 2$$ separate
contingency tables.

+ The null hypothesis to which this test applies is that, in each of the K
strata, there is no association between rows and columns.
+ The test statistic consists of pooling deviations from expected counts
across all K strata, where the expected counts are defined conditional on
the margins (they are the means and variances under a hypergeometric
distribution):

$$ \frac{\sum_{k=1}^K (n_{11k}- \mathbb{E}[n_{11k}])^2}{\sum_{k=1}^K (\mathbb{V}ar[n_{11k}])}$$

<a href="#top" class="back-to-top">
  Back to Top &uarr;
</a>

<h2 id="mcnemars"> 4. McNemar's test </h2>

McNemar's test is used to test symmetry in marginal probabilities across the
diagonal in a contingency table.
 More formally, the null hypothesis asks whether the running marginal
probabilities across rows and columns are the same: $$p_{i\cdot} = p_{\cdot i}$$ for all i.

 This is the so-called ''test of marginal homogeneity," it it is often used to
see whether a treatment had any effect. For example, if the rows of the
contingency table measure whether someone is sick before the treatment
and the columns measure whether they were still sick afterwards, then if
the probability that they are sick has not changed between the timepoints,
then the treatment has had no effect (and the null hypothesis of marginal
homogeneity holds).


 The test statistic used by McNemar's test is given by

$$ \frac{n_{21} - n_{12}}{\sqrt{n_{21} + n_{12}}}$$


which measures the discrepancy in off-diagonal elements.


<a href="#top" class="back-to-top">
  Back to Top &uarr;
</a>
