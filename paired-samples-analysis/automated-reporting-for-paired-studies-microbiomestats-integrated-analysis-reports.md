---
description: >-
  MicrobiomeStat provides automated solutions for integrated analysis reporting
  in paired samples microbiome studies.
---

# Reports Generation

MicrobiomeStat provides an efficient solution by generating integrated reports for cross-sectional studies. These reports include:

* Visualizations to illustrate key findings
* Statistical summaries that highlight significant results

The `mStat_generate_report_pair()` function generates an integrated reports. It performs:

* **Alpha diversity**: The functions `generate_alpha_test_pair()`, `generate_alpha_change_test_pair()`, `generate_alpha_boxplot_long()`, and `generate_alpha_change_boxplot_pair()` are used for this purpose.
* **Beta diversity**: This is achieved through functions including `generate_beta_change_test_pair()`, `generate_beta_ordination_pair()`, `generate_beta_change_boxplot_pair()`, `generate_beta_pc_change_boxplot_pair()`, and `generate_beta_pc_boxplot_long()`.
* **Feature-level**: This involves the use of `generate_taxa_test_pair()`, `generate_taxa_change_test_pair()`, `generate_taxa_indiv_boxplot_long()`, `generate_taxa_boxplot_long()`, `generate_taxa_indiv_change_boxplot_pair()`, `generate_taxa_change_boxplot_pair()`, `generate_taxa_barplot_pair()`, `generate_taxa_dotplot_pair()`, `generate_taxa_change_dotplot_pair()`, `generate_taxa_heatmap_pair()`, and `generate_taxa_change_heatmap_pair()`.

The function then compiles these analyses into a comprehensive PDF report. The report includes:

* A data summary from `mStat_summarize_data_obj()`
* Visual representations like alpha diversity boxplots, beta diversity ordination plots, and taxa composition visualizations
* Tables detailing statistical test results
* Commentary on key findings

Before using the function, it's important to understand the parameters:

* `data.obj`: A list object in a specific format to MicrobiomeStat, which can include components such as feature.tab (matrix), feature.ann (matrix), meta.dat (data.frame), tree, and feature.agg.list (list). This object can be converted from other formats using several functions from the MicrobiomeStat package, or constructed manually. For more detailed information on how to convert data from other formats or how to construct the `data.obj` manually, please refer to the following page.

{% content-ref url="../setting-up-microbiomestat-installation-and-data-preparation/laying-the-foundation-creating-the-microbiomestat-data-object/" %}
[laying-the-foundation-creating-the-microbiomestat-data-object](../setting-up-microbiomestat-installation-and-data-preparation/laying-the-foundation-creating-the-microbiomestat-data-object/)
{% endcontent-ref %}

* Once the data has been successfully constructed, users may wish to perform minor adjustments to the data, such as removing levels with few subjects from the group.var. In such cases, you can refer to this document for guidance.

{% content-ref url="../data-manipulation-and-transformation/data-filtering.md" %}
[data-filtering.md](../data-manipulation-and-transformation/data-filtering.md)
{% endcontent-ref %}

