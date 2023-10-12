---
description: >-
  In this section, we will cover the key function in MicrobiomeStat for
  combining multiple microbiome datasets into one object for integrated
  analysis.
---

# Data Combination

## Data Combination

Combining multiple microbiome datasets is a crucial step in many research projects. The `mStat_combine_data()` function in MicrobiomeStat provides a straightforward way to merge two datasets into one object for integrated analysis.

### Overview

`mStat_combine_data()` is designed specifically for combining two MicrobiomeStat data objects that are in the raw format.

**Each data object should contain:**

* `feature.tab`: OTU/ASV table
* `meta.dat`: Sample metadata
* `feature.ann`: Taxonomic annotations

**The function will:**

* Row-bind the two `feature.tab` matrices
* Row-bind the two `feature.ann` matrices
* Row-bind the two `meta.dat` data frames

The output is a single merged data object ready for integrated analysis.

### Usage

```{r
mStat_combine_data(
  data.obj1 = obj1, 
  data.obj2 = obj2
)
```

* `data.obj1`: The first data object to combine
* `data.obj2`: The second data object to combine

### Details

Here is how `mStat_combine_data()` works under the hood:

First it checks that both input objects are in the raw MicrobiomeStat format.

It then identifies common features and samples between the two objects.

* For common features, it checks that the data values are consistent between the two objects.
* If no common features or samples are found, it will print a warning message.

Next, it performs:

* A full join of the two `feature.tab` matrices by feature IDs
* Replaces any NA values with 0
* Gathers into a long format
* Spreads back to wide format
* Sets feature IDs as row names

This yields the combined `feature.tab` matrix.

It performs similar operations to combine `feature.ann` and `meta.dat`.

Finally, it returns the merged data object containing all three components.

### Applications

Key applications of this function include:

* Merge case and control groups for differential abundance analysis
* Combine multiple cohorts for meta-analysis
* Compile temporal datasets for longitudinal analysis
* Create one large dataset from multiple studies of a population

Proper dataset integration is crucial for maximizing the potential of your microbiome research. `mStat_combine_data()` lets you seamlessly merge compatible data objects, enabling more powerful integrated analysis.
