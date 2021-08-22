---
layout: page
title: Unsupervised Learning
permalink: /stat/unsupervised/
---
*Based on the “Statistical Consulting Cheatsheet” by Prof. Kris Sankaran*


[UNDER CONSTRUCTION]

Essentially, the goal of unsupervised methods is data compression, either to
facilitate human interpretation or to improve the quality of downstream analysis.
The compressed representation should still contain the signal in present the
data, but the noise should be filtered away. __Unlike regression and classification
(which are supervised), no single variable is of central interest.__ From another
perspective, unsupervised methods can be thought of as inferring latent discrete
(clustering) or continuous (factor analysis) "labels"  --- __if they were available, the
problem would reduce to a supervised one.__

Unsupervised learning encompasses a variety of methods. We go briefly over the following essential directions:
+ <a href="#subsection-1"> Clustering </a>
+ <a href="#subsection-2">  Low-dimensional Representations </a>
+ <a href="#subsection-3">  Networks </a>
+ <a href="#subsection-4"> Mixture Modelling </a>
<br/>


<h2 id="subsection-1">  1. Clustering</h2>

Clustering methods group similar samples with one another. The usual products
of a clustering analysis are:
+ __Cluster assignments:__ Which samples belong to which clusters?

+ __Cluster characterization:__ How can we interpret the resulting clusters?This can be achieved by looking at cluster centroids (the within-cluster
averages) or medoids (a sample from within the cluster that is representative of that cluster in some way).


Clustering techniques can roughly be divided up into <a href="#subsubsection-11"> __those that are distance-
based__</a> and <a href="#subsubsection-12">  __those that are probabilistic__</a>. 

<h3 id="subsubsection-11"> 1.1 Distance-based clustering</h3>

Distance-based clustering has two main ingredients:  (1) a clustering algorithm, (2) a distance/ choice of similarities.  Depending on the application, and the problem at hand, these are thus the main "customization" options. In this paragraph, we provide a few classical options. Note however, that you can be creative, adapt and tailor your choices (in particular the choice of the distance) to the problem at hand: the trick usually consists in defining "the right metric", "the right measure of similarity", etc.


__Step 1: Selecting an Algorithm.__ Common distance-based methods include:

<img src="{{ site.baseurl }}/images/kmeans.png" alt="drawing" width="250" style="float: right; margin-left: 1em;"/>

+ __K-means:__ This builds K-clusters in an iterative way, attempting to minimize the sum of within-cluster distances (so that the clusters are compact). Since all directions are treated symmetrically, the resulting clusters tend
to be spherical. Similarly, since clusters are treated symmetrically, they tend to all be approximately the same size.
<br/>


+ __Hierarchical clustering:__ This builds clusters hierarchically. Specifically, it
begins with each sample as its own singleton cluster. The pair that is
found to be most similar is then merged together, and the procedure is
continued until all clusters are joined into one large cluster. The nearness
of a pair of cluster has to be defined somehow: common choices include
__"complete-link" merging (say that the distance between two clusters is the
distance between the furthest away pair across clusters)__ or __"average-link"
(say the distance is the average pairwise distance between clusters)__. \\
<img src="{{ site.baseurl }}/images/hierarchical.png" alt="drawing" width="350" style="float: right; margin-left: 1em;"/>
An advantage of this approach is that it provides a full tree relating samples. The more branch length a pair of samples shares, the more similar they
are. In particular, this approach avoids the problem of choosing K, though
clusters can be chosen by cutting the tree at some horizontal level. The
main drawback of this approach is that it does not it does not scale as
well as fixed-K methods.

<img src="{{ site.baseurl }}/images/spectral2.png" alt="drawing" width="250" style="float: right; margin-left: 1em;"/>
+ __Spectral clustering:__ This method is "spectral", because it relies on the eigenvector decomposition of the Laplacian of the data: $$L= D - A$$ (where $$A$$ is a similarity matrix between datapoints, and $$D$$ the associated degree) to do a dimensionality reduction of the data before clustering.\\
The main  advantage of this method is that it is non-linear. 

+  __Kernel k-means:__ This is a kernelized version of k-means, ie., at a high level, k-means is applied to "high-dimensional embeddings" of the original dataset. This allows to use a richer class of distances, as well as to have non-lnear embeddings.\\
In these methods, decisions need to be made about (1) preprocessing and (2)
the distance to use. Preprocessing will vary on a case-by-case basis. Generally,
the goal of preprocessing is to ensure that the features included in clustering are
meaningful, which means we might drop, transform (e.g., standardization), or
derive features from (e.g., from counts to tf-idf scores) the original raw features.


__Step 2: Selecting a distance__ : As far as distances are concerned, some useful ones to know are:
  + __Euclidean distance:__ This is the standard length of a line between two
