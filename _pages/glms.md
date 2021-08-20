---
layout: page
title: Generalized Linear Models
permalink: /stat/glms/
---


Linear models provide the basis for most inference in multivariate settings. We
won't even begin to try to cover this topic comprehensively. There are entire
course sequences that only cover linear models. But, we'll try to highlight the
main regression-related ideas that are useful to know during consulting.

This section is focused more on the big-picture of linear regression and when
we might want to use it in consulting. We defer a discussion of inference in
linear models to the next section.


## 1. Linear regression

### 1.1. The Model

Linear regression learns a linear function between covariates and a response, and
is popular because there are well-established methods for performing inference
for common hypothesis.


+ Generally, model-fitting procedures suppose that there is a single variable
$$Y$$ of special interest. The goal of a supervised analysis is to determine the
relationship between many other variables (called covariates), $$X_1, \cdots X_p$$
and $$Y$$ . Having a model $$ Y = f(X_1, \cdots X_p)$$ can be useful for many
reasons, especially (1) improved scientific understanding (the functional
form of f is meaningful) and (2) prediction, using $$f$$ learned on one set of
data to guess the value of Y on a new collection of X's.

+  Linear models posit that the functional form $$f$$ is linear in the $$X_1, \cdots X_p$$.
This is interpreted geometrically by saying that the change in $$Y$$ that
occurs when $$X_j$$ is changed by some amount (and __all other covariates are
held fixed__) is proportional to the change in $$X_j$$ . E.g., when $$p = 1$$, this
means the scatterplot of $$X_1$$ against $$Y$$ can be well-summarized by a line.
 A little more formally, we suppose we have observed samples indexed by
$$i$$, where $$x_i \in \mathbb{R}^p$$ collects the covariates for the $$i$$th sample, and $$y_i$$ is the
associated observed response. A regression model tries to find a parameter
$$\beta \in \mathbb{R}^p$$ so that
<br>

$$  Y_i + x_i^T \beta +\epsilon_i$$

is plausible, where $$i$$ are drawn i.i.d. from $$N(0, \sigma^2) $$.

for some (usually
unknown) $$\sigma^2$$. The fitted value for $$\beta$$ after running a linear regression is
denoted $$\hat{\beta}$$ .

+ Compared to other forms of model fitting, a major advantage of of linear
models is that inference is usually relatively straightforwards { we can do
tests of significance / build confidence intervals of the strength of association across different $$X$$'s as well as comparison between models with
different sets of variables.
 The linear assumption is not well-suited to binary, count, or categorical
responses $$Y$$ , because it the model might think the response $$Y$$ belongs to
some range that's not even possible (think of extrapolating a linear in a
scatterplot when the y-axis values are all between 0 and 1). In these situations, it is necessary to apply generalized linear models (GLMs) (GLMs).
Fortunately, many of the ideas of linear models (methods for inference in
particular) have direct analogs in the GLM setting.


###  When is linear regression useful in consulting?

+  In a consulting setting, regression is useful for understanding the association between two variables, controlling for many others. This is basically
a rephrasing of point (2) above, but it's the essential interpretation of linear regression coecients, and it's this interpretation that many research
studies are going after.

+ Sometimes a client might originally come with a testing problem, but
might want help extending it to account for additional structure or covariates. In this setting, it can often be useful to propose a linear model
instead: it still allows inference, but it becomes much easier to encode
more complex structure.


### What are some common regression tricks useful in consulting?

+ __Adding interactions:__ People will often ask about adding interactions in
their regression, but usually from an intuition about the non-quantitative
meaning of the word "interaction." It's important to clarify the quanti-
tative meaning: Including an interaction term between $$X_1$$ and $$X_2$$ in a
regression of these variables onto $$Y$$ means that the slope of the relation-
ship between $$X_1$$ on $$Y$$ will be different depending on the value of $$X_2$$.
For example, i f$$X_2$$ can only take on two values (say, A and B), then the
relationship between $$X_1$$ and $$Y$$ will be linear with slope $$\beta_{1A}$$ in the case
that $$X_2$$ is A and $$\beta_{1B}$$  otherwise11. When $$X_2$$ is continuous, then there is
a continuum of slopes depending on the value of $$X_2: \beta_1 + \beta_{1\times2}X_2$$. See
Figure 5 for a visual interpretation of interactions.

+ __Introducing basis functions:__ The linearity assumption is not as restrictive
as it seems, if you can cleverly apply basis functions. Basis functions are
functions like polynomials (or splines, or wavelets, or trees...) which you
can mix together to approximate more complicated functions, see the figure.

