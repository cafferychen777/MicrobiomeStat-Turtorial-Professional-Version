---
description: >-
  Find out how to bring your DGEList data into the flexible MicrobiomeStat
  framework.
---

# Loading DGEList Data into MicrobiomeStat

Elevate your microbiome data analysis by **Loading DGEList Data into MicrobiomeStat**.

```r
# Example Code (replace with your real dataset!)
# Load necessary packages
# library(airway)
# library(DESeq2)
# library(edgeR)

# Load dataset
# data("airway")
# dds <- DESeqDataSet(airway, design = ~ cell + dex)
# dge <- DGEList(counts = counts(dds), group = dds$dex)

# Convert DGEList to MicrobiomeStat data object
# data.obj <- mStat_convert_DGEList_to_data_obj(dge)
```

The `mStat_convert_DGEList_to_data_obj` function is your key to this conversion:

* **dge.obj**: Your DGEList object that needs conversion.

The function returns a **MicrobiomeStat data object**, which includes:

* **feature.tab**: A matrix with counts data.
* **meta.dat**: A data frame with samples data.

This process ensures clarity by retaining only features with a sum > 0 from your counts data. Your research deserves focus and precision!

Unlock the power of **MicrobiomeStat** and connect the puzzle pieces of your research with the `mStat_convert_DGEList_to_data_obj` function. Your next discovery awaits!&#x20;
