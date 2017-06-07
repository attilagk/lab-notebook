---
layout: default
tags: [ meeting ]
title: Notes on a BSM Network Workshop
---

## Introduction

A BSM Network workshop took place on September 16, 2016 at the Lieber Institute for Brain Development at Johns Hopkins University in Baltimore.  Andy and I participated from Mount Sinai.  See [the program brossure]({{ site.baseurl}}/assets/projects/bsm/bsm-network-workshop-2016-sep-program.pdf) for all presentations and sessions.

## Notes on presentations

### Daniel Weinberger

Weinberger presented a new method, quality surrogate variable (qSVA), for the correction of RNA-seq data for RNA degradation.  qSVA is based on molecular degradation experiments and uses gene-specific, local, degradation susceptibility information; this is in contrast with global correction methods which assume uniform susceptibility across genomic locations.  The corresponding article {% cite Jaffe2016 --file 2016-10-14-notes-on-bsm-network-workshop.bib %} is currently [in BioArxiv](http://biorxiv.org/content/early/2016/09/09/074245).  He also emphasized the importance of specific transcript features in age-dependent risk for schizophrenia (SCZ).

### Walsh & Park

A collaboration between Walsh's and Peter Park's group followed up on the disagreement between their own previous work {% cite Lodato2015 --file 2016-10-14-notes-on-bsm-network-workshop.bib %} and {% cite Hazen2016 --file 2016-10-14-notes-on-bsm-network-workshop.bib %}.  The disagreement concerns the rate of somatic mutations in the brain.  While both groups performed single cell sequencing, they used different techniques: {% cite Lodato2015 --file 2016-10-14-notes-on-bsm-network-workshop.bib %} multiple displacement amplification and {% cite Hazen2016 --file 2016-10-14-notes-on-bsm-network-workshop.bib %} biological amplification by neuronal nuclear transfer and cloning.  The latter study estimated 50 times *lower* SNV mutation rate.

In their new work Walsh, Park, et al further analyzed the consequences of MDA and found that the mutant allele frequency tended to deviate greatly from the expected 0.5 presumably due to MDA.  They highlighted a specific kind of SNV (I forgot which one) that might explain the 50-fold difference in somatic single nucleotide mutation rate estimates.

### Josheph Gleeson

Gleeson presented their analysis showing that even a tiny vector DNA contamination might appear as a (false positive) case of low level (3 %) somatic mosaicism.  His group used Vecuum {% cite Kim2016 --file 2016-10-14-notes-on-bsm-network-workshop.bib %} to identify false somatic variants.

### Maxwell Sherman

Max from Peter Park's group presented in Andy's time slot his ongoing work on correcting for **artifacts** in CNV calling using BIC-seq2.  These artifacts arise in particular when the DNA library preparation **protocol uses PCR** (PCR-used, such as NYGC) and to a much lesser extent when it does not (PCR-free, e.g. Macrogen).  The PCR-used protocol results in large background fluctuation of log2 copy number ratio.  In set of DNA libraries in which known CNV was present across multiple samples, libraries tended to cluster not by biological variation (i.e. known CNVs) but by technical variation (i.e. PCR-used vs PCR-free).

He introduced a score, [mean absolute percentage deviation (MAPD)](https://en.wikipedia.org/wiki/Mean_absolute_percentage_error), which is robust against outliers.  However, I forgot which quantity this MAPD relates to (perhaps log2 copy number ratio?).  He found correlation between background read depth and CNV segments detected by BIC-seq2.

Max investigated *stability* in the sense of the variation across multiple DNA libraries from a single sample.  He found that variation in the average read depth results in variation in CNV calls.

### Larrson Omberg

Larrson (Sage Bionetworks) talked about how the BSMN may use Synapse.  There are private as well as public spaces on Synapse.  There is a distributed file system (e.g. Amazon S3), which is version controlled.  Other main features: APIs and wikis.  Docker, a virtual machine, was mentioned (although it might not have been Larrson).

### Others

* demo on PsychENCODE, CommonMind
* NDAR storage was discussed (NCBI's alternative to Amazon S3) in conjunction with Synapse
* a private GitHub group for the DCC/DAC was proposed (Pevsner)
* Peter Park pointed out that all QC-d data will be made public and discussed its consequences in terms of research and publication within and outside of the BSM network
* Donald Freed (Pevsner's group) presented their new article, The Contribution of Mosaic Variants to Autism Spectrum Disorder {% cite Freed2016 --file 2016-10-14-notes-on-bsm-network-workshop.bib %}.  [The online article](http://journals.plos.org/plosgenetics/article?id=10.1371/journal.pgen.1006245) contains links to useful resources such as [Don Freed's GitHub page](https://github.com/DonFreed) or the [NDAR entry](https://ndar.nih.gov/study.html?id=334) of the study

## Personal communication

### Max Sherman

I asked him about BIC-seq (both BIC-seq1 {% cite Xi2011 --file 2016-10-14-notes-on-bsm-network-workshop.bib %} and BIC-seq2 {% cite Xi2016a --file 2016-10-14-notes-on-bsm-network-workshop.bib %}).  He told me the following points

* His long-germ goal: fully replace BIC-seq with a structural variant (SV) caller that uses info not only on read-depth but also on paired-end distances and split read mappings.
* There are many best practices/secrets about using BIC-seq successfully.  One that is extremely important is a post-hoc filtering to remove false positive CNVs that tend to accumulate at telomeres.
* *BIC-seq2 has completely superseded BIC-seq1* in every aspect and application.  In particular, BIC-seq2 **can** benefit from a reference tissue, although this feature is not detailed in the BIC-seq paper {% cite Xi2016a --file 2016-10-14-notes-on-bsm-network-workshop.bib %}.  In that case normalization (using generalized additive model) must be done separately for the reference and target tissue before the comparison of the two.
* Max developed a Python package that implements many of the best practices for BIC-seq2 and would be open to sharing it with me.
* I told Max I would like to visit their lab once I reach a point with BIC-seq2 at which I need his help.

### Weichen Zhou

Weichen works in Ryan Mills' group in the University of Michigan.  The whole group performs computational analysis of structural variation (SV) in NGS data and related methodological development.  *A main focus of the group is SV in SCZ.*  They use public data as gold standard and, using simulations, combine SV call sets from different SV callers.  They collaborate with Jeff Kidd's lab to experimentally validate the new approaches.  Weichen recommended the group's [new article](https://genomebiology.biomedcentral.com/articles/10.1186/s13059-016-0993-1) presenting SVelter, "an algorithm that identifies regions of the genome suspected to harbor a complex event and then resolves the structure by iteratively rearranging the local genome structure, in a randomized fashion, with each structure scored against characteristics of the observed sequencing data" {% cite Zhao2016 --file 2016-10-14-notes-on-bsm-network-workshop.bib %}

## References

{% bibliography --file 2016-10-14-notes-on-bsm-network-workshop.bib %}
