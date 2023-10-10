---
description: Guide to importing data in BIOM format into MicrobiomeStat's structure.
---

# Transferring Data from BIOM into MicrobiomeStat

# Importing BIOM Data to MicrobiomeStat

Follow the instructions below to convert your BIOM data into a format suitable for MicrobiomeStat.

{% file src="../../.gitbook/assets/rich_dense_otu_table.biom" %}
**Caption:** BIOM file containing biological observation matrix and associated metadata.

{% file src="../../.gitbook/assets/biom-tree.phy" %}
**Caption:** Phylogenetic tree file representing evolutionary relationships among observed species.

{% code fullWidth="true" %}
```r
# Example code
rich_dense_biom <- "rich_dense_otu_table.biom"
treefilename <- "biom-tree.phy"

# Convert BIOM data into a MicrobiomeStat data object
data.obj <- mStat_import_biom_as_data_obj(rich_dense_biom, treefilename, parseFunction=parse_taxonomy_greengenes)
```
{% endcode %}

<figure><img src="../../.gitbook/assets/Screenshot 2023-10-10 at 15.18.52.png" alt=""><figcaption>Structure of the MicrobiomeStat data object created from BIOM data.</figcaption></figure>

By employing the `mStat_import_biom_as_data_obj` function, the BIOM data, along with optional phylogenetic tree data, can be integrated into the MicrobiomeStat framework. Here's a brief overview of the function's parameters:

* **BIOMfilename**: The file path to your BIOM data.
* **treefilename** (Optional): The file path to your phylogenetic tree.
* **parseFunction** (Optional): A user-defined function to interpret the taxonomy from the BIOM file metadata.

With the conversion complete, the data is now ready to be analyzed using the various tools provided by MicrobiomeStat.