Linear mixing can be done with linear models.
To see why this is potentially useful, suppose we want to use time as a
predictor in a model (e.g., where Y is number of species j present in the
sample), but that species population doesn't just increase or decrease lin-
early over time (instead, it's some smooth curve). Here, you can introduce
a spline basis associated with time and then use a linear regression of the
response onto these basis functions. The fitted coecients will define a
mean function relating time and the response.

+ __Derived features:__ Related to the construction of basis functions, it's often
possible to enrich a linear model by deriving new features that you imagine
might be related to $$Y$$ . The fact that you can do regression onto variables



Figure 5: In the simplest setting, an interaction between a continuous and binary
variable leads to two different slopes for the continuous variable. Here, we are
showing the scatterplot (xi1; yi) pairs observed in the data. We suppose there
is a binary variable that has also been measured, denoted xi2, and we shade
in each point according to its value for this binary variable. Apparently, the
relationship between x1 and y depends on the value of y (when in the pink group,
the slope is less. This can exactly be captured by introducing an interaction
term between x1 and x2. In cases where x2 is not binary, we would have a
continuous of slopes between x1 and y { one for each value of x2.
that aren't just the ones that were collected originally might not be ob-
vious to your client. For example, if you were trying to predict whether
someone will have a disease12 based on time series of some lab tests, you
can construct new variables corresponding to the \slope at the beginning,"
or \slope at the end" or max, or min, ... across the time series. Of course,
deciding which variables might actually be relevant for the regression will
depend on domain knowledge.
 One trick --- introducing random effects --- is so common that it gets it's
own section. Basically, it's useful whenever you have a lot of levels for a
particular categorical vector.



### 1.2. Diagnostics
How can you assess whether a linear regression model is appropriate? Many
types of diagnostics have been proposed, but a few of the most common are,
 Look for structure in residuals: According to equation 1, the amount
that the predictions ^yi = xTi
^  is o from yi (this dierence is called the
residual ri = ^yi 􀀀 yi) should be about i.i.d. N
􀀀
0; 2

. Whenever there
is a systematic pattern in these residuals, the model is misspecied in
some way. For example, if you plot the residuals over time and you nd
clumps that are all positive or negative, it means there is some unmeasured
phenomena associated with these time intervals that in
uences the average
value of the responses. In this case, you would dene new variables for
whether you are in one of these intervals, but the solution diers on a case-
by-case basis. Other types of patterns to keep an eye out for: nonconstant
spread (heteroskesdasticity), large outliers, any kind of discreteness (see
Figure 7).
 Make a qq-plot of residuals: More generally than simply nding large
outliers in the residuals, we might ask whether the residuals are plausibly
drawn from a normal distribution. qq-plots give one way of doing this
{ more often than not the tails are heavier than normal. Most people
ignore this, but it can be benecial to consider e.g. robust regression or
considering logistically (instead of normally) distributed errors.
 Calculate the leverage of dierent points: The leverage of a sample is a
measure of how much the overall t would change if you took that point
out. Points with very high leverage can be cause for concern { it's bad
if your t completely depended on one or two observations only { and
these high leverage points often turn out to be outliers. See Figure 8 for
an example of this phenomenon. If you nd a few points have very high


Figure 7: Some of the most common types of \structure" to watch out for in
residual plots are displayed here. The top left shows how residuals should appear
{ they look essentially i.i.d. N (0; 1). In the panel below, there is nonconstant
variance in the residuals, the one on the bottom has an extreme outlier. The
panel on the top-right seems to have means that are far from zero in a structured
way, while the one below has some anomalous discreteness.
leverage, you might consider throwing them out. Alternatively, you could
consider a robust regression method.
 Simulate data from the model: This is a common practice in the Bayesian
community (\posterior predictive checks"), though I'm not aware if people
do this much for ordinary regression models. The idea is simple though
{ simulate data from xTi
^  + i, where i  N
􀀀
0; ^2

and see whether the
new yi's look comparable to the original yi's. Characterizing the ways
they don't match can be useful for modifying the model to better t the
data.
Some diagnostics-related questions from past quarters,
 Evaluating regression model
4.3 Logistic regression
Logistic regression is the analog of linear regression that can be used whenever
the response Y is binary (e.g., patient got better, respondent answered \yes,"
email was spam).
22


In linear regression, the response y are directly used in a model of the
form yi = xTi
 + i. In logistic regression, we now want a model between
the xi and the unknown probabilities of the two classes when we're at xi:
p (xi) and 1 􀀀 p (xi).
 The observed value of yi corresponding to xi is modeled as being drawn
from a coin 
ip with probability p (xi).
 If we had t an ordinary linear regression model to the yi, we might get
tted responses ^yi outside of the valid range [0; 1], which in addition to
being confusing is bad modeling.
 Logistic regression instead models the log-odds transformation to the p (xi)
respectively). Concretely, it assumes the model
p (y1; : : : ; yn) =
Yn
i=1
p (xi)I(yi=1) (1 􀀀 p (xi))I(yi=0) (2)
where we are approximating
p (xi)  p (xi) :=
1
1 + exp
􀀀
􀀀xTi

 (3)
Logistic regression ts the parameter  to maximize the likelihood dened
in the model .
 An equivalent reformulation of the assumption is that log p(xi)
1􀀀p(xi)  xTi
,
i.e. the log-odds are approximately linear.


### 4.  Poisson regression
Poisson regression is a type of generalized linear model that is often applied
when the responses yi are counts (i.e., yi 2 f0; 1; 2; : : : ; g).
 As in logistic regression, one motivation for using this model is that using
ordinary logistic regression on these responses might yield predictions that
are impossible (e.g., numbers below zero, or which are not integers).
 To see where the main idea for this model comes from, recall that the
Poisson distribution with rate  draws integers with probabilities



### 5 Pseudo-Poisson and Negative Binomial regression
Pseudo-Poisson and negative binomial regression are two common strategies for
addressing overdispersion in count data.
In the pseudo-Poisson setup, a new parameter ' is introduced that sets the
relative scale of the variance in comparison to the mean: Var (y) = 'E[y]. This
is not associated with any real probability distribution, and the associated like-
lihood is called a pseudolikelihood. However, ' can be optimized along with 
from the usual Poisson regression setup to provide a maximum pseudolikelihood
estimate.
In negative binomial regression, the Poisson likelihood is abandoned alto-
gether in favor of the negative binomial likelihood. Recall that the negative bi-
nomial (like the Poisson) is a distribution on nonnegative counts f0; 1; 2; : : : ; g.
It has two parameters, p and r,
Pp;r [y = k] =

k + r 􀀀 1
k

pk (1 􀀀 p)r (9)
which can be interpreted as the number of heads that appeared before seeing r
tails, when 
ipping a coin with probability p of heads. More important than the
specic form of the distribution is the fact that it has two parameters, which
allow dierent variances even for the same mean,
