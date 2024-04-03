---
description: >-
  A step-by-step guide on converting SummarizedExperiment data format for
  compatibility with MicrobiomeStat.
---

# Converting SummarizedExperiment into MicrobiomeStat

The Bioconductor package `SummarizedExperiment` provides a convenient representation of experimental data.  MicrobiomeStat provides a straightforward method for converting SummarizedExperiment objects.

```r
# Check if the 'airway' package is installed
if (!requireNamespace("airway", quietly = TRUE)) {
  # If not installed, install 'airway'
  BiocManager::install("airway")
}

# Load necessary libraries
library(airway)

# Use the provided 'airway' dataset as an example
data(airway)

# Convert the SummarizedExperiment object 'airway' into a MicrobiomeStat data object
airway_obj <- mStat_convert_SummarizedExperiment_to_data_obj(airway)
```

<figure><img src="../../.gitbook/assets/Screenshot 2023-10-10 at 11.50.07.png" alt=""><figcaption><p>Structure of the <code>airway_obj</code> data object, converted from the <code>airway</code> SummarizedExperiment dataset. The visualization showcases the primary components of the MicrobiomeStat data object, detailing its matrices and data frames derived from the original dataset.</p></figcaption></figure>

The function `mStat_convert_SummarizedExperiment_to_data_obj` requires:

* **se.obj**: A SummarizedExperiment object for conversion.

Post conversion, the data is organized into a **MicrobiomeStat data object** with the following components:

* **feature.tab**: A matrix derived from assay data.
* **feature.ann**: A matrix consisting of rowData or feature annotations. Only the features present in the assay data are retained.
* **meta.dat**: A data frame containing colData or sample metadata.

For efficient analysis, features with an aggregate count of zero in the assay data are excluded during the conversion process.


