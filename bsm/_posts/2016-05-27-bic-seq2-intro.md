---
layout: post
title: Introduction to BIC-seq and BIC-seq2
tags: [ CNV, regression, classification, modeling, discussion ]
---

## Motivation

Understand the strengths and weaknesses of BIC-seq ([Xi et al 2011]) and BIC-seq2 (Xi et al, unpublished).

## CNV detection: approaches, tools

[Alkosdi et al 2014], [Pirooznia et al 2015], and [Zhao et al 2013] reviewed CNV detection approaches and software tools implementing those.  Below are the main approaches (Zhao et al 2013, Fig 1).  BIC-seq(2) is one of the read depth based techniques, whose main idea is that CNV results in read depth variation.

![fig1]({{ site.baseurl }}/assets/projects/bsm/2016-05-27-bic-seq2-intro/zhao-2013-cnv-review-fig1.jpg)

### Properties of read depth approaches

**advantages**

* informative on the amplitude of CNV (estimating copy ratio) not just on the breakpoints; this seems important for a mosaic sample in which some cells have a CNV while others don't
* work well for long CNV segments

**disadvantages**

* read depth depends not only on CNV but also on other variables such as GC content; these confound CNV detection and so must be removed somehow
* breakpoints are uncertain
* short CNV segments are hard to detect

### Performance of BIC-seq

[Alkosdi et al 2014] evaluated performance of several CNV detection tools including BIC-seq using simulations.  All of these tools were published between 2008--2012.  This analysis shows that ControlFreeC ([Boeva et al 2011]) and BIC-seq perform the best in terms of the sensitivity and specificity of calling CNV segments.  Like BIC-seq2, but unlike BIC-seq, ControlFreeC requires only a case sample without the need of a matched control sample.

![alkosdi 2014 fig 5]

As the authors of BIC-seq, [Xi et al 2011] also evaluated its performance with simulations and compared it to a single tool.  In their new unpublished manuscript Xi et al compared the performance of BIC-seq2 to BIC-seq and three other tools including ControlFreeC using both simulations and a set of experimentally validated CNVs.  They found that BIC-seq2 performed the best under most conditions while ControlFreeC showed nearly as strong performance. 

## BIC-seq and BIC-seq2

### Segmentation

Both techniques use the same overall approach to calling CNV segments and estimating copy number ratios for those segments.  The iterative segmentation procedure (Fig 1 of [Xi et al 2011], shown below) is summarized by the following algorithm:

1. *initialization*: start with an initial dense segmentation (initial set of CNV breakpoints)
1. *test stopping condition*: based on $$\Delta\mathrm{BIC}$$, the improvement by removing a breakpoint $$b$$ such that $$b$$ leads to the largest improvement compared to the removal of any other new breakpoint

    1. stop if $$\Delta\mathrm{BIC}\ge 0$$
    1. continue otherwise

1. *segmentation*: remove $$b$$ and go back to the previous step

Note that the removal of breakpoints successively reduces the complexity of the
segmentation model of CNV along the chromosome. Therefore BIC (Bayesian
information criterion) controls the eventual number and exact position of
breakpoints.

![xi 2011 fig 1]

### BIC and the $$\lambda$$ tuning parameter

The task in CNV detection on a chromosome is to model CNV as an idealized
segmentation pattern $$\theta$$. The model parameters $$\theta$$ contain two
kind of information on the CNV segmentation:

1. the number $$k$$ of segments and the position $$\tau_j$$ of demarcating breakpoints,
1. the (expected) copy number ratio $$p_j$$ for each segment,

where $$j=1,...,k$$.

The segmentation procedure uses a modified BIC:

$$
\begin{eqnarray}
\mathrm{BIC}  =
\begin{cases}
-2 \ell(\theta) + k \log n & \text{standard} \\
-2 \ell(\theta) + \lambda \, k \log n & \text{BIC-seq, BIC-seq2}
\end{cases}
\end{eqnarray}
$$

In the present context of CNV detection model complexity means the number $$k$$ of CNV segments ($$k-1$$ breakpoints) on a chromosome.

