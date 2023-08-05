---
description: Understand how to import your BIOM data into MicrobiomeStat's data structure.
---

# Transferring Data from BIOM into MicrobiomeStat

Here's how you can bring your BIOM data into MicrobiomeStat!

{% code fullWidth="true" %}
```r
# Example code
rich_dense_biom <- system.file("extdata", "rich_dense_otu_table.biom",  package="phyloseq")
treefilename <- system.file("extdata", "biom-tree.phy",  package="phyloseq")
refseqfilename <- system.file("extdata", "biom-refseq.fasta",  package="phyloseq")

# Convert BIOM data into a MicrobiomeStat data object
data_obj <- mStat_import_biom_as_data_obj(rich_dense_biom, treefilename, refseqfilename, parseFunction=parse_taxonomy_greengenes)
```
{% endcode %}

The `mStat_import_biom_as_data_obj` function takes your BIOM (Biological Observation Matrix) file and breathes life into it by creating a **MicrobiomeStat data object**. Here's the essentials you need:

* **BIOMfilename**: Your BIOM file, a treasure trove of your biological observation matrix and related metadata.
* **treefilename** (Optional): Your phylogenetic tree file, which encapsulates the evolutionary relationships among your observed species.
* **parseFunction** (Optional): Your custom function to parse the taxonomy from the metadata in your BIOM file. You can leverage 'parse\_taxonomy\_default' as your starting point!

Once your BIOM data is imported into the **MicrobiomeStat data object**, you're all set to harness the powerful suite of **MicrobiomeStat's statistical analysis tools**! Whether you have a phylogenetic tree or not, your journey into microbiome data analysis can smoothly set sail!

Embark on your exciting exploration now! Your voyage through the fascinating world of microbiome data analysis is about to commence!
