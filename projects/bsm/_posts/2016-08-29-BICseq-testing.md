---
layout: default
title: Testing the BICseq tool
---

The BICseq R package was installed both locally, on my laptop and on Minerva.  The critical segmentation step of the BICseq workflow fails, however.

Load package (warnings, messages suppressed)


```r
library(BICseq)
```

Using the test data supplied with BICseq create a BICseq object


```r
bs <- BICseq(sample = "../../data/BICseq/tumor_sorted.bam",
             reference = "../../data/BICseq/normal_sorted.bam", seqNames = "chr22")
str(bs)
```

```
## Formal class 'BICseq' [package "BICseq"] with 3 slots
##   ..@ samp    : chr "../../data/BICseq/tumor_sorted.bam"
##   ..@ ref     : chr "../../data/BICseq/normal_sorted.bam"
##   ..@ seqNames: chr "chr22"
```

The BICseq object appears to be OK.  Segmentation, however, fails


```r
segs <- getBICseg(object = bs)
```

```
## Error in .C("sort_rms_binning", as.integer(sample), length(sample), as.integer(reference), : "sort_rms_binning" not resolved from current namespace (BICseq)
```
<!-- MathJax scripts -->
<script type="text/javascript" src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML"></script>
