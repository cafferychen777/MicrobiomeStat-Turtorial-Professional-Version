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

The `mStat_subset_data()` function allows users to subset their microbiome data based on specific sample IDs or conditions present within the metadata. This operation is useful for focusing on particular samples of interest or when segregating data for analysis.

#### Example: Subsetting by Sample IDs

Load the necessary library and dataset:

```r
# Load the required libraries
library(MicrobiomeStat)

# Prepare data for the function
data(peerj32.obj)
```

Next, subset the data object using specific sample IDs:

```r
# Subset data object by sample IDs
subset.ids <- rownames(peerj32.obj$meta.dat)[1:10] # for demonstration purposes
subset_peerj32.obj <- mStat_subset_data(data.obj = peerj32.obj, samIDs = subset.ids)
```

This example considers only the first ten samples from the `peerj32.obj` dataset for subsetting.

#### Example: Subsetting by a Condition

Continue with the `peerj32.obj` dataset. You can also subset based on conditions present in the metadata:

```r
# Subset data object by condition
condition <- "time == '1'" # for demonstration purposes
subset_peerj32.obj2 <- mStat_subset_data(data.obj = peerj32.obj, condition = condition)
```

In this example, the data object is subsetted to retain only those samples that belong to the first-time point (`time == '1'`).

By following these real-world examples, users can efficiently subset their microbiome datasets using either sample IDs or specific conditions from the metadata.

### Working with Distance Matrices: `mStat_subset_dist()`

```{r}
mStat_subset_dist(
  dist.obj = dist,
  samIDs = c("s1", "s2", "s3")  
)
```

This function processes each matrix in the list, subsetting based on the provided sample IDs.

### Filtering Microbiome Data by Prevalence and Abundance: `mStat_filter()`

The `mStat_filter()` function serves as a cornerstone within the MicrobiomeStat package for filtering microbiome data based on prevalence and abundance thresholds. Given its integration into various other functions that accept `prev.filter` and `abund.filter` parameters, it plays a critical role in ensuring data quality and relevance. By consistently filtering data to retain only taxa that satisfy specified criteria, the package ensures that analyses are concentrated on the most pertinent and widespread taxa.

#### Usage:

```r
mStat_filter(
  x = data_matrix,
  prev.filter = 0.5,
  abund.filter = 5
)
```

#### Function's Operation:

1. **Data Reshaping:** The function reshapes the input matrix/data frame to a long format, making it more amenable for grouping and filtering operations.

2. **Grouping by Taxa:** Data is grouped by individual taxa. This allows for the computation of both average abundance and prevalence for each taxon.

3. **Calculating Metrics:** Within each group (i.e., for each taxon), the function calculates:
   - `avg_abundance`: The average abundance across all samples.
   - `prevalence`: The proportion of samples where the taxon is present (non-zero).

4. **Filtering:** The function then applies the user-specified thresholds to filter the taxa:
   - Taxa that have a prevalence below `prev.filter` are removed.
   - Taxa that have an average abundance below `abund.filter` are removed.

#### Practical Example:

To illustrate the functionality, let's consider a mock matrix:

```r
# Define a mock matrix
data_matrix <- matrix(c(0, 3, 4, 0, 2, 7, 8, 9, 10), ncol=3)
colnames(data_matrix) <- c("sample1", "sample2", "sample3")
rownames(data_matrix) <- c("taxa1", "taxa2", "taxa3")

# Use the mStat_filter function
filtered_data <- mStat_filter(data_matrix, 0.5, 5)

# Display the filtered data
print(filtered_data)
```

Similarly, you can use the `mStat_filter()` function on real microbiome datasets, like `peerj32.obj`, to streamline your data based on prevalence and abundance before diving into deeper analyses.

## Conclusion

Data filtering is an essential step in microbiome analysis. With MicrobiomeStat, researchers have a comprehensive set of tools to refine their data, ensuring that downstream analysis is both meaningful and interpretable.
