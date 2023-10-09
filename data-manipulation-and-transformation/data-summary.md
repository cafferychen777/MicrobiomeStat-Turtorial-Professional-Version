---
description: >-
  This page provides an overview of the mStat_summarize_data_obj() function,
  which summarizes key components of a MicrobiomeStat data object.
---

# Data Summary

## Overview

The `mStat_summarize_data_obj()` function in the MicrobiomeStat package provides a comprehensive summary of a MicrobiomeStat data object, ensuring users have a clear understanding of their data structure and contents. Here are the main components and features of the function:

* **feature.tab**: This is the microbiome feature table. The function computes and displays various summary statistics for this table such as minimum, maximum, average, and median reads per sample, the proportion of zero counts, and the count of features that only appear once in the data.

* **meta.dat**: Contains sample metadata. The function checks for the presence of this component and computes the number of metadata variables. If time-series information is provided through the `time.var` argument, it can further display histograms depicting sample counts over time, either ungrouped or stratified by a specified `group.var`. Additionally, the function showcases a boxplot of sequencing depth over time, a crucial visualization when dealing with longitudinal microbiome data.

* **feature.ann**: Represents feature annotations. For each column in the feature annotations, the function computes the proportion of missing annotations, which can be vital for downstream analysis that relies on these annotations.

* **tree**: This is an optional phylogenetic tree. The function checks for its presence in the data object and informs the user whether the tree exists or not.

* **feature.agg.list**: The function checks for the existence of a list of aggregated taxonomies and returns the names of taxonomies that have been aggregated, if available.

To support visualization, the function integrates with the `ggplot2` package to produce histograms and boxplots. It offers flexibility through optional parameters like `time.var`, `group.var`, and `palette`, allowing users to customize visualizations based on time-series data and specific grouping variables. The produced summary table offers a snapshot of the dataset's key characteristics, aiding in data exploration and ensuring appropriate pre-processing before advanced analyses.

## Usage

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

## Example

In this example, we'll demonstrate how to use the `mStat_summarize_data_obj()` function from the MicrobiomeStat package on a typical dataset `subset_T2D.obj`.

```r
library(MicrobiomeStat)

data(subset_T2D.obj)

# Summarize data object
summary <- mStat_summarize_data_obj(subset_T2D.obj, "visit_number", "subject_race")

# View summary
print(summary)
```

<figure><img src="../.gitbook/assets/Screenshot 2023-08-07 at 18.55.31.png" alt=""><figcaption><p>Histogram of sample counts over time by subject race using mStat_summarize_data_obj() on the subset_T2D dataset. The plot shows the distribution of sample counts across visit numbers, grouped by subject race. Samples from African American subjects are shown in red, while samples from Caucasian subjects are shown in blue. This allows visual inspection of the longitudinal sampling distribution stratified by an important covariate. The mStat_summarize_data_obj() function and inclusion of grouping variables enables informative exploration of metadata and temporal distributions.</p></figcaption></figure>

After executing the `mStat_summarize_data_obj()` function, we obtain a structured summary table that captures the essential attributes and metrics of the `subset_T2D.obj` dataset. This table encompasses various facets, from basic statistical properties of the microbiome feature table to the detailed insights about metadata, feature annotations, and time-series distribution. Let's delve into the results:

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

In summary, the `mStat_summarize_data_obj()` function furnishes a detailed and structured overview of a MicrobiomeStat data object. This comprehensive breakdown ensures that the user possesses a firm grasp on their data's intricacies and characteristics, setting the stage for well-informed subsequent statistical analyses.
