---
layout: page
title: Multiple Hypothesis Testing
permalink: /stat/mht/
---

When testing multiple hypotheses, by simple chance, you might end up having a few spurious results: you thus need to adjust for multiple hypotheses testing.
The issue is that in classical testing, errors are measured on a per-hypothesis
level. In the multiple testing setting, two proposals that quantify the error rate
over a collection of hypotheses are,
+ __Family-Wise Error Rate (FWER):__ This is defined as the probability that
at least one of the rejections in the collection is a false positive. It can be
overly conservative in settings with many hypothesis. That said, it is the
expected standard of evidence in some research areas.

+  __False Discovery Rate (FDR):__ This is defined as the expected proportion of
significant results that are false positives. Tests controlling this quantity
don't need to be as conservative as those controlling FWER.


## The Methods

Methods for multiple hypotheses testing include:
+ Bonferonni Correction
+ Benjamini-Hochberg Correction (BH-q)
+ Benjamini-Yekuteli:

A few other methods are worth noting:
+  Simes procedure: This is actually always more powerful than Bonferroni,
and applicable in the exact same setting, but people don't use it as often,
probably because it's not as simple.
+  Westfall-Young procedure: This is a permutation-based approach to FWER
control.
+ Knockoffs: It's possible to control FDR in high-dimensional linear models.
This is still a relatively new method though, so may not be appropriate for day-to-day consulting (yet).


## When should you use one vs another?


