---
layout: default
tags: [literature, SNV, somatic-variant, bioinformatics, classification]
title: Approaches to Somatic SNV Calling
---

## Somatic SNV callers

### Strelka

* good overall performance ([Roberts 2013 Bioinformatics][Roberts 2013 Bioinformatics], [Cai 2016 Sci Rep][Cai 2016 Sci Rep])
* especially strong at low allele fraction ([Roberts 2013 Bioinformatics][Roberts 2013 Bioinformatics])

## Filters

* strand bias, nearby SNVs, spanning deletions, adjacent indels, variant base and mapping qualities

## Evaluating performance

* simulation
* More accurate sequencing techniques e.g. Sanger
* dbSNP, HapMap (false positives)

## General findings

* using multiple callers is beneficial ([Roberts 2013 Bioinformatics][Roberts 2013 Bioinformatics])
* post-calling filters are beneficial because error characteristics change with technology so it's easier to design new post-hoc filters than new calling methods
    * call sets are the most sensitive to strand bias ([Roberts 2013 Bioinformatics][Roberts 2013 Bioinformatics])

[Roberts 2013 Bioinformatics]: https://www.ncbi.nlm.nih.gov/pmc/articles/PMC3753564
[Cai 2016 Sci Rep]: https://www.ncbi.nlm.nih.gov/pmc/articles/PMC5118795/
<!-- MathJax scripts -->
<script type="text/javascript" src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML"></script>
