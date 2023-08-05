---
description: >-
  Data aggregation summarizes microbiome data at broader taxonomic levels. This
  reveals intuitive compositional patterns and trends.
---

# Data Aggregation

## Data Aggregation

Data aggregation is a critical step in microbiome data analysis. It summarizes the feature table at broader taxonomic levels, providing intuitive insights into compositional patterns. The `mStat_aggregate_data()` function enables simple, flexible data aggregation within MicrobiomeStat.

### Overview

`mStat_aggregate_data()` aggregates a microbiome dataset by a specified subject variable and optional stratification variable.

It collapses the feature table by taxonomic annotations to generate aggregated abundance tables at each taxonomic level.

The output is a new MicrobiomeStat data object containing the aggregated data for downstream analysis.

### Usage

A basic usage is:

```{r
mStat_aggregate_data(
  data.obj = obj,
  subject.var = "subject_id",
  strata.var = "group"
)
```

Where:

* `data.obj` is the MicrobiomeStat data object
* `subject_var` is the subject variable like patient ID
* `strata_var` is an optional grouping variable

### Details

`mStat_aggregate_data()` performs:

* **Taxonomic aggregation** of the feature table by collapsing features
* Summarization by **subject and stratification variables**
* Generation of **aggregated abundance tables** for each taxonomic level

It checks `feature.agg.list` and appends new aggregation tables. If no list exists, it creates one.

Key steps include:

* Collapsing feature table by taxonomic annotations
* Grouping by subject and stratification variables
* Summarizing abundance values within each group
* Returning aggregated tables and updated metadata

This provides aggregated data tailored to various analyses like beta diversity, differential abundance testing, and longitudinal studies.

### Applications

Key uses of this function:

* Examine compositional patterns at higher taxonomic levels
* Reduce table complexity for certain analyses
* Enable stratified comparisons of aggregation profiles
* Prepare data for longitudinal analysis

**Proper data aggregation unlocks microbiome discoveries** at broader ecological scales. `mStat_aggregate_data()` provides an automated, flexible way to generate aggregated datasets for diverse downstream tasks.
