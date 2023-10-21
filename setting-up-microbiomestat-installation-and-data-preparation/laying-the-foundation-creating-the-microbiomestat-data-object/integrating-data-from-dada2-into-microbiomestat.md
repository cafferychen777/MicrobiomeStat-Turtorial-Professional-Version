---
description: >-
  Learn the steps to import data from DADA2 into MicrobiomeStat's data
  structure.
---

# Importing Data from DADA2 into MicrobiomeStat

This guide will walk you through the steps required to convert and integrate data from DADA2 into MicrobiomeStat.

### Download Necessary Files

{% file src="../../.gitbook/assets/dada2_seqtab.rds" %}
Sequence table (`seq_tab`) - This table from DADA2 contains sample-specific sequence abundance data. Rows represent samples, and columns correspond to sequences.
{% endfile %}

{% file src="../../.gitbook/assets/dada2_taxtab.rds" %}
Taxonomy assignment table (`tax_tab`) - This table provides taxonomic classifications for sequences, helping users understand the biological context of the identified sequences.
{% endfile %}

{% file src="../../.gitbook/assets/dada2_samdata.txt" %}
Sample metadata table (\`sam\_tab\`) - This table offers detailed metadata for each sample, enriching the context and facilitating more in-depth analyses.
{% endfile %}

### Code to Import Data

```r
# This function requires the `Biostrings` and `yaml` packages.
 
# If you encounter issues when running the example code, please ensure that you have installed and loaded these packages.
library(Biostrings)
library(yaml)
# Read the sequence table from the downloaded RDS file
seq_tab <- readRDS("path_to_your_downloaded_folder/dada2_seqtab.rds")

# Read the taxonomy table from the downloaded RDS file
tax_tab <- readRDS("path_to_your_downloaded_folder/dada2_taxtab.rds")

# Read the sample metadata from the downloaded txt file
sam_tab <- read.table(
  "path_to_your_downloaded_folder/dada2_samdata.txt",
  sep = "\t",
  header = TRUE,
  row.names = 1
)

# Convert DADA2 data into a MicrobiomeStat data object
data_obj <- mStat_import_dada2_as_data_obj(
  seq_tab = seq_tab,
  tax_tab = tax_tab,
  sam_tab = sam_tab
)
```

<figure><img src="../../.gitbook/assets/Screenshot 2023-10-10 at 14.34.05.png" alt=""><figcaption><p>Structure of the MicrobiomeStat data object after importing DADA2 data. This visualization highlights the different components of the data object, including the feature table, taxonomy table, and sample metadata, facilitating a comprehensive understanding of the imported dataset.</p></figcaption></figure>

{% hint style="info" %}
Note: Ensure to replace `"path_to_your_downloaded_folder"` with the actual path where you've saved the downloaded files.
{% endhint %}
