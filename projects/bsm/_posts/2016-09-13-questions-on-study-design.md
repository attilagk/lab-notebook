---
layout: default
---

abbreviations

* CNV: copy number variation
* SNV: single nucleotide variant
* TE: transposable element
* SCZ/CTR: schizophrenia/control

## Questions

### Biological

* priority of goals?
    1. descriptive: pattern of somatic mutations in SCZ and in CTR; recurrently mutated genes in SCZ
    1. hypothesis driven: test statistical significance between SCZ and CTR
* priority of mutation types: CNV, SNV, or TE?
* priority for coding regions? (exploit rich functional annotation, use pathway analysis)

Note that hypothesis driven statistical analysis will benefit (require?) a parametric model.
For CNVs, for example, the parameters would hold information on the frequency,
length and amplification of CNVs, as well as on the set of genes known to be
associated with SCZ.  Even if the descriptive analysis produced only weakly
significant and therefore inconclusive results on single genes, a global (exome
or genome-wide) parametric model would allow the aggregation of those results
to test SCZ vs CTR hypothesis with high power.

### Technical

* separate WGS (low coverage) and WES (high coverage)?
* low coverage + many individuals or high coverage + few individuals? (the former seems more ideal)
* integration
    * a single "blindly trusted" tool or, as recommended, several complementary tools combined (if yes, how)?
    * sequencing platform: long read platform (PacBio) in addition to short read one (Illumina)?
    * RNA-seq for CNV?

### BIC-seq(2)

* BIC-seq or BIC-seq2?
    * possible to combine them with each other and with other approaches?
    * if only BIC-seq2, how to utilize reference tissue?
* how to use BIC-seq(2) for hypothesis driven analysis (testing SCZ-CTR difference)?
* who to ask regarding support on software (bugs, etc)?

## Decision theoretic design of experiments

1. review of previous studies and resources
    * known quantitative patterns of somatic and germline variation
    * NGS-based approaches and tools to study them
    * loss (utility) function: the utility of testing power versus the cost (money, time) of data generation and analysis
1. simulations
    * designed on quantitative biological patterns
    * various coverage, fraction of mutant cells, difference in SCZ-CTR, and number of individuals
    * evaluate performance of several tools
1. gold standard data (1000 genome)?
1. decisions on design
    * based on observed performance and loss function
<!-- MathJax scripts -->
<script type="text/javascript" src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML"></script>