points in a euclidean space: $$d(x_i, x_j) = || x_i - x_j||^2 = \sum_{k=1}^p ( x_{ik} - x_{jk})^2 $$. It is a natural
default, especially considering it's connection to squared-error loss / the
exponent in the Gaussian. Note that, due to this connection to squared-
error, it can be sensitive to outliers.
+ __Weighted euclidean distance:__ If we want the coordinates to have different amounts of influence in the resulting clustering, we can reweight the coordinates in the euclidean distance according to
$$d(x_i, x_j) = \sum_{k=1}^p  w_k ( x_{ik} - x_{jk})^2 $$
where $$w_k$$ is the weight associated with coordinate $$k$$. This kind of reweighting can be useful when the relative weights for groups of columns should
be made comparable to one another.
+ __Mahalanobis distance:__ This is defined as $$d(x_i, x_j) = (x_i-x_j)^T \Sigma^{-1} (x_i -x_j)$$.\\
Note that ordinary and weighted euclidean distances are special cases, with $$\Sigma = I_p$$ and $$\text{diag}(w)$$, respectively. Geometrically, this distance reweights
directions according to the contours defined by the ellipse with eigenvectors of $$\Sigma$$ . \\
Equivalently, this is the distance that emerges when using an
ordinary euclidean distance on the rescaled data set, which premultiplies $$X$$ according to  $$\Sigma^{-1/2}$$.
+ __Manhattan / $$\ell_1$$-distance__: Defined as $$d(x_i, x_j) = \sum_{k=1}^d  | x_{ik} - x_{jk}| $$ When applied to binary data, this counts the
number of mismatches between coordinates.
+ __Cosine distance:__ This measures the size of the angle between a pair of
vectors. This can be especially useful when the length of the vectors is
irrelevant. For example, if a document is pasted to itself, the word count
has doubled, but the relative occurrences of words has remained the same.
Formally, this distance is defined as $$d(x_i, x_j) = 1 - \frac{x_i^Tx_j}{||x_i||_2 ||x_j||_2} = 1 - \cos(\theta_{x_i, x_j})$$
where $$\theta_{x_i, x_j}$$ represents the angle between $$x_i$$ and $$x_j$$ .
+ __Jaccard:__ This is a distance between pairs of length $$p$$ binary sequences $$x_i$$ and $$x_j$$  defined as
$$d(x_i, x_j) = 1 - \frac{ \sum_{k=1}^p 1_{x_{ik} = x_{jk} =1}}{p} $$
or one minus the fraction of coordinates that are 0/1. The motivation for
this distance is that coordinates that are both 0 should not contribute to
similarity between sequences, especially when they may be dominated by
0s. We apply this distance to the binarized version of the species counts.
+ __Dynamic time warping distance:__
A dynamic time warping distance is
useful for time series that might not be quite aligned. The idea is to
measure the distance between the time series after attempting to align
them using dynamic programming.
+ __Mixtures of distances:__ Since a convex combination of distances is still a
distance, new ones can be tailored to specific problems accordingly. For
example, on data with mixed types, a distance can be defined to be a
combination of a euclidean distance on the continuous types and a jaccard
distance on the binary types.






<h3 id="subsubsection-12"> 1.2. Probabilistic clustering techniques </h3>

In contrast, probabilistic clustering techniques assume latent cluster indicator $$z_i$$ ( e.g  $$z_i =k$$ if sample $$i$$ belongs to cluster $$k$$(  for each sample and define a likelihood model (which must itself be fit)
assuming these indicators are known. Inference of these unknown $$z_i$$'s provides
the sample assignments, while the parameters fitted in the likelihood model can
be used to characterize the clusters. Some of the most common probabilistic
clustering models are:
+ __Gaussian mixture model:__ The generative model here supposes that there
are $$K$$ means $$\mu_{1; \cdots ; K}$$. Each sample $$i$$ is assigned to one of $$K$$ categories
(call it $$z_i = k$$), and then the observed sample is drawn from a gaussian
with mean $$\mu_k$$ and covariance $$\Sigma$$  (which doesn't depend on the class assignment). This model can be considered the probabilistic analog of K-means
(K means actually emerges as the small-variance limit).
+ __Multinomial mixture model:__ This is identical to the gaussian mixture
model, except the likelihood associated with the kth group is $$\text{Mult} (n_i; p_k)$$,
where $$n_i$$ is the count in the $$i$$th row. This is useful for __clustering count
matrices__.
+ __Latent Class Analysis:__
+ __Hidden Markov Model:__ This is a temporal version of (any of) the earlier
mixture models.The idea is that the underlying states zi transition to one
another according to a markov model, and the observed data are some
emission (gaussian or multinomial, say) from the underlying states. The
point is that a sample might be assigned to a centroid different from
the closest one, if it would allow the state sequence to be one with high
probability under the markov model.




<h2 id="subsection-2"> 2.  Low-dimensional representations </h2>  

+ __Principle Components Analysis__
+ __Factor analysis__
+ __Distance based methods__


<h2 id="subsection-3">    3. Networks </h2>




<h2 id="subsection-4"> 4.  Mixtures </h2>

