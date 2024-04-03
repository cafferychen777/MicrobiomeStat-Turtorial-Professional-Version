---
description: 

---

# Creating the MicrobiomeStat Data Object

This section outlines the core components of the basic MicrobiomeStat data object. We use the basic list type to encapsulate the key components of a typical microbiome/omics dataset.  The use of the basic data structure in R improves the flexibility and interoperability with other tools. In MicrobiomeStat functions, the parameter `data.obj` is designed to receive the basic MicrobiomeStat data object.

## Core Components

### Component 1: feature.tab (matrix)

The primary matrix of the data object is the **feature.tab**. It records the measurements (e.g. counts) of detected features (**OTU/ASV/KEGG/Gene**, etc.) in the samples. In this matrix, features are designated as rows, while samples are designated as columns. It's essential to ensure the row names (feature names) and column names (sample names ) be named properly and uniquely.

<figure><img src="../../.gitbook/assets/Screenshot 2023-10-11 at 10.35.59.png" alt=""><figcaption><p>peerj32.obj$feature.tab</p></figcaption></figure>

### Component 2: meta.dat (data.frame)

The **meta.dat** is a structured data.frame that holds metadata describing the sample characteristics. The rows represent the samples and they must align precisely with the columns of the feature.tab. The row names of meta.dat should be the unique sample IDs. Each column in the meta.dat acts as an **annotation** for the samples. The column names should be informative and concise. To facilitate analysis, for continuous/numeric variables,  make sure they are of "numeric" type ("as.numeric" to convert), and for categorical variables, make sure they are of "factor" type with informative factor levels ("as.factor" to convert). It is a good practice to order the levels properly when creating a factor and always put the reference level in the first.  Level ordering is especially important for ordinal variables. For the meta data of a longitudinal dataset, we expect to see the following variables or similar:

* **subject**: a factor that indicates individual subjects or experimental units in the study. This can be alphanumeric and unique for each participant. For example, "subj001", "subj002", etc. In a longitudinal study, each subject could have multiple samples collected over time. Therefore, the number of unique subjects should be less than the total number of samples. Ideally, each subject should have the same time points. In reality, however, not all subjects may have samples at every time point. Some subjects might miss certain sampling time points due to various reasons such as dropout, missed visits, or sample quality issues.  MicrobiomeStat can handle such unbalanced longitudinal data, where not all subjects have samples at every time point. Make sure that each sample (row) in your meta.dat has a valid subject identifier and a corresponding time point. 

* **timepoint**: a factor indicating the specific time points at which measurements were taken. The order of the levels should reflect the order of the time points. For example, if measurements were taken at baseline, 6 months, and 12 months, then the levels might be ordered as "baseline", "6mo", "12mo". MicrobiomeStat also supports continuous time point. For example, if the exact days or hours of measurement are known, they can be represented as numerical values such as 0, 180, 365 for days since the study starts.

* **group**: a factor indicating the grouping of subjects, typically for comparison purposes. For example, the group variable contains the "case" and "control" status. We may put "control" in the first/reference level. By doing so, in visualization, "control" will appear before "case", and in regression analysis, the coefficient will be interpreted as the effect when the status changes from "control" to "case".

*  Other variables: the variables could be time-varying (sample-level characteristics) or non-time-varying (subject-level characteristics).  

{% hint style="info" %}
**Note**: Tibbles are not allowed for this component. Ensure you are working with a standard R data.frame.
{% endhint %}

### Component 3: feature.ann (matrix)

The **feature.ann** is an annotation matrix where features such as **OTU/ASV/KEGG/Gene**, etc., are designated as rows. These row names must exactly match those in the feature.tab to maintain consistency in the data object. The columns of the feature.ann matrix hold **annotations** for the respective research entities/features.

For example:

* In **microbiome data**, typical annotations might include taxonomic classifications, ranging from **Kingdom** to **Species**.
* For **single-cell sequencing data**, this matrix could contain cell type classifications at different granularities.
* In the context of **KEGG data**, the columns might represent various pathway classifications at different hierarchical levels like L1, L2, and L3.

<figure><img src="../../.gitbook/assets/Screenshot 2023-10-11 at 10.38.05.png" alt=""><figcaption><p>peerj32.obj$feature.ann</p></figcaption></figure>

### Component 4: tree (phylo, optional)

The **phylogenetic tree** represents the evolutionary relationships among the features. In most situations when using MicrobiomeStat, the phylogenetic tree is not required. However, for certain beta-diversity calculations, the phylogenetic tree becomes essential as it provides additional evolutionary context. Unless your analysis specifically requires this evolutionary perspective, you can typically proceed without this component.

