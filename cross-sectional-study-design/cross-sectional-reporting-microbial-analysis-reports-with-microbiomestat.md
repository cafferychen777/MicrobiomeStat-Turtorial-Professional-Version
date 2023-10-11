---
description: >-
  MicrobiomeStat offers automated analysis reporting for cross-sectional studies
  in microbiome research. It generates comprehensive reports covering alpha
  diversity, beta diversity, and taxonomic compos
---

# Cross-Sectional Reporting: Microbial Analysis Reports with MicrobiomeStat

Microbiome research yields complex multidimensional datasets. These encompass:

* Alpha diversity: Represents within-sample taxonomic diversity.
* Beta diversity: Captures between-sample taxonomic dissimilarity.
* Differential abundance: Pinpoints differentially abundant taxa across experimental conditions.

Manually analyzing these dimensions is not only tedious but prone to inconsistencies.

MicrobiomeStat offers an efficient approach, generating integrated reports for cross-sectional studies. These reports encompass:

* Visualizations to illustrate key findings
* Statistical summaries highlighting the main outcomes

This ensures a thorough and consistent interpretation of data.

Central to this process is the `mStat_generate_report_single()` function. It conducts:

* **Alpha diversity** analyses: These utilize functions like `mStat_calculate_alpha_diversity()`, `generate_alpha_boxplot_single()`, and `generate_alpha_test_single()`.
* **Beta diversity** analyses: Leveraging functions such as `mStat_calculate_beta_diversity()`, `generate_beta_ordination_single()`, and `generate_beta_test_single()`.
* **Taxonomic composition** analyses: Employing `generate_taxa_barplot_single()`, `generate_taxa_dotplot_single()`, `generate_taxa_heatmap_single()`, and `generate_taxa_test_single()`.

The function then assembles these analyses into a cohesive PDF report, featuring:

* A data summary from `mStat_summarize_data_obj()`
* Visual representations like alpha diversity boxplots, beta diversity ordination plots, and taxa composition visualizations
* Tables detailing statistical test results
* Commentaries on key findings

Before we implement the function, let's understand the parameters we will be using:

* `data.obj`: A list object in a specific format to MicrobiomeStat, which can include components such as feature.tab (matrix), feature.ann (matrix), meta.dat (data.frame), tree, and feature.agg.list (list). This object can be converted from other formats using several functions from the MicrobiomeStat package, or constructed manually. For more detailed information on how to convert data from other formats or how to construct the `data.obj` manually, please refer to the following page.

{% content-ref url="../setting-up-microbiomestat-installation-and-data-preparation/laying-the-foundation-creating-the-microbiomestat-data-object/" %}
[laying-the-foundation-creating-the-microbiomestat-data-object](../setting-up-microbiomestat-installation-and-data-preparation/laying-the-foundation-creating-the-microbiomestat-data-object/)
{% endcontent-ref %}

* `group.var`: Variable name used for grouping samples.
* `test.adj.vars`: Names of columns in the metadata containing covariates to be adjusted for in statistical tests and models. Default is NULL, which indicates no covariates are adjusted for in statistical testing.
* `vis.adj.vars`: Names of columns in the metadata containing covariates to visualize in plots, in addition to the primary variables of interest such as groups. Default is NULL, which indicates only the primary variables of interest will be visualized without additional covariates.
* `strata.var`: Variable to stratify the analysis by (optional).
* `subject.var`: Variable name used for subject identification.
* `time.var`: Variable name used for time points.
* `alpha.obj`: An optional list containing pre-calculated alpha diversity indices. If NULL (default), alpha diversity indices will be calculated using `mStat_calculate_alpha_diversity` function.
* `alpha.name`: The alpha diversity index to be plotted. Supported indices include "shannon", "simpson", "observed\_species", "chao1", "ace", and "pielou".
* `depth`: An integer. The sequencing depth to be used for the "Rarefy" and "Rarefy-TSS" methods. If NULL, the smallest total count across samples is used as the rarefaction depth.
* `dist.obj`: Distance matrix between samples, usually calculated using `mStat_calculate_beta_diversity` function. If NULL, beta diversity will be automatically computed from `data.obj` using `mStat_calculate_beta_diversity`.
* `dist.name`: A character vector specifying which beta diversity indices to calculate. Supported indices are "BC" (Bray-Curtis), "Jaccard", "UniFrac" (unweighted UniFrac), "GUniFrac" (generalized UniFrac), "WUniFrac" (weighted UniFrac), and "JS" (Jensen-Shannon divergence).
* `vis.feature.level`: The column name in the feature annotation matrix (feature.ann) of data.obj to use for visualization and plotting. This can be a taxonomic level like "Phylum" or "Genus" for microbiome data. For single-cell data, this could be a cell type identifier such as "CellType". For KEGG data, this could be a pathway level such as "Pathway_L1", "Pathway_L2", or "Pathway_L3". If you want to avoid aggregation, you can set it to "original", and no aggregation will be performed. The selected feature level will be used to aggregate the data at the specified level in the generated visualizations.
* `test.feature.level`: The column name in the feature annotation matrix (feature.ann) of data.obj to use for testing or analytical purposes. This can be a taxonomic level like "Phylum" or "Genus" for microbiome data. For single-cell data, this could be a cell type identifier such as "CellType". For KEGG data, this could be a pathway level such as "Pathway_L1", "Pathway_L2", or "Pathway_L3". If you want to avoid aggregation, you can set it to "original", and no aggregation will be performed. The selected feature level will be used to aggregate the data at the specified level for statistical tests and models.
* `feature.dat.type`: The type of the feature data, which determines how the data is handled in downstream analyses. Should be one of: "count": Raw count data, will be normalized by the function. "proportion": Data that has already been normalized to proportions/percentages. "other": Custom abundance data that has unknown scaling. No normalization applied.
* `feature.box.axis.transform`: A string indicating the transformation to apply to the data before plotting. Options are: 
  * "identity": No transformation (default),
  * "sqrt": Square root transformation,
  * "log": Logarithmic transformation.
  In the function `mStat_generate_report_single`, this parameter is only used in `generate_taxa_boxplot_single` and `generate_taxa_indiv_boxplot_single`. In other functions, it is also used solely to adjust boxplots related to feature functions.
