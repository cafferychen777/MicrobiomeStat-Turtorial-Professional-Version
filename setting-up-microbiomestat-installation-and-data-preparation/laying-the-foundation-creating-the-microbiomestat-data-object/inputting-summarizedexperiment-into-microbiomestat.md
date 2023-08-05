---
description: >-
  Understand the process to import SummarizedExperiment data into
  MicrobiomeStat.
---

# Inputting SummarizedExperiment into MicrobiomeStat

Here's how to streamline your microbiome data analysis by **Inputting SummarizedExperiment into MicrobiomeStat**.

```r
# Example Code (replace with your real dataset!)
# Assuming 'se' is your SummarizedExperiment object
# data_obj <- mStat_convert_SummarizedExperiment_to_data_obj(se)

# If you have airway data available as a SummarizedExperiment object
# you can convert it to a MicrobiomeStat data object using:
 library(airway)
 data(airway)
 airway_obj <- mStat_convert_SummarizedExperiment_to_data_obj(airway)
```

Take control of your research with the `mStat_convert_SummarizedExperiment_to_data_obj` function:

* **se.obj**: Your SummarizedExperiment object that's ready for conversion.

This function transforms your data into a **MicrobiomeStat data object** that includes:

* **feature.tab**: A matrix with assay data.
* **feature.ann**: A matrix with rowData (feature annotations). Only features in the assay data are included.
* **meta.dat**: A data frame with colData (metadata).

Stay focused on your research by retaining only features with a sum > 0 from your assay data.

The `mStat_convert_SummarizedExperiment_to_data_obj` function is your key to unlocking the power of **MicrobiomeStat**. Begin your data conversion journey and get ready for the discoveries ahead!&#x20;
