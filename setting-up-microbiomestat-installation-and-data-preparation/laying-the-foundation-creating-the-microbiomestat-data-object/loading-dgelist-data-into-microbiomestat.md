---
description: >-
  Learn the steps to efficiently import DGEList data structures into
  MicrobiomeStat for streamlined analysis.
---

# Converting DGEList Data into MicrobiomeStat

This guide will assist you in transferring data from the DGEList format to MicrobiomeStat.

Before we proceed with the conversion, let's examine the original structure of our DGEList data:

<figure><img src="../../.gitbook/assets/Screenshot 2023-10-10 at 14.45.38.png" alt=""><figcaption><p>Original structure of the DGEList data.</p></figcaption></figure>

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

<figure><img src="../../.gitbook/assets/Screenshot 2023-10-10 at 14.46.16.png" alt=""><figcaption><p>Structure of the MicrobiomeStat data object after importing DGEList data.</p></figcaption></figure>

By using the `mStat_convert_DGEList_to_data_obj` function, you can effectively convert:

* **dge.obj**: Your existing DGEList object.

The resultant **MicrobiomeStat data object** includes:

* **feature.tab**: A matrix populated with counts data.
* **meta.dat**: A data frame detailing the sample information.

As part of the conversion process, features with a sum of zero from the counts data are filtered out to maintain data relevance and accuracy.