* `group.var`: The name of the variable of primary interest.
* `test.adj.vars`: Names of columns in the metadata containing covariates to be adjusted for in statistical tests and models. Default is NULL, which indicates no covariates are adjusted for in statistical testing.
* `vis.adj.vars`: For alpha and beta diversity visualization functions, the `vis.adj.vars` parameter designates the column names in the metadata that correspond to covariates. When covariates are specified, `vis.adj.vars` will adjust the `alpha.obj` and `dist.obj` accordingly, resulting in an alpha diversity index and a distance matrix that have been modified to account for the additional covariates.
* `strata.var`: Variable to stratify the analysis by (optional).
* `subject.var`: Variable name used for subject identification.
* `time.var`: Variable name used for time points.
* `change.base`: This parameter specifies the base level for calculating changes in paired data. It is particularly useful when dealing with longitudinal data where you want to understand how different features change relative to a baseline measurement.
* `alpha.obj`: An optional list containing pre-calculated alpha diversity indices. If NULL (default), alpha diversity indices will be calculated using `mStat_calculate_alpha_diversity` function.
* `alpha.name`: The alpha diversity index to be plotted. Supported indices include "shannon", "simpson", "observed\_species", "chao1", "ace", and "pielou". If you are not interested in alpha diversity for datasets other than microbiome data, you can set this parameter to NULL. In this case, the report will not include any alpha diversity results, only feature-level analysis will be presented.
* `depth`: An integer. The sequencing depth to be used for the "Rarefy" and "Rarefy-TSS" methods. If NULL, the smallest total count across samples is used as the rarefaction depth.
* `dist.obj`: Distance matrix between samples, usually calculated using `mStat_calculate_beta_diversity` function. If NULL, beta diversity will be automatically computed from `data.obj` using `mStat_calculate_beta_diversity`.
* `dist.name`: A character vector specifying which beta diversity indices to calculate. Supported indices are "BC" (Bray-Curtis), "Jaccard", "UniFrac" (unweighted UniFrac), "GUniFrac" (generalized UniFrac), "WUniFrac" (weighted UniFrac), and "JS" (Jensen-Shannon divergence). Similar to `alpha.name`, if you do not care about beta diversity for non-microbiome data, you can set this parameter to NULL. The report will then exclude beta diversity results and only provide feature-level analysis.
* `pc.obj`: A list containing the results of dimension reduction/Principal Component Analysis. This should be the output from functions like `mStat_calculate_PC`, containing the PC coordinates and other metadata. If NULL (default), dimension reduction will be automatically performed using metric multidimensional scaling (MDS) via `mStat_calculate_PC`. The `pc.obj` list structure should contain:
  * `$points`: A matrix with samples as rows and PCs as columns containing the coordinates.
  * `$eig`: Eigenvalues for each PC dimension.
  * `$vectors`: Loadings vectors for features onto each PC.
  * Other metadata like `$method`, `$dist.name`, etc. See `mStat_calculate_PC` function for details on output format.
* `vis.feature.level`: The column name in the feature annotation matrix (feature.ann) of data.obj to use for visualization and plotting. This can be a taxonomic level like "Phylum" or "Genus" for microbiome data. For KEGG data, this could be a pathway level such as "Pathway\_L1", "Pathway\_L2", or "Pathway\_L3". If you want to avoid aggregation, you can set it to "original", and no aggregation will be performed. The selected feature level will be used to aggregate the data at the specified level in the generated visualizations.
* `test.feature.level`: The column name in the feature annotation matrix (feature.ann) of data.obj to use for testing or analytical purposes. This can be a taxonomic level like "Phylum" or "Genus" for microbiome data. For single-cell data, this could be a cell type identifier such as "CellType". For KEGG data, this could be a pathway level such as "Pathway\_L1", "Pathway\_L2", or "Pathway\_L3". If you want to avoid aggregation, you can set it to "original", and no aggregation will be performed. The selected feature level will be used to aggregate the data at the specified level for statistical tests and models.
* `feature.dat.type`: The type of the feature data, which determines how the data is handled in downstream analyses. Should be one of: "count": Raw count data, will be normalized innerally. "proportion": Data that has already been normalized to proportions/percentages. "other": Other data types such as transcriptomic and metabolomic data, where the user will decide the QC, normalization, and transformation. If the user wants to normalize/transform the abundance data on his own way, he can also use this option.
* `feature.change.func`: A function or character string specifying how to calculate the change from baseline value. This allows flexible options:
  * If a function is provided, it will be applied to each row to calculate change. The function should take 2 arguments: value at timepoint t and value at baseline t0.
  * If a character string is provided, following options are supported:
    * 'relative change': (value\_t - value\_t0) / (value\_t + value\_t0)
    * 'absolute change': value\_t - value\_t0
    * 'log fold change': log2(value\_t + 1e-5) - log2(value\_t0 + 1e-5)
  * Default is 'relative change'.
  * If none of the above options are matched, an error will be thrown indicating the acceptable options or prompting the user to provide a custom function.
* `feature.box.axis.transform`: A string indicating the transformation to apply to the data before plotting. Options are:
  * "identity": No transformation (default),
  * "sqrt": Square root transformation,
  * "log": Logarithmic transformation.
