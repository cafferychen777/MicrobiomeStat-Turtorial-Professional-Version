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

<figure><img src="../.gitbook/assets/Screenshot 2023-10-08 at 21.44.44.png" alt=""><figcaption><p>Using the <code>mStat_summarize_data_obj()</code> function on a specified dataset, a boxplot depicting sequencing depth over time has been generated. This plot illustrates the distribution of sequencing depth across various time points, stratified by a provided grouping variable, such as patient race. For instance, if the grouping variable is patient race, samples from African American subjects might be shown in red, while those from Caucasian subjects could be depicted in blue. This visualization aids in the visual inspection of the longitudinal sampling distribution when stratified by a significant covariate. The <code>mStat_summarize_data_obj()</code> function, combined with the inclusion of grouping variables, facilitates an informative exploration of metadata and temporal distributions.</p></figcaption></figure>

After executing the `mStat_summarize_data_obj()` function, we obtain a structured summary table that captures the essential attributes and metrics of the `subset_T2D.obj` dataset. This table encompasses various facets, from basic statistical properties of the microbiome feature table to the detailed insights about metadata, feature annotations, and time-series distribution. Let's delve into the results:

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

In summary, the `mStat_summarize_data_obj()` function furnishes a detailed and structured overview of a MicrobiomeStat data object. This comprehensive breakdown ensures that the user possesses a firm grasp on their data's intricacies and characteristics, setting the stage for well-informed subsequent statistical analyses.
