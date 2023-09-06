---
description: >-
  Data normalization is an essential step in microbiome data analysis. It helps
  account for sequencing depth differences and make samples comparable.
---

# Data Normalization

Microbiome data often exhibit significant variance in sequencing depth among samples. Such variances, if unaccounted for, can skew the analysis and interpretation. **Data normalization is crucial to account for this technical variation**, ensuring that samples are comparable and results are robust.

MicrobiomeStat provides several normalization techniques. These can be broadly categorized into:

1. Rarefaction
2. Scaling approaches like TSS, CSS, DESeq, TMM, GMPR, and more

## Rarefaction

Rarefaction is a technique that equalizes the sequencing depth by randomly subsampling reads from each sample. 

The `mStat_rarefy_data()` function rarefies the feature table to an even sequencing depth.

```{r}
mStat_rarefy_data(
  data.obj = obj,
  depth = 10000
)
```

When you don't specify a depth, it automatically chooses the smallest total count across samples.

**The function follows these steps:**

* Validates if the data object contains a feature table.
* Subsamples the feature table to achieve an even depth.
* Removes rows that contain only zeros.
* Updates the data object and any associated annotations.

Through rarefaction, technical variations between samples are minimized, promoting a more equitable comparison.

## Scaling Approaches

Instead of subsampling as in rarefaction, scaling approaches adjust the counts based on certain derived factors.

`mStat_normalize_data()` is the function you'd use to employ various scaling techniques, such as:

* Total sum scaling (TSS)
* Cumulative sum scaling (CSS)
* DESeq normalization
* Trimmed mean M-values (TMM)
* Generalized Median Polish Ratio (GMPR)
* Rarefy-TSS combination 

```{r}
mStat_normalize_data(
  data.obj = obj,
  method = "TSS" 
)
```

How the function works:

* First, it extracts the OTU (Operational Taxonomic Unit) table from the data object.
* Based on the method chosen, it calculates the appropriate scaling factor.
* For some methods, the data is scaled directly using this factor. For others, a more nuanced adjustment, such as rarefying followed by scaling, is applied.
* If a `feature.agg.list` exists in the data, it re-aggregates based on the newly normalized data.

Finally, the function returns the normalized data object and the scaling factors used. This is vital for reproducibility and understanding the transformation applied.

### Note on Choosing a Method

The best normalization method largely depends on the specific nature and requirements of your study. Rarefaction, for instance, might discard valuable data in samples with higher sequencing depth. Meanwhile, scaling approaches adjust counts without discarding data. Still, the appropriateness of each method varies based on the dataset's characteristics and the research question.

**It's crucial to normalize your data, whether through rarefaction or scaling. This ensures the comparability of samples, laying a strong foundation for downstream analyses such as beta-diversity assessments, clustering, and differential abundance testing.**
