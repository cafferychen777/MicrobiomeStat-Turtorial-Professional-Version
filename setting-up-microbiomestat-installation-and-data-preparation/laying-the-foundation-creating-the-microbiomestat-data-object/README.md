---
description: 'description: Instructions for creating MicrobiomeStat data objects.'
---

# Building the Foundation: Creating the MicrobiomeStat Data Object

<figure><img src="../../.gitbook/assets/image (1) (1).png" alt=""><figcaption><p>Figure 1: The Structure of a MicrobiomeStat Data Object. The diagram demonstrates the relationship among the five components: 'feature.tab', 'meta.dat', 'feature.ann', the optional 'tree', and the optional 'feature.agg.list'. It clarifies how samples and features (OTU/ASV/KEGG/Gene etc.) are interconnected within these components.</p></figcaption></figure>

**Objective: Understanding the MicrobiomeStat Data Object Structure** This section outlines the core components of the MicrobiomeStat data object and provides insight into their functionality and relevance.

**Component 1: feature.tab (Matrix)** The primary matrix of the data object is the **feature.tab**. It represents the connection between research entities (**OTU/ASV/KEGG/Gene**, etc.) and **samples**. In this matrix, research entities are designated as rows, while samples are designated as columns. It's essential to ensure that the row names correspond to the research entities (**OTU/ASV/KEGG/Gene**, etc.) and the column names align with the names of the samples.

<figure><img src="../../.gitbook/assets/Screenshot 2023-10-11 at 10.35.59.png" alt=""><figcaption><p>peerj32.obj$feature.tab</p></figcaption></figure>

**Component 2: meta.dat (Data frame)** The **meta.dat** is a structured data frame that holds metadata corresponding to the samples. The rows represent the samples and they must align precisely with the columns of the feature.tab. Each column in the meta.dat acts as an **annotation** for the samples. These annotations can include various categories or factors associated with each sample. For instance:

* **Disease Severity**: This could categorize samples based on the severity of a disease, such as 'mild', 'moderate', or 'severe'.
* **Treatment Status**: It might indicate if a sample comes from a patient who has been treated or not, e.g., 'treated' or 'untreated'.
* **Study Phase**: For longitudinal studies, samples might be grouped into different time points or phases like 'phase 1', 'phase 2', 'phase 3', and 'phase 4'.

It's imperative to ensure that the row names of the meta.dat are the names of the samples and they match exactly with the column names of the feature.tab.

{% hint style="info" %}
**Note**: Tibbles are not allowed for this component. Ensure you are working with a standard R data frame.
{% endhint %}

<figure><img src="../../.gitbook/assets/Screenshot 2023-10-11 at 10.37.35.png" alt=""><figcaption><p>peerj32.obj$meta.dat</p></figcaption></figure>

**Component 3: feature.ann (Matrix)** The **feature.ann** is an annotation matrix where research entities such as **OTU/ASV/KEGG/Gene**, etc., are designated as rows. These row names must exactly match those in the feature.tab to maintain consistency in the data object. The columns of the feature.ann matrix hold **annotations** offering insights and classifications for the respective research entities.

For example:

* In **microbiome data**, typical annotations might include taxonomic classifications, ranging from **Kingdom** to **Species**.
* For **single-cell sequencing data**, this matrix could present cell type classifications, distinguishing between different cellular populations.
* In the context of **KEGG data**, the columns might represent various pathway classifications, including hierarchical levels like L1, L2, and L3.

Ensure that the row names of the feature.ann align perfectly with the row names of the feature.tab to ensure accurate and meaningful analyses.

<figure><img src="../../.gitbook/assets/Screenshot 2023-10-11 at 10.38.05.png" alt=""><figcaption><p>peerj32.obj$feature.ann</p></figcaption></figure>

**Component 4: tree (Optional)** The **phylogenetic tree** represents the evolutionary relationships among various research entities. In most situations when using MicrobiomeStat, the phylogenetic tree is not required; in fact, about 99% of MicrobiomeStat analyses can be conducted without it. However, for certain beta-diversity calculations, the phylogenetic tree becomes essential as it provides additional evolutionary context. Unless your analysis specifically requires this evolutionary perspective, you can typically proceed without this component.

<figure><img src="../../.gitbook/assets/Screenshot 2023-10-11 at 10.42.09.png" alt=""><figcaption><p>MicrobiomeData$tree</p></figcaption></figure>

**Component 5: feature.agg.list (Optional)** The **feature.agg.list** is an optional component derived from the aggregation of data in the **feature.tab** based on annotations in **feature.ann**. This aggregated data is generated using the `mStat_aggregate_by_taxonomy()` function.

The `feature.agg.list` is a list containing matrices for each level of aggregation. For example, if you aggregated the data by taxonomy, you might have entries like `$Phylum` or `$CellType`. To access a specific level of aggregation within the `feature.agg.list`, you would use syntax such as `data.obj$feature.agg.list$Phylum` or `data.obj$feature.agg.list$CellType`.

For each matrix within the `feature.agg.list`:

* The columns represent the sample names.
* The rows represent the aggregated level, such as `Phylum` or `CellType`.

<figure><img src="../../.gitbook/assets/Screenshot 2023-10-11 at 10.39.08.png" alt=""><figcaption><p>peerj32.obj$feature.agg.list</p></figcaption></figure>

It's worth noting that if you employ another function and set a `feature.level` parameter other than "original", the function will automatically call `mStat_aggregate_by_taxonomy()` to ensure the data is properly aggregated.

Together, these components comprise the MicrobiomeStat data object, which facilitates a structured and comprehensive analysis of microbiome data. After constructing the MicrobiomeStat data object, it is imperative to validate the integrity of the data object using the `mStat_validate_data(data.obj)` function. The subsequent sections will delve into the specifics of each component.
