---
description: >-
  Data filtering selectively removes certain features or samples from the
  dataset.MicrobiomeStat provides key functions to filter data objects based on
  criteria.
---

# Data Filtering

## Data Filtering

Filtering selectively removes features or samples based on criteria, enabling focused downstream analysis.

MicrobiomeStat provides key data filtering functions:

* `mStat_remove_feature()`: Remove features by ID
* `mStat_subset_data()`: Subset by sample ID
* `mStat_subset_dist()`: Subset distance matrices

### Remove Features

The `mStat_remove_feature()` function removes specified features from the data object:

```{r
mStat_remove_feature(
  data.obj = obj,
  featureIDs = c("feat1", "feat2"),
  feature.level = "Family"
)
```

It can filter at multiple taxonomic levels, or by original feature ID.

**Key steps:**

* Check feature IDs are characters
* Handle 'original' level separately
* Subset feature/annotation tables
* Recalculate abundance aggregation

Filtering focuses analysis on relevant features and improves result interpretation.

### Subset by Sample

The `mStat_subset_data()` function subsets the data object by sample IDs:

```{r
mStat_subset_data(
  data.obj = obj,
  samIDs = c("s1", "s2", "s3") 
)
```

It also allows **subsetting by a logical expression** on metadata:

```{r
mStat_subset_data(
  data.obj = obj,
  condition = "group == 'control'"
)
```

This filters samples based on metadata conditions, similar to dplyr::filter.

The function first checks if a condition is provided and subsets metadata accordingly. It then extracts matching samples from feature table and annotations.

This enables flexible, metadata-driven subsetting of microbiome data objects.

Key steps include:

* Handle sample IDs or logical expression
* Subset metadata
* Extract matching samples from tables
* Report excluded samples

Expressions offer an intuitive way to subset relevant samples for focused analysis.

### Subset Distance Matrices

The `mStat_subset_dist()` function subsets distance matrices by sample ID:

```{r
mStat_subset_dist(
  dist.obj = dist,
  samIDs = c("s1", "s2", "s3")  
)
```

It loops through the list of matrices, subsetting each by the sample IDs.

**Proper data filtering improves analysis quality and interpretability.** MicrobiomeStat provides flexible tools to filter microbiome data objects for focused downstream tasks.
