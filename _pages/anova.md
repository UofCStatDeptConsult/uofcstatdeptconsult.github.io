---
layout: page
title: ANOVA
permalink: /stat/anova/
---

ANOVA is another topic that comes up a lot in consulting sessions, and can often sound daunting. The following notes serve as a short introduction to this rather vast topic --- entire books have been written on the topic.

As a brief summary, ANOVA stands for "Analysis Of Variance". As defined by Scheffe in his eponymous book, it "is a statistical technique for analyzing measurements depending on several kinds of effect operating simultaneously, to decide which kinds of effects are important and to estimate the effects".

A first example is assessing whether two group of young students taking a class with two distinct new teaching methods (eg, Flipped Classroom Format, and some other method) learn how to read better than the students using the traditional method. Like the t-test, ANOVA is generally a way of assessing whether the difference betwen two groups (or more) is significant – that is, the outcome of an ANOVA will tell you whether or not you can accept or reject the null hypothesis.

But ANOVA is a little richer than that. Following another example from Scheffe's introduction, ANOVA would be a good candidate to study an experiment on the use of two different fertilizers on four different types of tomatoes in three different localities.  In this case, "the yield might depend on the variety, the chemical treatment, and the locality. In particular, it will depend on interactions between these factors". Mathematically, ANOVA is a way of analysing models of the following type:

$$ y_i = {x}_{1i} \beta_1   +  \mathbb{x}_{2i} \beta_2 + \cdots  +\mathbb{x}_{pi} \beta_p +\epsilon_i  $$

where $$x_{ij}$$ are known observed variables, the $$\beta_j$$ are some "idealized formulation of some aspects of interest to the investigator in the phenomena underlying the observations". The usual minimal assumptions made in this model are (a) that the noise is centered ($$\mathbb{E}[\epsilon_i]=0$$) and (b) that the observations are uncorrelated:  ($$\mathbb{E}[\epsilon_i\epsilon_j]=0$$).

Note the resemblance with linear regression: 
+ if $$\{x_{ij}\}$$ are continuous-valued, this is indeed regression analysis. 
+ But __if the $$\{x_{ij}\}$$ are indicator variables (ie, $$\{0,1\}$$ depending on whether or not observation $$i$$ belongs to category $$j$$), then this is ANOVA.__
+ If there are some $$\{x_{ij}\}$$ of both kinds, this is __an analysis of covariance (ANCOVA).__


Still following the introduction by Scheffe, these types of analyses can be broken down in further categories depending on the nature of the unknown $$\beta_j$$s:
+ if they are assumed to be unknown constants --- in which case they are called parameters of the model ---, this is a __fixed-effects__ model.
+ if they are assumed to be random variables, this is a __random-effects__ model.
+ if the analysis comprises a mix of both, this is a __mixed-effects__ model.

In this short introduction, we will review the main classes of ANOVA-types of analysis:
+ <a href="#subsection-1"> One-way ANOVA </a>
+ <a href="#subsection-2">  Two-way ANOVA </a>
+ <a href="#subsection-3">  MANOVA </a>
+ <a href="#subsection-4"> ANCOVA </a>
<br/>



<h2 id="subsection-1" style="color:#EAC117;">  One-way ANOVA </h2>


The 0ne-way ANOVA regers to the comparison of the  __means__ of several propulations. We denote the means by $$\mu_1, \mu_2, \cdots, \mu_k$$.

