---
description: >-
  This page provides an overview of the mStat_summarize_data_obj() function,
  which summarizes key components of a MicrobiomeStat data object.
---

# Data Summary

#### Overview

The `mStat_summarize_data_obj()` function in the MicrobiomeStat package summarizes the key components of a MicrobiomeStat data object, including:

* feature.tab - microbiome feature table
* meta.dat - sample metadata
* feature.ann - feature annotations
* tree - phylogenetic tree (optional)

It generates useful summary statistics and information to understand the properties of the data object before further analysis.

#### Usage

```markdown
mStat_summarize_data_obj(
  data.obj,
  time.var = NULL, 
  group.var = NULL,
  palette = NULL
)
```

* `data.obj`: MicrobiomeStat data object to summarize
* `time.var`: Optional time variable column name in meta.dat to analyze temporal distribution
* `group.var`: Optional grouping variable in meta.dat to group time histogram
* `palette`: Vector of colors for grouping in histogram

#### Details

* For `feature.tab`, the summary includes total reads, average reads per sample, sparsity, etc.
* For `meta.dat`, it summarizes sample metadata fields, and optionally visualizes sample distribution over time.
* For `feature.ann`, it calculates proportion of missing values for each annotation.
* It also checks for a phylogenetic `tree` and aggregated taxonomies.
* The summary is returned as a tidy tibble data frame for easy inspection.

#### Example

```r
library(MicrobiomeStat)

data(subset_T2D.obj)

# Summarize data object
summary <- mStat_summarize_data_obj(subset_T2D.obj, "visit_number", "subject_race")

# View summary
print(summary)
```

<figure><img src="../.gitbook/assets/Screenshot 2023-08-07 at 18.55.31.png" alt=""><figcaption><p>Histogram of sample counts over time by subject race using mStat_summarize_data_obj() on the subset_T2D dataset. The plot shows the distribution of sample counts across visit numbers, grouped by subject race. Samples from African American subjects are shown in red, while samples from Caucasian subjects are shown in blue. This allows visual inspection of the longitudinal sampling distribution stratified by an important covariate. The mStat_summarize_data_obj() function and inclusion of grouping variables enables informative exploration of metadata and temporal distributions.</p></figcaption></figure>

<figure><img src="../.gitbook/assets/Screenshot 2023-10-08 at 21.44.44.png" alt=""><figcaption></figcaption></figure>

| Category                | Variable                                     | Value    |
| ----------------------- | -------------------------------------------- | -------- |
| Basic Statistics        | Number of samples                            | 575      |
| Basic Statistics        | Number of features                           | 9533     |
| Basic Statistics        | Min. reads per sample                        | 2007     |
| Basic Statistics        | Max. reads per sample                        | 91908    |
| Basic Statistics        | Total reads across all samples               | 14138179 |
| Basic Statistics        | Average reads per sample                     | 1483.078 |
| Basic Statistics        | Median reads per sample                      | 21062    |
| Basic Statistics        | Proportion of zero counts                    | 0.963    |
| Basic Statistics        | Count of features that only appear once      | 1505     |
| Metadata                | Number of metadata variables                 | 14       |
| Feature Annotations     | Proportion of missing annotations in Kingdom | 0        |
| Feature Annotations     | Proportion of missing annotations in Phylum  | 0        |
| Feature Annotations     | Proportion of missing annotations in Class   | 0.002    |
| Feature Annotations     | Proportion of missing annotations in Order   | 0.012    |
| Feature Annotations     | Proportion of missing annotations in Family  | 0.128    |
| Feature Annotations     | Proportion of missing annotations in Genus   | 0.484    |
| Feature Annotations     | Proportion of missing annotations in Species | 0.887    |
| Phylogenetic Tree       | Exists in the dataset                        | No       |
| Time-Series Information | Number of unique time points                 | 6        |
| Time-Series Information | Sample count at time point: 1                | 117      |
| Time-Series Information | Sample count at time point: 2                | 104      |
| Time-Series Information | Sample count at time point: 3                | 97       |
| Time-Series Information | Sample count at time point: 4                | 104      |
| Time-Series Information | Sample count at time point: 5                | 79       |
| Time-Series Information | Sample count at time point: 6                | 74       |

This generates an informative summary of a MicrobiomeStat data object before statistical analysis.