* `feature.analysis.rarafy`: Logical, indicating whether to rarefy the data at the feature-level for analysis. If TRUE, the feature data will be rarefied before analysis. Default is TRUE.
* `bar.area.feature.no`: A numeric value indicating the number of top abundant features to retain in both barplot and areaplot. Features with average relative abundance ranked below this number will be grouped into 'Other'. Default 20.
* `heatmap.feature.no`: A numeric value indicating the number of top abundant features to retain in the heatmap. Features with average relative abundance ranked below this number will be grouped into 'Other'. Default 20.
* `dotplot.feature.no`: A numeric value indicating the number of top abundant features to retain in the dotplot. Features with average relative abundance ranked below this number will be grouped into 'Other'. Default 40.
* `feature.mt.method`: Character, multiple testing method for features, "fdr" or "none", default is "fdr".
* `feature.sig.level`: Numeric, significance level cutoff for highlighting features, default is 0.1.
* `base.size`: Base font size for the generated plots.
* `theme.choice`: Plot theme choice. Can be one of: "prism": ggprism::theme\_prism(), "classic": theme\_classic(), "gray": theme\_gray(), "bw": theme\_bw().
* `output.file`: A character string specifying the output file name for the report. Full path can be specified, e.g., "path_to_your_location/report". The file extension (.pdf or .html) will be automatically added based on the `output.format` if not already present. This parameter also determines the title of the generated report. For example, if set to "path_to_your_location/my_report", the report title will be "my_report".
* `output.format`: A character string specifying the desired output format of the report. Must be either "pdf" or "html". Default is c("pdf", "html"), which will use the first value ("pdf") if not explicitly specified. This parameter determines whether the report will be generated as a PDF or HTML document.

Note: Before running the function, please be aware of potential compatibility issues between RStudio and LaTeX. These issues can lead to problems such as images from the RStudio Viewer appearing in unexpected locations in the PDF report. To avoid this, it's recommended to clear the current images in the RStudio Viewer before running the function. You can do this by clicking on the broom icon in the RStudio Viewer.

Now, let's see how we can implement the function:

```r
# Load the dataset
data(peerj32.obj)
data.obj = peerj32.obj

# Specify variable names
group.var = "group" # Variable used for grouping samples
subject.var = "subject" # Variable used for subject identification
time.var = "time" # Variable used for time points
strata.var = "sex" # Variable to stratify by in diversity calculations and statistical tests

# Specify diversity indices
alpha.name = c("shannon", "observed_species") # Alpha diversity indices to calculate
dist.name = c("BC",'Jaccard') # Beta diversity indices to calculate

# Specify feature levels for visualization and testing
vis.feature.level = c("Genus") # Feature levels to use for visualization
test.feature.level = c("Genus") # Feature level to use for testing

# Specify other parameters
feature.dat.type = "count" # Type of the feature data
theme.choice = "bw" # Theme choice for the plots
base.size = 18 # Base size for the plots
feature.mt.method = "none" # Multiple testing method for features
feature.sig.level = 0.1 # Significance level cutoff for highlighting features

# Specify output file
output.file = "path/to/report.pdf" # Replace with your own file path for the output report

# Specify parameters for feature retention
bar.area.feature.no = 30 # Number of top abundant features to retain in barplot and areaplot
heatmap.feature.no = 30 # Number of top abundant features to retain in heatmap
dotplot.feature.no = 20 # Number of top abundant features to retain in dotplot

# Specify optional parameters
dist.obj = NULL # Replace with a pre-computed distance matrix if available
alpha.obj = NULL # Replace with a pre-computed alpha diversity matrix if available
change.base = "1" # Base for calculating relative change
feature.change.func = "relative change" # Function for calculating feature change

# Run the function
mStat_generate_report_pair(
   data.obj = data.obj,
   group.var = group.var,
   test.adj.vars = NULL,
   vis.adj.vars = NULL,
   strata.var = strata.var,
   subject.var = subject.var,
   time.var = time.var,
   change.base = change.base,
   alpha.obj = alpha.obj,
   alpha.name = alpha.name,
   dist.obj = dist.obj,
   dist.name = dist.name,
   feature.change.func = feature.change.func,
   vis.feature.level = vis.feature.level,
   test.feature.level = test.feature.level,
   bar.area.feature.no = bar.area.feature.no,
   heatmap.feature.no = heatmap.feature.no,
   dotplot.feature.no = dotplot.feature.no,
   feature.dat.type = feature.dat.type,
   feature.mt.method = feature.mt.method,
   feature.sig.level = feature.sig.level,
   theme.choice = theme.choice,
   base.size = base.size,
   output.file = output.file
 )
```

