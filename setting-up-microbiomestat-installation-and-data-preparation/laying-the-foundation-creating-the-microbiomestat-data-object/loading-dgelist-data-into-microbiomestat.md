---
description: >-
  Learn the steps to efficiently import DGEList data structures into MicrobiomeStat for streamlined analysis.
---

# Importing DGEList Data into MicrobiomeStat

This guide will assist you in transferring data from the DGEList format to MicrobiomeStat.

Before we proceed with the conversion, let's examine the original structure of our DGEList data:

Now, follow the steps below to convert the DGEList data:

```r
# Load necessary packages
library(airway)
library(DESeq2)
library(edgeR)

# Load dataset
data("airway")
dds <- DESeqDataSet(airway, design = ~ cell + dex)
dge <- DGEList(counts = counts(dds), group = dds$dex)

# Convert DGEList to MicrobiomeStat data object
data.obj <- mStat_convert_DGEList_to_data_obj(dge)
```

After conversion, let's take a look at the transformed data structure:

By using the `mStat_convert_DGEList_to_data_obj` function, you can effectively convert:

* **dge.obj**: Your existing DGEList object.

The resultant **MicrobiomeStat data object** encompasses:

* **feature.tab**: A matrix populated with counts data.
* **meta.dat**: A data frame detailing the sample information.

As part of the conversion process, features with a sum of zero from the counts data are filtered out to maintain data relevance and accuracy.

By integrating DGEList data into the MicrobiomeStat framework, researchers can harness the comprehensive analysis tools provided by MicrobiomeStat, further enhancing their data-driven insights and findings.&#x20;
