---
description: Discover the process of importing your DADA2 data into MicrobiomeStat.
---

# Integrating Data from DADA2 into MicrobiomeStat

Alright! Time to seamlessly transport your data from DADA2 to MicrobiomeStat!

Here's how to launch your DADA2 data straight into the MicrobiomeStat cosmos:

```r
# Example code
seq_tab <- readRDS(
  system.file(
    "extdata", "dada2_seqtab.rds",
    package = "microbiomeMarker"
  )
)
tax_tab <- readRDS(
  system.file(
    "extdata", "dada2_taxtab.rds",
    package = "microbiomeMarker"
  )
)
sam_tab <- read.table(
  system.file(
    "extdata", "dada2_samdata.txt",
    package = "microbiomeMarker"
  ),
  sep = "\t",
  header = TRUE,
  row.names = 1
)

# Import DADA2 data into a MicrobiomeStat data object
data_obj <- mStat_import_dada2_as_data_obj(seq_tab = seq_tab, tax_tab = tax_tab, sam_tab = sam_tab)
```

The function `mStat_import_dada2_as_data_obj` acts as your trusted **data transporter**, diligently taking care of your essential DADA2 outputs:

* **seq\_tab**: Your sequence table from DADA2. Rows symbolize samples, while columns stand for sequences (features). This table forms the backbone for your upcoming MicrobiomeStat voyage.
* **tax\_tab** (Optional): Your taxonomy assignment table, enriching your sequence data with layers of taxonomic information.
* **sam\_tab** (Optional): Your sample metadata table, akin to a treasure map guiding the exploration of your samples' metadata.
* **phy\_tree** (Optional): Your 'phylo' class phylogenetic tree, showcasing the intertwined relationships between your sequences.

Your data transition is now complete! What you've got in your hands is a **MicrobiomeStat data object**, an all-access pass to MicrobiomeStat's wide array of downstream analysis functions.

With your DADA2 data now comfortably housed in a MicrobiomeStat-compatible format, you're all set for a thrilling ride through the breathtaking vistas of microbiome data analysis. So fasten your seatbelts and enjoy the journey!