__Main assumptions (to be verified!!):__
+  the $$y$$ should be a continuous variable.
+ The $$k$$ populations are normal with __equal variance $$\sigma^2$$.__  Equivalently: $$ y_{ij} = \mu_i + \epsilon_i $$ (with $$(i= 1, \cdots I,; \quad j = 1 \cdots J_i$$), with $$\epsilon_{ij} \sim N(0, \sigma^2)$$. 
You can test the equal variance assumption using Levene's test for homogeneity of variances. If your data fails this assumption, you will need to not only carry out a Welch ANOVA instead of a one-way ANOVA, but also use a different post hoc test.
+ These $$\epsilon_{ij}$$ are independent.
+ There should be __no significant outliers__, which would deter the quality of the mean-fitting procedure (make sure to assess whether this is the case before running the anova).

__The purpose of one-way ANOVA is to test the hypothesis: $$ H_0: \mu_1 = \mu_2 = \cdots  = \mu_k.$$__


 
### One-way ANOVA vs a t-test

From a practical perspective, ANOVA and t-tests are used in different settings.  We use ANOVA whenever we have more than 3 groups to compare. The null is:

$$ H_0: \mu_1 = \mu_2 = \cdots  = \mu_k$$

The __t-test allows you to do pairwise comparisons__ between groups. On the other hand, the ANOVA allows you to test all at once whether there exists a group that is significantly different from the others (it is called "an omnibus test").

So, suppose you want to compare $$k \geq 3 $$ groups:
1. you could do all $${k \choose 2 }$$ pairwise comparisons of your groups, but that might be (a) long and (b) tedious, because you'd have to adjust for multiple hypotheses testing.
2.   ANOVA will give you a single number (called the "F-statistic") and its associated  p-value to help you support or reject the null hypothesis displayed above. If the null is rejected (ie, it is probable that at least one of the group does not have the same mean), then you can do into the process of determining which one (we will come back to this at the end of this section)


__From a statistical perspective, this means that one of the differences between the ANOVA and the t-test lies in the quantities they are trying to compare:__
- ANOVA compares the variances between groups (hence the name) to test the null.
- The t-test does pairwaise comparisons of the means.


### How does ANOVA work?

As explained in the introductory paragraph, another way of interpreting ANOVA is just as a special case of linear regression, with an indicator variable for each group:

$$ Y_{ij} = \mu  +  \alpha_j + \epsilon_{ij}  \quad (M)$$  with $$\sum_{j} \alpha_j=0$$ (The last condition is only necessary to make sure that the model is identifiable.)


__An intuitive derivation of the F-statistics:__
The idea is to verify whether our model $$M$$ is valid or not. As in linear regression, this can be based on the comparison of the residuals:  if the model is valid (and the means are different), then the ratio of "explained" variance  to "unexplained" variance should be large. Alternatively, if the null is true, then the "explained" variance should be of the same order of magnitude as the residuals, since we'd be fitting local noise. 

Under the full model, the groups have different mean $$\bar{y}_{i}$$ and we have assumed that all groups have the same variance. Note that the residuals of the "full" model M can be written as:

$$SS_{full} = \sum_{j=1}^K \sum_{i=1}^{n_j}( y_{ij}- \bar{y}_{i})^2 $$.
Under the "null model":
$$ SS_{tot} = \sum_{j=1}^K \sum_{i=1}^{n_j} ( y_{ij}- \bar{y}_{tot})^2  =  \sum_{j=1}^K \sum_{i=1}^{n_j}( y_{ij}- \bar{y}_j +\bar{y}_j - \bar{y}_{tot})^2 $$ 

$$\implies SS_{tot} =  \sum_{j=1}^K \sum_{i=1}^{n_j}( y_{ij}- \bar{y}_j)^2+ \sum_{j=1}^K n_j (\bar{y}_j - \bar{y}_{tot})^2$$ 

As in linear regression, to test for the hypothesis $$H$$, we want to assess if the complete model (with $p_f=K$ parameters, and  N-K degrees of freedom)  fits the data __significantly__ better that the null (with $p_{0}=1$ single parameters, and N-1 degrees of freedom_( that is, the SS under the full model will always be smaller that the null model, but is that difference really significant? This is done through an F-test:

$$ F = \frac{\frac{SS_{null} - SS_{full}}{p_f-p_0}} {\frac{SS_{full}}{N-p_f}} $$, so that is our case:
$$ F = \frac{ \frac{\sum_{j=1^k} n_j (\bar{y}_j - \bar{y}_{tot})^2}{K-1}} {\frac{\sum_{j}\sum_i (y_{ij} - \bar{y}_j)^2 }{n-K}} $$

This, under the null, behaves like an $F_{K-1, N-K}$ distribution.


In R, this is simply done through the command:

```
test.1<- aov(y~group,data=mydataset)
summary(test.1)

```

__After running the ANOVA__: The next question is to understand where the differences lie. With regular ANOVA, a standard method is Tukey’s Honest Significant Difference test:

```
 TukeyHSD(test.1)
```

From the theoretical perspective,  it compares the means of every treatment to the means of every other treatment; that is, it applies simultaneously to the set of all pairwise comparisons $$  \mu _{i}-\mu _{j}j $$. It is basically a t-test on all pairwise mean comparisons that __corrects for multiple hypothesis testing__ by controlling the family-wise error rate. In other words, the statistic for each pairwise mean is:
$$ q_s = \frac{|\mu_i- \mu_j|}{SE}$$, but the distribution used to assign a p-value is no longer a t-distribution, but a studentized range distribution. The latter characterizes the distribution of $$ \frac{\bar{y}_{\max} - \bar{y}_{\min}}{S\sqrt{2/n}} $$, assuming that we take a sample of size $$n$$ from each of k populations with the same normal distribution  and suppose that $$\bar{y}_\min$$ is the smallest of these sample means and $$\bar{y}_\max$$ s the largest of these sample means, and that $$S^2$$ is the pooled sample variance from these samples.


### What to do if the variance are not equal

The recommended alternative is to run Welch-ANOVA. __This version has ALL the same assumptions (and hence requires all the same checks) as ANOVA, except for the same variances__.

Mathematically, the test is defined as follows:

$$f = \frac{\frac{1}{k-1} \sum_{j=1}^k w_j (\bar{y}_j - \bar{y}_{tot_2})^2}{1 + \frac{2(k-2)}{k^2-1} \sum_{j=1}^k \frac{1}{n_j-1} (1- w_j/w)^2 } $$

with $$w_j = \frac{n_j}{s_j^2}$$,  $$w= \sum_{j=1}^k w_j$$, and $$\bar{y}_{tot-w} = \frac{\sum_{j=1}^k w_j\bar{y}_j}{w}$$.


In other words, it's a weighted version of the traditional test. It has been shown that this statistics behave like an F statistics: $$ F(K-1, \frac{K^2-1}{3 \sum_j \frac{1}{n_j-1}(1-\frac{w_j}{w})^2})$$.

Not to worry though, you can call the test directly in R:

```
summary(test.2<-oneway.test(y~group,data=mydataset))
```
To understand where the differences lie, with Welch ANOVA, a standard method is the Games-Howell test.This is part of the function oneway in the `userfriendlyscience` package:

```
library(userfriendlyscience)
with(mydataset,oneway(x=group,y=y,posthoc="games-howell"))
```
<h2 id="subsection-2" style="color:#EAC117;">  Two (or more)-way ANOVA </h2>


This is a generalization of the previous ANOVA to a setting where we want to investigate the effect of two or more factors (rather than just the mean per group):

$$ y_{ijk} = \mu + \alpha_j + \beta_k  + \epsilon_i$$



The assumptions it makes are similar to the one-way case:
+ Homogeneity of variance (a.k.a. homoscedasticity)
+ Independence of observations
+ $$y$$ are normally distributed.

The  statistics used to assess the validity of the model is still similar to that of linear regression (except we have here categorical variables, represented by dummy variables):

$$ F = \frac{\frac{SS_{null} - SS_{full}}{p_f-p_0}} {\frac{SS_{full}}{N-p_f}} $$

Examples in R:
```{r}
two.way <- aov( y ~ effect1 + effect2, data = mydata)
two.way.with.interactions <- aov( y ~ effect1 * effect2, data = mydata)
two.way.with.blocking <- aov( y ~ effect1 * effect2 + block, data = mydata) 
### block is here a blocking variable, if the randomisation for effects 1 and 2 happened within blocks
```
The Tukey’s Honestly-Significant-Difference (TukeyHSD) test will still let us see which groups are different from one another.

```{r}
TukeyHSD(two.way)
```
<h2 id="subsection-3" style="color:#EAC117;">  MANOVA </h2>

Multivariate analysis of variance (MANOVA) is a procedure for comparing multivariate sample means --- that is, assuming that you want to compare two outcomes across multiple groups. 

It os often followed by significance tests involving individual dependent variables separately. It is based on statistics using the covariance of observations.



<h2 id="subsection-4" style="color:#EAC117;">  ANCOVA </h2>

ANOVA is used to compare and contrast the means of two or more populations. ANCOVA is used to compare one variable in two or more populations while considering other variables. It is supposed to eliminate the effect of one or more continuous extraneous variable, from the dependent variable before carrying out research.


__Key differences with ANOVA:__
+ It "accounts" for the effect of continuous variables: ANOVA entails only categorical independent variable, i.e. factor. ANCOVA encompasses a categorical and a metric independent variable. This means that ANOVA characterises between group variations, exclusively to treatment/group assignments. In contrast, ANCOVA divides between group variations to treatment and covariate.
+ While ANOVA uses both linear and non-linear model (interactions_. On the contrary, ANCOVA uses only linear model.