* `feature.mt.method`: Character, multiple testing method for features, "fdr" or "none", default is "fdr".
* `feature.sig.level`: Numeric, significance level cutoff for highlighting features, default is 0.1.
* `base.size`: Base font size for the generated plots.
* `theme.choice`: Plot theme choice. Can be one of: "prism": ggprism::theme\_prism(), "classic": theme\_classic(), "gray": theme\_gray(), "bw": theme\_bw().
* `output.file`: A character string specifying the output file name for the report. This will also determine the title of the generated report. For example, if you set it to "path_to_your_location/report.pdf", the title of the report will be "report".

Now, let's see how we can implement the function:

```r
 mStat_generate_report_single(
   data.obj = peerj32.obj,
   dist.obj = NULL,
   alpha.obj = NULL,
   group.var = "group",
   vis.adj.vars = c("sex"),
   test.adj.vars = c("sex"),
   subject.var = "subject",
   time.var = "time",
   alpha.name = c("shannon", "observed_species"),
   depth = NULL,
   dist.name = c("BC",'Jaccard'),
   t.level = "1",
   feature.box.axis.transform = "sqrt",
   strata.var = "sex",
   vis.feature.level = c("Phylum", "Family", "Genus"),
   test.feature.level = "Family",
   feature.dat.type = "count",
   theme.choice = "bw",
   base.size = 20,
   feature.mt.method = "none",
   feature.sig.level = 0.2,
   output.file = "path/to/mStat_generate_report_single_example.pdf"
 )
```

<figure><img src="../.gitbook/assets/mStat_generate_report_single_example_page-0001.jpg" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/mStat_generate_report_single_example_page-0002.jpg" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/mStat_generate_report_single_example_page-0003.jpg" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/mStat_generate_report_single_example_page-0004.jpg" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/mStat_generate_report_single_example_page-0005.jpg" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/mStat_generate_report_single_example_page-0006.jpg" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/mStat_generate_report_single_example_page-0007.jpg" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/mStat_generate_report_single_example_page-0008.jpg" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/mStat_generate_report_single_example_page-0009.jpg" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/mStat_generate_report_single_example_page-0010.jpg" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/mStat_generate_report_single_example_page-0011.jpg" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/mStat_generate_report_single_example_page-0012.jpg" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/mStat_generate_report_single_example_page-0013.jpg" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/mStat_generate_report_single_example_page-0014.jpg" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/mStat_generate_report_single_example_page-0015.jpg" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/mStat_generate_report_single_example_page-0016.jpg" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/mStat_generate_report_single_example_page-0017.jpg" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/mStat_generate_report_single_example_page-0018.jpg" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/mStat_generate_report_single_example_page-0019.jpg" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/mStat_generate_report_single_example_page-0020.jpg" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/mStat_generate_report_single_example_page-0021.jpg" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/mStat_generate_report_single_example_page-0022.jpg" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/mStat_generate_report_single_example_page-0023.jpg" alt=""><figcaption></figcaption></figure>

{% file src="../.gitbook/assets/mStat_generate_report_single_example.pdf" %}

The automated report relieves the burden of **manual analysis** and promotes consistency. By **integrating results** from diverse analytical dimensions, it strengthens evidence and tells a cohesive story.

MicrobiomeStat powers researchers with **automated workflows** to swiftly synthesize and report discoveries from cross-sectional studies. The standardized reports integrate multidimensional perspectives including:

* **Alpha diversity**
* **Beta diversity**
* **Differential abundance analysis**

for comprehensive insights.

The reports contain intuitive visualizations and statistical summaries to support biological interpretation and facilitate result dissemination. By automating time-consuming, manual analytical tasks, MicrobiomeStat enables rapid, reproducible, and robust reporting to accelerate microbiome research.
