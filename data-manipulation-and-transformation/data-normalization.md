---
description: >-
  Data normalization is an essential step in microbiome data analysis. It helps
  account for sequencing depth differences and make samples comparable.
---

# Data Normalization

## Data Normalization

Microbiome data can vary substantially in sequencing depth between samples. **Data normalization accounts for this technical variation**, making samples comparable for robust analysis.

MicrobiomeStat provides two key normalization techniques:

* Rarefaction
* Scaling approaches like TSS, CSS, etc

### Rarefaction

The `mStat_rarefy_data()` function rarefies the feature table to an even sequencing depth.

```{r
mStat_rarefy_data(
  data.obj = obj,
  depth = 10000
)
```

It rarefies down to the specified depth or the smallest total count across samples if no depth is given.

**Steps include:**

* Check data object contains feature table
* Subsample feature table to even depth
* Remove all-zero rows
* Update data object and annotations

This reduces technical variation and enables fair comparisons between samples.

### Scaling Approaches

The `mStat_normalize_data()` function scales samples using various techniques:

* Total sum scaling (TSS)
* Cumulative sum scaling (CSS)
* DESeq
* Trimmed mean M-values (TMM)

```{r
mStat_normalize_data(
  data.obj = obj,
  method = "TSS" 
)
```

It estimates scale factors by the chosen approach and divides the feature table by these factors.

The scaled data object and normalization factors are returned.

**Proper normalization via rarefaction or scaling makes samples comparable prior to key analyses like beta-diversity, clustering, and differential abundance testing.**
