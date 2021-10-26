---
layout: page
title: ANOVA
permalink: /stat/anova/
---

[UNDER CONSTRUCTION]


ANOVA is another topic that comes up a lot in consulting sessions.

ANOVA stands for "Analysis Of Variance". Like the t-test, this test is generally a way of assessing whether the difference betwen two groups (or more) is significant – that is, the outcome of an ANOVA will tell you whether or not you can accept or reject the null hypothesis.

Ex: you want to assess whether two group of young students taking a class with two distinct new teaching methods learn how to read better than the students using the traditional method.

## When to use an ANOVA vs a t-test?

From a practical perspective, ANOVA and t-tests are used in different settings.  We use ANOVA whenever we have more than 3 groups to compare. The null is:

$$ H_0: \mu_1 = \mu_2 = \cdots  = \mu_k$$

The t-test allows you to do pairwise comparisons between groups. On the other hand, the ANOVA allows you to test all at once whether there exists a group that is significantly different from the others.

So, suppose you want to compare $$k \geq 3 $$ groups:
1- you could do all $${k \choose 2 }$$ pairwise comparisons of your groups, but that might be (a) long and (b) tedious, because you'd have to adjust for multiple hypotheses testing.
2-   ANOVA will give you a single number (called the "F-statistic") and its associated  p-value to help you support or reject the null hypothesis displayed above. If the null is rejected (ie, it is probable that at least one of the group does not have the same mean), then you can do into the process of determining which one (we will come back to this at the end of this section)


__From a statistical perspective, one of the differences between the ANOVA and the t-test lies in the quantities they are trying to compare:__
- ANOVA compares the variances between groups
- The t-test compares means.


## How does ANOVA work?

The basic assumption behind the ANOVA model is that the data follows the following model:

$$ Y_{ij} = \mu_j $$

for each observation $i$ from group $j$.

Another way of interpreting ANOVA is just as a special case of linear regression, with an indicator variable for each group:

$$ Y_{ij} = \mu  +  \alpha_j + \epsilon_{ij} $$  with $$\sum_{j} \alpha_j=0$$

(The last condition is only necessary to make sure that the model is identifiable.)

Translating $$H_0$$ into matrix form gives the following result:

$$ H_0 : C\beta = 0_{\mathbb{R}^{k-1}} $$


This results in an $$F$$ distribution:
$$F = \frac{ \frac{1}{r} ( SS_{SUB} - SS_{FULL} }{ \frac{1}{n-k} SS_{FULL}  }  \sim F_{k-1, N-k}$$

In other words, the test beocmes:

$$ F = \frac{\frac{1}{k-1} SS_{TOTAL} - SS_{within}}{SS_{within+} $$
