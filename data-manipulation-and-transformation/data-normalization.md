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

Sure! Here's a more detailed version of the scaling approaches section, integrating the specifics of the `mStat_normalize_data()` function:

## Scaling Approaches in Microbiome Analysis

When working with microbiome data, scaling is a pivotal step in ensuring that the data remains consistent across samples and is ready for downstream analysis. The `mStat_normalize_data()` function in MicrobiomeStat offers a suite of scaling techniques tailored to handle the unique challenges and intricacies of microbiome datasets.

### Available Scaling Techniques:

* **Total Sum Scaling (TSS):** This method involves scaling the counts in each sample so that they sum to a consistent total.
  
* **Cumulative Sum Scaling (CSS):** This technique adjusts counts based on the cumulative sum, focusing on ensuring consistent summation across the dataset.
  
* **DESeq Normalization:** Borrowed from the DESeq2 package commonly used for RNA-seq data, this method scales counts based on geometric means.

* **Trimmed Mean M-values (TMM):** This approach calculates scaling factors based on a weighted trimmed mean of the log-expression ratios.
  
* **Geometric Mean of Pairwise Ratios (GMPR):** GMPR uses A robust normalization method for zero-inflated count data such as microbiome sequencing data.
  
* **Rarefy-TSS Combination:** This method combines rarefaction, a subsampling approach, with total sum scaling to optimize count representation.

To apply any of these methods, you can use:

```r
normalized_data <- mStat_normalize_data(
  data.obj = obj,
  method = "TSS" # or any other desired method
)
```

### Function Workflow:

1. **Extract OTU Table:** The function begins by retrieving the OTU (Operational Taxonomic Unit) table from the provided data object.
  
2. **Determine Scaling Factor:** Depending on the chosen normalization method, an appropriate scaling factor is calculated. This factor will be applied to adjust the counts in the dataset.
  
3. **Data Scaling:** Counts in the OTU table are then scaled using the derived factor. Some methods, like "TSS" and "CSS," apply direct scaling. Others, like the "Rarefy-TSS" combination, employ a more intricate approach involving both rarefying and scaling.

4. **Re-aggregation (if necessary):** If a `feature.agg.list` exists within the data object, this list will be re-aggregated based on the newly normalized data to ensure consistency.

5. **Return Results:** The function concludes by returning both the normalized data object and the scaling factors that were applied. This ensures that users have a clear understanding of the transformations and can ensure reproducibility in their work.

### Key Considerations:

After constructing your `mStat` data object, it's essential to use `mStat_validate_data()` to validate its integrity. Only then should you proceed with normalization. Moreover, while `mStat_validate_data()` provides comprehensive checks, it isn't exhaustive. If it clears your data, it indicates the absence of obvious issues but doesn't guarantee flawless function operability across the board.

Remember, scaling is a fundamental step in microbiome data analysis, ensuring that your findings are robust, accurate, and reflective of the underlying biological realities.

### Note on Choosing a Method

The best normalization method largely depends on the specific nature and requirements of your study. Rarefaction, for instance, might discard valuable data in samples with higher sequencing depth. Meanwhile, scaling approaches adjust counts without discarding data. Still, the appropriateness of each method varies based on the dataset's characteristics and the research question.

**It's crucial to normalize your data, whether through rarefaction or scaling. This ensures the comparability of samples, laying a strong foundation for downstream analyses such as beta-diversity assessments, clustering, and differential abundance testing.**
