---
description: Learn to seamlessly import data from QIIME2 into MicrobiomeStat's data object.
---

# Importing Data from QIIME2 into MicrobiomeStat

Here's how you import your Qiime2 data into MicrobiomeStat!

```r
# Example code
otuqza_file <- system.file("extdata", "table.qza", package = "microbiomeMarker")
taxaqza_file <- system.file("extdata", "taxonomy.qza", package = "microbiomeMarker")
sample_file <- system.file("extdata", "sample-metadata.tsv", package = "microbiomeMarker")
treeqza_file <- system.file("extdata", "tree.qza", package = "microbiomeMarker")

# Import QIIME2 data into a MicrobiomeStat data object
data_obj <- mStat_import_qiime2_as_data_obj(
    otu_qza = otuqza_file, taxa_qza = taxaqza_file,
    sam_tab = sample_file, tree_qza = treeqza_file,
)
```

The `mStat_import_qiime2_as_data_obj` function transforms your Qiime2 data into **MicrobiomeStat-friendly format**, setting you up for your exploration journey through the microbial universe. Here's what you need:

* **otu\_qza**: Your feature table, offering a snapshot of your microbial community.
* **taxa\_qza** (Optional): Your taxonomy assignment table, providing taxonomic identities to your microbial species.
* **sam\_tab** (Optional): Your sample metadata table, illuminating the conditions and characteristics of your samples.
* **refseq\_qza** (Optional): Your representative sequences table, featuring the most common sequences in your community.
* **tree\_qza** (Optional): Your phylogenetic tree, illustrating the evolutionary relationships amongst your microbial species.

Your Qiime2 data is now equipped for a journey through the MicrobiomeStat landscape. The MicrobiomeStat data object, created by the transformation, can directly dock into the **MicrobiomeStat's analytical tools**, offering you an extensive suite of options for microbiome data exploration.

Buckle up, explorers! Your journey into the mesmerizing world of microbiome data analysis starts here!&#x20;
