---
description: 

---

# Creating the MicrobiomeStat Data Object

This section outlines the core components of the MicrobiomeStat data object. We use the basic list type to encapsulate the key components of a typical microbiome/omics dataset.  The use of the basic data structure in R improves the flexibility and interoperability with other tools.

**Component 1: feature.tab (matrix)** The primary matrix of the data object is the **feature.tab**. It represents the connection between research entities/features (**OTU/ASV/KEGG/Gene**, etc.) and **samples**. In this matrix, research entities are designated as rows, while samples are designated as columns. It's essential to ensure the row names (feature names) and column names (sample names ) be named properly and uniquely.

<figure><img src="../../.gitbook/assets/Screenshot 2023-10-11 at 10.35.59.png" alt=""><figcaption><p>peerj32.obj$feature.tab</p></figcaption></figure>

**Component 2: meta.dat (data.frame)** The **meta.dat** is a structured data.frame that holds metadata corresponding to the samples. The rows represent the samples and they must align precisely with the columns of the feature.tab. Each column in the meta.dat acts as an **annotation** for the samples. The column names should be informative and concise. To facilitate analysis, for continuous/numeric variables,  make sure they are of "numeric" type ("as.numeric" to convert), and for categorical variables, make sure they are of "factor" type with informative factor levels ("as.factor" to convert). It is a good practice to order the levels properly when creating a factor and always put the reference level in the first.  Level ordering is especially important for ordinal variables. For the meta data of a longitudinal dataset, we expect to see the following variables or similar:

* **subject**: a factor that indicates individual subjects or experimental units in the study. This can be alphanumeric and unique for each participant. For example, "subj001", "subj002", etc.

* **timepoint**: a factor indicating the specific time points at which measurements were taken. The order of the levels should reflect the order of the time points. For example, if measurements were taken at baseline, 6 months, and 12 months, then the levels might be ordered as "baseline", "6mo", "12mo". Yes, we do support continuous types for timepoints. For example, if the exact days or hours of measurement are known, they can be represented as numerical values such as 0, 180, 365 for days since study start.

* **group**: a factor indicating the classification of subjects, typically for comparison purposes. For example, the group variable contains the "case" and "control" status. We may put "control" in the first/reference level. By doing so, in visualization, "control" will appear before "case", and in regression analysis, the coefficient will be interpreted as the effect when the status changes from "control" to "case".

{% hint style="info" %}
**Note**: Tibbles are not allowed for this component. Ensure you are working with a standard R data.frame.
{% endhint %}

**Component 3: feature.ann (matrix)** The **feature.ann** is an annotation matrix where research entities such as **OTU/ASV/KEGG/Gene**, etc., are designated as rows. These row names must exactly match those in the feature.tab to maintain consistency in the data object. The columns of the feature.ann matrix hold **annotations** for the respective research entities/features.

For example:

* In **microbiome data**, typical annotations might include taxonomic classifications, ranging from **Kingdom** to **Species**.
* For **single-cell sequencing data**, this matrix could present cell type classifications, distinguishing between different cellular populations.
* In the context of **KEGG data**, the columns might represent various pathway classifications, including hierarchical levels like L1, L2, and L3.

<figure><img src="../../.gitbook/assets/Screenshot 2023-10-11 at 10.38.05.png" alt=""><figcaption><p>peerj32.obj$feature.ann</p></figcaption></figure>

**Component 4: tree (phylo, optional)** The **phylogenetic tree** represents the evolutionary relationships among various research entities. In most situations when using MicrobiomeStat, the phylogenetic tree is not required. However, for certain beta-diversity calculations, the phylogenetic tree becomes essential as it provides additional evolutionary context. Unless your analysis specifically requires this evolutionary perspective, you can typically proceed without this component.

<figure><img src="../../.gitbook/assets/Screenshot 2023-10-11 at 10.42.09.png" alt=""><figcaption><p>MicrobiomeData$tree</p></figcaption></figure>

**Component 5: feature.agg.list (Optional)** The **feature.agg.list** is an optional component derived from the aggregation of data in the **feature.tab** based on annotations in **feature.ann**. This aggregated data is generated using the `mStat_aggregate_by_taxonomy()` function.

The `feature.agg.list` is a list containing matrices for each level of aggregation. For example, if you aggregated the data by taxonomy, you might have entries like `$Phylum`. To access a specific level of aggregation within the `feature.agg.list`, you would use syntax such as `data.obj$feature.agg.list$Phylum`.

For each matrix within the `feature.agg.list`:

* The columns represent the sample names.
* The rows represent the aggregated level, such as `Phylum`.

<figure><img src="../../.gitbook/assets/Screenshot 2023-10-11 at 10.44.02.png" alt=""><figcaption><p>peerj32.obj$feature.agg.list$Phylum</p></figcaption></figure>

It's worth noting that if you employ another function and set a `feature.level` parameter other than "original", the function will automatically call `mStat_aggregate_by_taxonomy()` to ensure the data is properly aggregated.

Together, these components comprise the MicrobiomeStat data object, which facilitates a structured and comprehensive analysis of microbiome data. After constructing the MicrobiomeStat data object, it is imperative to validate the integrity of the data object using the `mStat_validate_data(data.obj)` function. The subsequent sections will delve into the specifics of each component.