<figure><img src="../../.gitbook/assets/Screenshot 2023-10-11 at 10.42.09.png" alt=""><figcaption><p>MicrobiomeData$tree</p></figcaption></figure>

### Component 5: feature.agg.list (Optional)

The **feature.agg.list** is an optional component derived from the aggregation of data in the **feature.tab** based on annotations in **feature.ann**. This aggregated data is generated using the `mStat_aggregate_by_taxonomy()` function.

The `feature.agg.list` is a list containing matrices for each level of aggregation. For example, if you aggregated the data by taxonomy, you might have entries like `$Phylum`. To access a specific level of aggregation within the `feature.agg.list`, you would use syntax such as `data.obj$feature.agg.list$Phylum`.

For each matrix within the `feature.agg.list`:

* The columns represent the sample names.
* The rows represent the aggregated level, such as `Phylum`.

<figure><img src="../../.gitbook/assets/Screenshot 2023-10-11 at 10.44.02.png" alt=""><figcaption><p>peerj32.obj$feature.agg.list$Phylum</p></figcaption></figure>

It's worth noting that if you call a MicrobiomeStat function and set a `feature.level` parameter other than "original", the function will automatically call `mStat_aggregate_by_taxonomy()` to ensure the data is properly aggregated.

Together, these components comprise the MicrobiomeStat data object, which facilitates a structured representation of microbiome data. After constructing the MicrobiomeStat data object, it is imperative to validate the integrity of the data object using the `mStat_validate_data` function. 

## Specialized Data Objects

In addition to the basic data object, the MicrobiomeStat toolkit accepts specialized data objects for specific analyses, such as alpha and beta diversity analysis.  These specialized data objects can be passed to the specific parameters (arguments) in relevant MicrobiomeStat functions as detailed below.

### alpha.obj

`alpha.obj`: For functions performing alpha diversity analysis, the `alpha.obj` parameter accepts a data object that encapsulates various alpha diversity measures of the samples (matrix type, rows: samples, columns: alpha diversity measures).  The `mStat_calculate_alpha_diversity` function calculates the common alpha diversity measures ("shannon", "simpson", "observed_species", "chao1", "ace", "pielou"). If no data object is provided to `alpha.obj` where needed, the function will be called automatically. However, to expedite the analysis process, it is advisable to pre-calculate the alpha diversity indices and store them in a properly named data object for future repeated use.

It is important to note that `mStat_calculate_alpha_diversity` does not perform data rarefaction, a process that equalizes the sequencing depth across samples so that the alpha diversity measures (esp. species richness) are not influenced by sequencing depth variation. It is always recommended that the users perform rarafaction using `mStat_rarefy_data` before calculating the alpha diversity.

### dist.obj

`dist.obj`: For functions performing beta diversity (distance) analysis, the `dist.obj` parameter accepts a data object containing various beta diversity measures in the form of pairwise distances (list type, each element a distance matrix). The `mStat_calculate_beta_diversity` function calculates the common beta diversity measures ("BC" (Bray-Curtis), "Jaccard", "UniFrac" (unweighted), "GUniFrac" (generalized), "WUniFrac" (weighted), and "JS" (Jensen-Shannon divergence)). If we need to remove the effects of some covariates on beta diversity measures, the `mStat_calculate_adjusted_distance` function can be utilized. This adjustment, based on linear models and multidimensional scaling, removes sources of unwanted variation, ensuring that the identified microbial patterns are not confounded by these covariates.  Similar to creating the alpha diversity object, rarefaction is highly recommended before calculating the beta diversity since these unweighted beta diversity measures are highly influenced by sequencing depth variation.

### pc.obj

`pc.obj`:  For functions performing beta diversity analysis based on the principal coordinates (PC) of distance matrices,  the `pc.obj` parameter accepts a data object containing the PCs of various distance matrices (list type, each element is a matrix with rows representing samples, columns representing PCs). The `mStat_calculate_PC` function can be used to generate the PC object.  This function allows for the choice between "mds" (multidimensional scaling) and "nmds" (non-metric multidimensional scaling) for ordination methods. If no PC object is passed to `pc.obj`, `mStat_calculate_PC` function will be automatically invoked using the default "mds". For users who prefer more advanced ordination methods, such as t-SNE or UMAP, these can be computed using external tools and then formatted to fit the MicrobiomeStat data structure for compatibility with the MicrobiomeStat toolkit.



With these essential data objects,  the MicrobiomeStat toolkit is equipped to handle a wide range of analyses, from diversity assessments to complex ordination techniques, providing users with comprehensive tools for microbiome data analysis.