Under the standard definition the first term $$\ell(\theta)$$ is the log likelihood of segmentation $$\theta$$ whereas the second term $$k \log n$$ penalizes the number $$k$$ of segments ($$n$$ is the number of total mapped reads, which is not affected by the segmentation procedure).  A general property of the standard BIC is that with more data a greater decrease in log likelihood $$\ell(\theta_{i+1})-\ell(\theta_i)$$ is caused by the removal of a given breakpoint in the $$i$$th iteration.  Therefore eventually more breakpoints are retained even under standard BIC; in other words: more data means greater $$k$$.

[Xi et al 2011] introduce the additional $$\lambda$$ parameter, which multiplies the penalty term and which enables the user to tune BIC-seq and BIC-seq2.  The motivation behind $$\lambda$$ is to take into account:

* "the desired level of confidence in the CNV calls",
* and "the genome coverage"

Theoretically, this seems superfluous because, as we saw, the standard BIC already provides an automatic (and theoretically justified) mechanism for finding the optimal number $$k$$ of CNV segments depending on the amount of data (coverage).  [Xi et al 2011] provide a heuristic practical recipe for the usage of $$\lambda$$ but the utility of that recipe is unclear and not convincing.

They also show that the called CNV segments are affected by $$\lambda$$ to a degree that depends on both the length of the segment and its copy number ratio ([Xi et al 2011] Fig 2, below with $$\lambda=2$$ on the *left* and $$\lambda=4$$ on the *right*).

![xi 2011 fig 2]

### What's new in BIC-seq2

Compared to its predecessor (BIC-seq) BIC-seq2 modifies the log likelihood of a segmentation.  The log likelihood in BIC-seq is defined as

$$
\begin{equation}
\ell(\theta) = \sum_{i=1}^n \log g_1(p_i) - \log g_0(p_i) + \log f(s_i),
\end{equation}
$$

where $$p_i$$ is the expected copy number (relative to control) and $$s_i$$ is the mapped position of read $$i$$ of total $$n$$ reads.  The functions $$g_1, g_0$$ are probabilities of a simple multinomial model referring to case and control reads, respectively. $$f$$ is an unknown function that reflects the contribution of GC content and other confounding variables.  Since $$f$$ is invariable to $$\theta$$ it does not affect the $$\Delta\mathrm{BIC}$$ values that govern the segmentation procedure and can be ignored.

The normalization step of BIC-seq2 (before segmentation) *estimates* $$g_0 + f$$ using GC content of a segment and the nucleotide composition around it and employing semi-parametric regression techniques.  Therefore no matched control sample is required.  The normalization step of BIC-seq2 uses some assumptions (e.g. read count is locally Poisson-distributed) that were deprecated by [Xi et al 2011] when they introduced BIC-seq.

## Challenges

* how to set $$\lambda$$?
* significance of CNV segments?  Issues with p-values
* mosaicism results in mixed sample: how does BIC-seq2 perform?

[Alkosdi et al 2014]: http://bib.oxfordjournals.org/content/16/2/242.long
[Boeva et al 2011]: http://bioinformatics.oxfordjournals.org/content/27/2/268.full
[Pirooznia et al 2015]: http://dx.doi.org/10.3389/fgene.2015.00138
[Zhao et al 2013]: http://bmcbioinformatics.biomedcentral.com/articles/10.1186/1471-2105-14-S11-S1
[Xi et al 2011]: http://www.pnas.org/content/108/46/E1128.full

[xi 2011 fig 1]: http://www.pnas.org/content/108/46/E1128/F1.large.jpg
[xi 2011 fig 2]: http://www.pnas.org/content/108/46/E1128/F2.large.jpg
[pirooznia 2015 fig 1]: http://www.frontiersin.org/files/Articles/129627/fgene-06-00138-r2/image_m/fgene-06-00138-g001.jpg
[alkosdi 2014 fig 5]: http://bib.oxfordjournals.org/content/16/2/242/F5.large.jpg
