---
description: >-
  Data filtering selectively removes certain features or samples from the
  dataset.MicrobiomeStat provides key functions to filter data objects based on
  criteria.
---

# Data Filtering

Microbiome data is rich and intricate, requiring specialized tools to aid in preprocessing and analysis. Filtering is one of the key steps in this process, allowing you to selectively focus on relevant samples and features.

## Overview of Data Filtering

Filtering, at its core, selectively removes features or samples from your dataset based on specific criteria. This refined focus can greatly improve the quality and interpretability of downstream analysis.

MicrobiomeStat has been designed to provide researchers with a set of flexible functions tailored to the unique challenges of microbiome data.

### Key Functions

- `mStat_remove_feature()`: This function allows you to remove specific features based on their IDs.
- `mStat_subset_data()`: Subset your data based on sample IDs or even conditions provided in the metadata.
- `mStat_subset_dist()`: If you're working with distance matrices, this function lets you subset them by sample ID.
- `mStat_filter()`: This versatile function aids in filtering taxa based on minimum prevalence and average abundance, ensuring the relevance of your data.

## Diving Deep into Each Function

### Removing Features with `mStat_remove_feature()`

```{r}
mStat_remove_feature(
  data.obj = obj,
  featureIDs = c("feat1", "feat2"),
  feature.level = "Family"
)
```

What's happening under the hood?

- It verifies the feature IDs.
- Separately handles the 'original' level.
- Subsets the feature and annotation tables.
- Recalculates abundance aggregation.

### Subset by Sample using `mStat_subset_data()`

```{r}
mStat_subset_data(
  data.obj = obj,
  samIDs = c("s1", "s2", "s3") 
)
```

You can also subset based on conditions present in the metadata:

```{r}
mStat_subset_data(
  data.obj = obj,
  condition = "group == 'control'"
)
```

The function verifies the provided condition, subsets the metadata, and then extracts the corresponding samples from the feature table and annotations. This metadata-driven approach is similar to `dplyr::filter`.

### Working with Distance Matrices: `mStat_subset_dist()`

```{r}
mStat_subset_dist(
  dist.obj = dist,
  samIDs = c("s1", "s2", "s3")  
)
```

This function processes each matrix in the list, subsetting based on the provided sample IDs.

### Filtering Microbiome Data by Prevalence and Abundance: `mStat_filter()`

```{r}
mStat_filter(
  x = data_matrix,
  prev.filter = 0.5,
  abund.filter = 5
)
```

The function operates by:

- Reshaping data to a long format for easier processing.
- Grouping by taxa to calculate their average abundance and prevalence.
- Applying filters based on the set thresholds.

For example, using a mock matrix:

```{r}
data_matrix <- matrix(c(0, 3, 4, 0, 2, 7, 8, 9, 10), ncol=3)
filtered_data <- mStat_filter(data_matrix, 0.5, 5)
print(filtered_data)
```

## Conclusion

Data filtering is an essential step in microbiome analysis. With MicrobiomeStat, researchers have a comprehensive set of tools to refine their data, ensuring that downstream analysis is both meaningful and interpretable.
