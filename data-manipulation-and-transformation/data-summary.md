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

| Category                      | Variable                                     | Value                                                                                                                                                                    |
| ----------------------------- | -------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Basic Statistics              | Min. reads per sample                        | 2007                                                                                                                                                                     |
| Basic Statistics              | Max. reads per sample                        | 91908                                                                                                                                                                    |
| Basic Statistics              | Total reads dplyr::across all samples        | 16314146                                                                                                                                                                 |
| Basic Statistics              | Average reads per sample                     | 1352.86060203997                                                                                                                                                         |
| Basic Statistics              | Median reads per sample                      | 22230                                                                                                                                                                    |
| Basic Statistics              | Proportion of zero counts                    | 0.970438438838742                                                                                                                                                        |
| Basic Statistics              | Count of features that only appear once      | 1551                                                                                                                                                                     |
| Metadata                      | Number of metadata variables                 | 13                                                                                                                                                                       |
| Metadata                      | Metadata variables                           | file\_id, md5, size, urls, sample\_id, file\_name, env\_broad\_scale, env\_local\_scale, env\_medium, reference\_db, target\_gene, collection\_timestamp, feature\_count |
| Feature Annotations           | Proportion of missing annotations in Kingdom | 0                                                                                                                                                                        |
| Feature Annotations           | Proportion of missing annotations in Phylum  | 0                                                                                                                                                                        |
| Feature Annotations           | Proportion of missing annotations in Class   | 0.00232191724023551                                                                                                                                                      |
| Feature Annotations           | Proportion of missing annotations in Order   | 0.0154241645244216                                                                                                                                                       |
| Feature Annotations           | Proportion of missing annotations in Family  | 0.136744340326727                                                                                                                                                        |
| Feature Annotations           | Proportion of missing annotations in Genus   | 0.493573264781491                                                                                                                                                        |
| Feature Annotations           | Proportion of missing annotations in Species | 0.893440583796335                                                                                                                                                        |
| Phylogenetic Tree             | Exists in the dataset                        | No                                                                                                                                                                       |
| Time-Series Information       | Unique time-points                           | 6                                                                                                                                                                        |
| Distribution of sample counts | Sample Count at Time-point: 1                | 136                                                                                                                                                                      |
| Distribution of sample counts | Sample Count at Time-point: 2                | 117                                                                                                                                                                      |
| Distribution of sample counts | Sample Count at Time-point: 3                | 108                                                                                                                                                                      |
| Distribution of sample counts | Sample Count at Time-point: 4                | 116                                                                                                                                                                      |
| Distribution of sample counts | Sample Count at Time-point: 5                | 86                                                                                                                                                                       |
| Distribution of sample counts | Sample Count at Time-point: 6                | 79                                                                                                                                                                       |

This generates an informative summary of a MicrobiomeStat data object before statistical analysis.
