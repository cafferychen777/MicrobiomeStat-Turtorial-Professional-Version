---
description: >-
  A guide on converting data from DESeqDataSet format into the MicrobiomeStat
  framework.
---

# Converting DESeqDataSet into MicrobiomeStat

This guide provides a systematic approach for importing data from the DESeqDataSet format to MicrobiomeStat.

To begin, let's inspect the initial structure of our DESeqDataSet data:

<figure><img src="../../.gitbook/assets/Screenshot 2023-10-10 at 14.56.23.png" alt=""><figcaption><p>Initial structure of the DESeqDataSet data.</p></figcaption></figure>

Proceed with the steps below to conduct the conversion:

```r
# Example Code (replace with your real dataset!)
# Load necessary packages
 library(airway)
 library(DESeq2)

# Load dataset
 data("airway")
 dds <- DESeqDataSet(airway, design = ~ cell + dex)

# Convert DESeqDataSet to MicrobiomeStat data object
 data.obj <- mStat_convert_DESeqDataSet_to_data_obj(dds)
```

Upon conversion, the data structure will be transformed as follows:

<figure><img src="../../.gitbook/assets/Screenshot 2023-10-10 at 14.57.15.png" alt=""><figcaption><p>Structure of the MicrobiomeStat data object after importing DESeqDataSet data.</p></figcaption></figure>

The `mStat_convert_DESeqDataSet_to_data_obj` function facilitates the transition:

* **dds.obj**: The DESeqDataSet object slated for conversion.

After the conversion, the **MicrobiomeStat data object** consists of:

* **feature.tab**: A matrix containing count data.
* **meta.dat**: A data frame detailing the sample information.
* **feature.ann**: A matrix presenting feature annotations.

For data integrity and relevance, the function ensures only features with a sum > 0 from the counts data are retained.
