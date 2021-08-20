---
layout: page
title: Unsupervised Learning
permalink: /stat/unsupervised/
---

UNDER CONSTRUCTION

Essentially, the goal of unsupervised methods is data compression, either to
facilitate human interpretation or to improve the quality of downstream analysis.
The compressed representation should still contain the signal in present the
data, but the noise should be ltered away. Unlike regression and classication
(which are supervised), no single variable is of central interest. From another
perspective, unsupervised methods can be thought of as inferring latent discrete
(clustering) or continuous (factor analysis) \labels" { if they were available, the
problem would reduce to a supervised one.

## Clustering
Clustering methods group similar samples with one another. The usual products
of a clustering analysis are,
+ Cluster assignments: Which samples belong to which clusters?
+  Cluster characterization: How can we interpret the resulting clusters?
This can be achieved by looking at cluster centroids (the within-cluster
averages) or medoids (a sample from within the cluster that is represen-
tative of that cluster in some way).
Clustering techniques can roughly be divided up into those that are distance-
based and those that are probabilistic. Common distance-based methods in-
clude:
+  K-means: This builds K-clusters in an iterative way, attempting to mini-
mize the sum of within-cluster distances (so that the clusters are compact).
Since all directions are treated symmetrically, the resulting clusters tend
to be spherical. Similarly, since clusters are treated symmetrically, they
tend to all be approximately the same size.
+   Hierarchical clustering: This builds clusters hierarchically. Specifically, it
begins with each sample as its own singleton cluster. The pair that is
found to be most similar is then merged together, and the procedure is
continued until all clusters are joined into one large cluster. The nearness
of a pair of cluster has to be dened somehow { common choices include
\complete-link" merging (say that the distance between two clusters is the
distance between the furthest away pair across clusters) or \average-link"
(say the distance is the average pairwise distance between clusters). An
advantage of this approach is that it provides a full tree relating samples
{ the more branch length a pair of samples shares, the more similar they
are. In particular, this approach avoids the problem of choosing K, though
clusters can be chosen by cutting the tree at some horizontal level. The
main drawback of this approach is that it does not it does not scale as
well as xed-K methods.


+ Spectral clustering: This

+  Kernel k-means:
In these methods, decisions need to be made about (1) preprocessing and (2)
the distance to use. Preprocessing will vary on a case-by-case basis. Generally,
the goal of preprocessing is to ensure that the features included in clustering are
meaningful, which means we might drop, transform (e.g., standardization), or
derive features from (e.g., from counts to tf-idf scores) the original raw features.
As far as distances are concerned, some useful ones to know are
  +  Euclidean distance: This is the standard length of a line between two
points in a euclidean space: d (xi; xj) =
Pp
k=1 (xik 􀀀 xjk)2. It is a natural
default, especially considering it's connection to squared-error loss / the
exponent in the Gaussian. Note that, due to this connection to squared-
error, it can be sensitive to outliers. For reference, the distance can be
written as kxi 􀀀 xjk2.
 Weighted euclidean distance: If we want the coordinates to have dier-
ent amounts of in
uence in the resulting clustering, we can reweight the
coordinates in the euclidean distance according to
d (xi; xj) =
Xp
k=1
wk (xik 􀀀 xjk)2 ; (32)
where wk is the weight associated with coordinate k. This kind of reweight-
ing can be useful when the relative weights for groups of columns should
be made comparable to one another.
+   Mahalanobis distance: This is dened as
d (xi; xj) = (xi 􀀀 xj)T 􀀀1 (xi 􀀀 xj) (33)
Note that ordinary and weighted euclidean distances are special cases, with
􀀀1 = Ip and diag w, respectively. Geometrically, this distance reweights
directions according to the contours dened by the ellipse with eigenvec-
tors of . Equivalently, this is the distance that emerges when using an
ordinary euclidean distance on the \whitened" data set, which premulti-
plies X according to 􀀀1
2 .
+   Mannhattan / `1-distance: When applied to binary data, this counts the
number of mismatches between coordinates.
 Cosine distance: This measures the size of the angle between a pair of
vectors. This can be especially useful when the length of the vectors is
irrelevant. For example, if a document is pasted to itself, the word count
has doubled, but the relative occurrences of words has remained the same.
Formally, this distance is dened as,
d (xi; xj) = 1 􀀀
xTi
xj
kxik2kxjk2
= 1 􀀀 cos
􀀀
xi;xj

; (34)
where xi;xj represents the angle between xi and xj .

 Jaccard: This is a distance between pairs of length p binary sequences xi
and xj dened as
d (xi; xj) = 1 􀀀
Pp
k=1 I (xik = xjk = 1)
p
; (35)
or one minus the fraction of coordinates that are 0/1. The motivation for
this distance is that coordinates that are both 0 should not contribute to
similarity between sequences, especially when they may be dominated by
0s. We apply this distance to the binarized version of the species counts.
 Dynamic time warping distance: A dynamic time warping distance is
useful for time series that might not be quite aligned. The idea is to
measure the distance between the time series after attempting to align
them

Related to these probabilistic mixture models are mixed-membership models.
They inhabit the space between discrete clustering continuous latent variable
model methods { each data point is thought to be a mixture of a few underlying
\types." These our outside of the scope of this cheatsheet, but see .... for details.
 Medwhat learning algorithm
 Unsupervised learning for classifying materials
 Clustering survey data
 CS 106A survey


## Low-dimensional representations
### Principle Components Analysis
 Relationship power scale
 Survey data underlying variables
 Analyzing survey data
 Teacher data and logistic regression
 Unsupervised learning for classifying materials
###  Factor analysis
 Culturally relevant depression scale
### Distance based methods


##  Networks
 Molecular & cellular physiology
 Variational bounds on network structuresrst (using dynamic programming).
 Mixtures of distances: Since a convex combination of distances is still a
distance, new ones can be tailored to specic problems accordingly. For
example, on data with mixed types, a distance can be dened to be a
combination of a euclidean distance on the continuous types and a jaccard
distance on the binary types.
In contrast, probabilistic clustering techniques assume latent cluster indica-
tor zi for each sample and dene a likelihood model (which must itself be t)
assuming these indicators are known. Inference of these unknown zi's provides
the sample assignments, while the parameters tted in the likelihood model can
be used to characterize the clusters. Some of the most common probabilistic
clustering models are,
 Gaussian mixture model: The generative model here supposes that there
are K means 1; : : : ; K. Each sample i is assigned to one of K categories
(call it zi = k), and then the observed sample is drawn from a gaussian
with mean k and covariance  (which doesn't depend on the class assign-
ment). This model can be considered the probabilistic analog of K-means
(K means actually emerges as the small-variance limit).
 Multinomial mixture model: This is identical to the gaussian mixture
model, except the likelihood associated with the kth group is Mult (ni; pk),
where ni is the count in the ith row. This is useful for clustering count
matrices.
 Latent Class Analysis:
 Hidden Markov Model: This is a temporal version of (any of) the earlier
mixture models.The idea is that the underlying states zi transition to one
another according to a markov model, and the observed data are some
emission (gaussian or multinomial, say) from the underlying states. The
point is that a sample might be assigned to a centroid dierent from
the closest one, if it would allow the state sequence to be one with high
probability under the markov model.
