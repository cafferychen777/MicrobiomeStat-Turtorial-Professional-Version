---
description: Learn the steps to import data from DADA2 into MicrobiomeStat's data structure.
---

# Importing Data from DADA2 into MicrobiomeStat

This guide will walk you through the steps required to convert and integrate data from DADA2 into MicrobiomeStat.

### Download Necessary Files

{% file src="../../.gitbook/assets/dada2_seqtab.rds" %}
**Caption:** Sequence table (`seq_tab`) - This table from DADA2 contains sample-specific sequence abundance data. Rows represent samples, and columns correspond to sequences.

{% file src="../../.gitbook/assets/dada2_taxtab.rds" %}
**Caption:** Taxonomy assignment table (`tax_tab`) - This table provides taxonomic classifications for sequences, helping users understand the biological context of the identified sequences.

{% file src="../../.gitbook/assets/dada2_samdata.txt" %}
**Caption:** Sample metadata table (`sam_tab`) - This table offers detailed metadata for each sample, enriching the context and facilitating more in-depth analyses.

### Code to Integrate Data

```r
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

By following the above steps, the DADA2 data gets structured into a MicrobiomeStat data object. This object is now ready to be used with the various analysis functionalities provided by MicrobiomeStat.

Note: Ensure to replace `"path_to_your_downloaded_folder"` with the actual path where you've saved the downloaded files.