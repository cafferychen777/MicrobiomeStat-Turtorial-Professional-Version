---
description: >-
  This guide provides a step-by-step method for importing QIIME2 data structures
  into the MicrobiomeStat data object.
---

# Importing Data from QIIME2 into MicrobiomeStat

To effectively utilize the capabilities of MicrobiomeStat, it's essential to ensure your data is in a compatible format. This guide details the process to convert QIIME2 data structures into MicrobiomeStat's required data object.

The provided files represent essential QIIME2 artifacts and metadata required for microbiome data analysis:

{% file src="../../.gitbook/assets/table.qza" %}
This artifact contains the feature table, which is a summary of the observed microbial features (e.g., OTUs, ASVs) across different samples. QIIME2 users typically recognize it as their primary count or abundance matrix.
{% endfile %}

{% file src="../../.gitbook/assets/taxonomy.qza" %}
This artifact holds taxonomic assignments for each microbial feature, allowing users to determine the classification of observed features at various taxonomic ranks (e.g., kingdom, phylum, class).
{% endfile %}

{% file src="../../.gitbook/assets/sample-metadata.tsv" %}
A tab-separated values (TSV) file detailing the metadata associated with each sample. Metadata might include sample conditions, treatments, or any other annotations pertinent to the study.
{% endfile %}

{% file src="../../.gitbook/assets/tree.qza" %}
A phylogenetic tree artifact representing the evolutionary relationships between microbial features. This tree is crucial for various phylogenetic diversity metrics and analyses within QIIME2.
{% endfile %}

```r
# Define paths to your QIIME2 files
# NOTE: Update these paths to point to the location of the files on your computer.
otuqza_file <- "path_to_your_directory/table.qza"
taxaqza_file <- "path_to_your_directory/taxonomy.qza"
sample_file <- "path_to_your_directory/sample-metadata.tsv"
treeqza_file <- "path_to_your_directory/tree.qza"

# Import QIIME2 data into a MicrobiomeStat data object
data_obj <- mStat_import_qiime2_as_data_obj(
    otu_qza = otuqza_file, 
    taxa_qza = taxaqza_file,
    sam_tab = sample_file, 
    tree_qza = treeqza_file
)
```

<figure><img src="../../.gitbook/assets/Screenshot 2023-10-10 at 11.35.49.png" alt=""><figcaption><p>This output showcases the data paths and structure of the converted <code>data_obj</code> from QIIME2 to MicrobiomeStat format. Displayed paths indicate the location of QIIME2 files on a local machine. The <code>str(data_obj)</code> function in R reveals the organized structure of the <code>data_obj</code> list, detailing its various components such as <code>feature.tab</code>, <code>meta.dat</code>, <code>feature.ann</code>, and the phylogenetic <code>tree</code>. Each component houses relevant microbiome data ready for analysis using MicrobiomeStat.</p></figcaption></figure>

The function `mStat_import_qiime2_as_data_obj` efficiently transforms QIIME2 datasets into the data format required by MicrobiomeStat. Here's a breakdown of the input parameters:

* **otu\_qza**: This file represents your feature table, containing the abundance data of microbial entities across samples.
* **taxa\_qza** (Optional): This is the taxonomy assignment table which provides taxonomic classifications for the microbial entities.
* **sam\_tab** (Optional): The sample metadata table offers details about the conditions, treatments, or any other pertinent annotations related to the samples.
* **tree\_qza** (Optional): A phylogenetic tree depicting the evolutionary relationships of the microbial entities.

Upon successful conversion, the generated MicrobiomeStat data object can be directly used with the suite of analysis tools provided by MicrobiomeStat, ensuring a seamless and efficient analytical workflow.

Ensure that you have the correct file paths and all required dependencies installed before initiating the conversion process.