{% hint style="info" %}
**Note:** Due to certain interactions between R and RStudio, the visualization of significant features in the boxplot section of the report may occasionally not be displayed. If you encounter this issue, a potential solution is to clear the Viewer in RStudio (by clicking on the broom icon), and subsequently reset the environment (by using the `rm(list=ls())` command or clicking on the broom icon in the Environment pane). This procedure often resolves the display issue, allowing the boxplot to render correctly.
{% endhint %}

<figure><img src="../.gitbook/assets/mStat_generate_report_pair_example_page-0001.jpg" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/mStat_generate_report_pair_example_page-0002.jpg" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/mStat_generate_report_pair_example_page-0003.jpg" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/mStat_generate_report_pair_example_page-0004.jpg" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/mStat_generate_report_pair_example_page-0005.jpg" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/mStat_generate_report_pair_example_page-0006.jpg" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/mStat_generate_report_pair_example_page-0007.jpg" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/mStat_generate_report_pair_example_page-0008.jpg" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/mStat_generate_report_pair_example_page-0009.jpg" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/mStat_generate_report_pair_example_page-0010.jpg" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/mStat_generate_report_pair_example_page-0011.jpg" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/mStat_generate_report_pair_example_page-0012.jpg" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/mStat_generate_report_pair_example_page-0013.jpg" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/mStat_generate_report_pair_example_page-0014.jpg" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/mStat_generate_report_pair_example_page-0015.jpg" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/mStat_generate_report_pair_example_page-0016.jpg" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/mStat_generate_report_pair_example_page-0017.jpg" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/mStat_generate_report_pair_example_page-0018.jpg" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/mStat_generate_report_pair_example_page-0019.jpg" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/mStat_generate_report_pair_example_page-0020.jpg" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/mStat_generate_report_pair_example_page-0021.jpg" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/mStat_generate_report_pair_example_page-0022.jpg" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/mStat_generate_report_pair_example_page-0023.jpg" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/mStat_generate_report_pair_example_page-0024.jpg" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/mStat_generate_report_pair_example_page-0025.jpg" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/mStat_generate_report_pair_example_page-0026.jpg" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/mStat_generate_report_pair_example_page-0027.jpg" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/mStat_generate_report_pair_example_page-0028.jpg" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/mStat_generate_report_pair_example_page-0029.jpg" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/mStat_generate_report_pair_example_page-0030.jpg" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/mStat_generate_report_pair_example_page-0031.jpg" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/mStat_generate_report_pair_example_page-0032.jpg" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/mStat_generate_report_pair_example_page-0033.jpg" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/mStat_generate_report_pair_example_page-0034.jpg" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/mStat_generate_report_pair_example_page-0035.jpg" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/mStat_generate_report_pair_example_page-0036.jpg" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/mStat_generate_report_pair_example_page-0037.jpg" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/mStat_generate_report_pair_example_page-0038.jpg" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/mStat_generate_report_pair_example_page-0039.jpg" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/mStat_generate_report_pair_example_page-0040.jpg" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/mStat_generate_report_pair_example_page-0041.jpg" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/mStat_generate_report_pair_example_page-0042.jpg" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/mStat_generate_report_pair_example_page-0043.jpg" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/mStat_generate_report_pair_example_page-0044.jpg" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/mStat_generate_report_pair_example_page-0045.jpg" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/mStat_generate_report_pair_example_page-0046.jpg" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/mStat_generate_report_pair_example_page-0047.jpg" alt=""><figcaption></figcaption></figure>

{% file src="../.gitbook/assets/mStat_generate_report_pair_example (1).pdf" %}

The automated report reduces the need for manual analysis and ensures consistency. By integrating results from different analytical dimensions, it provides a comprehensive view of the data.

MicrobiomeStat equips researchers with automated workflows to quickly synthesize and report findings from cross-sectional studies. The standardized reports integrate multidimensional perspectives, including alpha diversity, beta diversity, and differential abundance analysis, for comprehensive insights.

The reports contain clear visualizations and statistical summaries to aid in biological interpretation and facilitate result dissemination. By automating time-consuming manual analytical tasks, MicrobiomeStat enables rapid, reproducible, and robust reporting to advance microbiome research.
