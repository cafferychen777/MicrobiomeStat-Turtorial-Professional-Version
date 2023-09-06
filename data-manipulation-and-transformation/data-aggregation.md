---
description: >-
  Data aggregation summarizes microbiome data at broader taxonomic levels. This
  reveals intuitive compositional patterns and trends.
---

# Data Aggregation in Microbiome Analysis

Data aggregation plays a vital role in simplifying and structuring microbiome data analysis. By summarizing data at broader taxonomic levels or by subject variables, users can gain clearer insights into compositional patterns within their datasets. MicrobiomeStat offers several functions for this purpose: `mStat_aggregate_data()`, `mStat_aggregate_by_taxonomy()`, and `mStat_aggregate_by_taxonomy2()`.

## 1. mStat_aggregate_data()

### Function Overview

The `mStat_aggregate_data()` function is designed to aggregate a microbiome dataset based on a specified subject variable. Additionally, users have the option to further stratify the aggregation through an additional variable.

```r
mStat_aggregate_data(
  data.obj = obj,
  subject.var = "subject_id",
  strata.var = "group"
)
```

Where:
- `data.obj`: Refers to the data object within MicrobiomeStat that needs to be aggregated.
- `subject.var`: Represents the primary variable for aggregation, such as a patient ID.
- `strata.var`: Acts as an optional stratification variable, allowing for more nuanced aggregation based on groups or categories.

## Detailed Breakdown

The `mStat_aggregate_data()` function executes the following key actions:

1. **Taxonomic Aggregation:** The function collapses the feature table based on taxonomic annotations, facilitating the generation of aggregated abundance tables across various taxonomic levels.

2. **Grouping and Summarization:** Data is grouped by the primary subject and, if specified, the stratification variable. Within these groups, abundance values are then summarized to provide a cohesive representation of data.

3. **Handling of `feature.agg.list`**: The function checks for the presence of a `feature.agg.list` within the `data.obj`. If it exists, the function appends new aggregation tables to it. In the absence of this list, a new one is created.

4. **Output Generation:** The final output is a renewed MicrobiomeStat data object. This object now houses the aggregated data, primed for downstream analysis.

## Practical Applications

The `mStat_aggregate_data()` function can be harnessed for a variety of analytical purposes:

- **Pattern Examination at Higher Taxonomic Levels:** Users can delve into compositional patterns at broader taxonomic scales, revealing overarching trends within the dataset.
  
- **Data Simplification:** By aggregating data, the overall complexity of tables is reduced. This is particularly beneficial for specific types of analyses where more straightforward data structures are preferred.
  
- **Stratified Data Comparisons:** If a stratification variable is specified, the function enables stratified comparisons. Users can contrast aggregation profiles across different groups or categories.
  
- **Data Preparation for Longitudinal Studies:** The function equips users with aggregated datasets, tailor-made for detailed longitudinal analyses.

In summation, effective data aggregation is pivotal for unlocking deeper microbiome insights, especially when looking at broader ecological contexts. The `mStat_aggregate_data()` function emerges as a quintessential tool in this regard, providing users with an automated yet flexible method to derive aggregated datasets suited for a plethora of analytical tasks.

## 2. mStat_aggregate_by_taxonomy()

### Function Overview

The `mStat_aggregate_by_taxonomy()` function facilitates aggregation based on a specific taxonomic level, such as "Phylum" or "Family". This ensures that the output is presented in a more biologically meaningful manner.

### Usage

```r
data.obj <- list(
  feature.tab = matrix(c(5, 10, 0, 7, 0, 20), nrow=3, byrow=TRUE),
  feature.ann = data.frame(Phylum = c("Bacteroidetes", "Firmicutes", "Bacteroidetes"))
)
rownames(data.obj$feature.tab) <- c("OTU1", "OTU2", "OTU3")
colnames(data.obj$feature.tab) <- c("Sample1", "Sample2")
rownames(data.obj$feature.ann) <- c("OTU1", "OTU2", "OTU3")

result.obj <- mStat_aggregate_by_taxonomy(data.obj, feature.level="Phylum")
```

### Detailed Breakdown

The `mStat_aggregate_by_taxonomy()` function first verifies the `feature.level` to ensure it's provided. Then, it loads the data object and joins the OTU table with the taxonomy table. Post this, it ensures the row names between the two tables are consistent. Lastly, it performs aggregation based on the chosen taxonomic level and updates the `feature.agg.list` with the aggregated table.

### Practical Applications

This function is particularly beneficial when:

- **Interpreting biological implications:** By aggregating data based on taxonomic levels, it's easier to draw biological conclusions from the data.

- **Reducing granularity:** Often, microbial OTU tables can be very granular, which might not be ideal for all types of analyses. Aggregating by taxonomy simplifies the data representation.

- **Harmonizing OTU and Taxonomy data:** By ensuring that the OTU and taxonomy tables' row names are consistent, the function prevents potential mismatches that can arise due to discrepancies between the tables.

## 3. mStat_aggregate_by_taxonomy2()

### Function Overview

Unlike its predecessor, the `mStat_aggregate_by_taxonomy2` function allows aggregation based on a single taxonomic level at a time. It focuses on merging the OTU table and the taxonomy table based on the provided taxonomy level.

### Usage

```r
feature.tab <- matrix(c(5, 10, 0, 7, 0, 20), nrow=3, byrow=TRUE)
rownames(feature.tab) <- c("OTU1", "OTU2", "OTU3")
colnames(feature.tab) <- c("Sample1", "Sample2")

feature.ann <- data.frame(Phylum = c("Bacteroidetes", "Firmicutes", "Bacteroidetes"))
rownames(feature.ann) <- c("OTU1", "OTU2", "OTU3")

result <- mStat_aggregate_by_taxonomy2(feature.tab, feature.ann, feature.level="Phylum")
```

### Detailed Breakdown

1. **Input Verification:** The function first ensures that a `feature.level` is provided and that it specifies a single taxonomy level.

2. **Consistency Checks:** Before proceeding with aggregation, it verifies that the row names of the OTU (`feature.tab`) and taxonomy (`feature.ann`) tables match.

3. **Data Merging:** It performs an inner join on the OTU and taxonomy tables. This step ensures only rows with consistent names across both tables are considered for aggregation.

4. **Aggregation:** Data is then aggregated based on the provided taxonomy level, with any missing taxonomy labels being replaced with the term "Unclassified".

5. **Final Cleanup:** Finally, rows in the aggregated table with all zeros (i.e., no counts) are removed.

### Practical Applications

- **Single-level Focus:** This function is perfect for users who wish to focus on a specific taxonomic level without any distractions.

- **Consistency Checks:** It ensures data integrity by comparing row names between the OTU and taxonomy tables, alerting users to discrepancies.

- **Data Reduction:** This function simplifies datasets by aggregating at the specified taxonomy level, making it more digestible for subsequent analyses.
