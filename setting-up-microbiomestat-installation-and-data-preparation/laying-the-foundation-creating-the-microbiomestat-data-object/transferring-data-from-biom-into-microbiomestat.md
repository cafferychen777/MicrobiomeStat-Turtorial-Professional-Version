---
description: Guide to importing data in BIOM format into MicrobiomeStat's structure.
---

# Importing Data from BIOM into MicrobiomeStat

Follow the instructions below to convert your BIOM data into a format suitable for MicrobiomeStat.

{% file src="../../.gitbook/assets/rich_dense_otu_table.biom" %}
BIOM file containing biological observation matrix and associated metadata.
{% endfile %}

{% file src="../../.gitbook/assets/biom-tree.phy" %}
Phylogenetic tree file representing evolutionary relationships among observed species.
{% endfile %}

{% code fullWidth="true" %}
```r
# Example code

# Specify the path to your BIOM data. Replace the placeholder with the correct path on your system.
rich_dense_biom <- "rich_dense_otu_table.biom"
# Reminder: Ensure that you replace the above path with the actual location of your BIOM file.

# Specify the path to your phylogenetic tree. Again, replace the placeholder with the correct path on your system.
treefilename <- "biom-tree.phy"
# Reminder: Ensure that you replace the above path with the actual location of your phylogenetic tree file.

# Convert the BIOM data and associated phylogenetic tree (if provided) 
# into a format suitable for MicrobiomeStat using the mStat_import_biom_as_data_obj function.
data.obj <- mStat_import_biom_as_data_obj(biom_filename = rich_dense_biom, tree_filename = treefilename)

# The resulting 'data.obj' is now a MicrobiomeStat data object that you can use for further analyses.
```
{% endcode %}

<figure><img src="../../.gitbook/assets/Screenshot 2023-10-10 at 15.18.52.png" alt=""><figcaption><p>Structure of the MicrobiomeStat data object created from BIOM data.</p></figcaption></figure>

By employing the `mStat_import_biom_as_data_obj` function, the BIOM data, along with optional phylogenetic tree data, can be integrated into the MicrobiomeStat framework. Here's a brief overview of the function's parameters:

* **BIOMfilename**: The file path to your BIOM data.
* **treefilename** (Optional): The file path to your phylogenetic tree.
* **parseFunction** (Optional): A user-defined function to interpret the taxonomy from the BIOM file metadata.

