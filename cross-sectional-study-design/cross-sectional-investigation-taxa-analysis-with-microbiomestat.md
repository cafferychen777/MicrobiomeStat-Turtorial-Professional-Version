---
description: >-
  Delve deep into microbial diversity patterns with MicrobiomeStat's
  feature-level analysis tools, providing insight into taxa abundance variations
  across distinct groups.
---

# Cross-Sectional Investigation: Feature-level Analysis with MicrobiomeStat

A foundational step in feature-level analysis is discerning taxa with differential abundance between groups. The `generate_taxa_test_single` function facilitates this by performing differential abundance testing.

The `generate_taxa_test_single` function employs the LinDA method to facilitate differential abundance analysis at the feature level. This method is essential for discerning taxa with distinct abundance between groups.

> Zhou H, He K, Chen J, Zhang X. LinDA: linear models for differential abundance analysis of microbiome compositional data. Genome Biol. 2022 Apr 14;23(1):95. doi: 10.1186/s13059-022-02655-5. PMID: 35421994; PMCID: PMC9012043.

The `feature.dat.type` parameter plays a crucial role in the data preprocessing phase:

* `count`: For raw count data, the function first performs sparsity treatment, followed by a Total Sum Scaling (TSS) normalization. This process ensures that the data is suitably normalized and comparable across samples.
* `proportion`: Data presented as proportions remains unaltered. However, it's worth noting that during the LinDA differential abundance analysis, zeroes in the dataset are substituted with half of the smallest non-zero count for each feature. This adjustment is done to mitigate the impact of zero-inflation.
* `other`: In scenarios where the data originates from non-microbiome sources, like single-cell studies, spatial transcriptomics, KEGG pathways, or gene data, a different data transformation approach might be more apt. When `feature.dat.type` is set to "other", the function refrains from any normalization or scaling operations, allowing users to apply domain-specific transformations if necessary.

Further enhancing data robustness, the `prev.filter` and `abund.filter` parameters filter taxa based on prevalence and average abundance, respectively. Specifically:

* `prev.filter` targets taxa retention based on their prevalence across samples.
* `abund.filter` focuses on the average abundance of taxa across all samples.

Such filtering ensures that the analysis centers on taxa both prevalent and abundant, enhancing result reliability by excluding potential outliers or noise.

For those directly analyzing entities like OTU, ASV, Gene, KEGG, etc., that don't require aggregation, it's recommended to set the `feature.level` parameter to "original".

Furthermore, when interpreting the results, it's essential to understand the role of `feature.sig.level` and `feature.mt.method` parameters:

* `feature.sig.level`: This parameter determines the significance level, primarily influencing the position of the dashed lines in the volcano plot. It sets the threshold for distinguishing between significant and non-significant differences in taxa abundance.
* `feature.mt.method`: There are two options available for this parameter: "fdr" (False Discovery Rate) and "none". Regardless of how this parameter is set, it's crucial to note that the `generate_taxa_test_single` function always performs adjustments post-testing. However, the `feature.mt.method` specifically influences the visualization in the volcano plot, guiding how p-values are adjusted in that context.

By understanding and appropriately setting these parameters, users can ensure a more accurate and contextually relevant interpretation of the plotted results.

```r
data(peerj32.obj)
test.list <- generate_taxa_test_single(
    data.obj = peerj32.obj,
    time.var = "time",
    t.level = "2",
    group.var = "group",
    adj.vars = "sex",
    feature.dat.type = "count",
    feature.level = c("Phylum","Genus","Family"),
    prev.filter = 0.1,
    abund.filter = 0.0001
)
volcano_plots <- generate_taxa_volcano_single(data.obj = peerj32.obj,
                                              group.var = "group",
                                              test.list = test.list,
                                              feature.sig.level = 0.1,
                                              feature.mt.method = "none")
volcano_plots
```

<figure><img src="../.gitbook/assets/Screenshot 2023-10-07 at 15.18.08.png" alt=""><figcaption></figcaption></figure>

The resultant volcano plot visualizes the relationship between the magnitude of change (fold-change) and its statistical significance. This graphical representation aids researchers in pinpointing taxa with substantial differential abundance.

Upon preprocessing which includes filtering, normalization, and optional aggregation at designated taxonomic levels, the function employs the LinDA method to identify variances between groups while adjusting for provided covariates.

Subsequently, the provided table offers a comprehensive overview of various taxa accompanied by pertinent statistical measures, paving the way for detailed exploration and analysis.

In the context of this table:

* `Variable`: This corresponds to the specific feature level you have set in `feature.level`. It identifies the taxon or feature being analyzed.
* `Coefficient`: Also known as log2FoldChange, this represents the bias-corrected coefficients. It indicates the degree and direction of change in the abundance of a specific taxon.
* `SE`: This stands for lfcSE, the standard errors of the coefficients. It measures the variability or dispersion of the coefficient values.
* `Mean Abundance`: This signifies the average abundance of a particular taxon (variable) across all samples.
* `Prevalence`: This metric represents the proportion of samples where a specific taxon is present, reflecting its widespread occurrence across the dataset.

#### Differential abundance results at Genus level

| Variable                          | Coefficient | SE        | P.Value    | Adjusted.P.Value | Mean.Abundance | Prevalence |
| --------------------------------- | ----------- | --------- | ---------- | ---------------- | -------------- | ---------- |
| Actinomycetaceae                  | 0.43088737  | 0.8125191 | 0.60204023 | 0.9892442        | 0.0001950405   | 0.7272727  |
| Aerococcus                        | -0.09734179 | 0.7893169 | 0.90314569 | 0.9892442        | 0.0002352668   | 0.5909091  |
| Aeromonas                         | 0.02022775  | 1.0311377 | 0.98455351 | 0.9892442        | 0.0002829477   | 0.6363636  |
| Akkermansia                       | -0.70914707 | 0.5667601 | 0.22603834 | 0.9892442        | 0.0202212889   | 1.0000000  |
| Allistipes et rel.                | -0.54628088 | 0.4985462 | 0.28688461 | 0.9892442        | 0.0083365762   | 1.0000000  |
| Anaerofustis                      | 0.37736758  | 0.3988262 | 0.35592789 | 0.9892442        | 0.0013725489   | 1.0000000  |
| Anaerostipes caccae et rel.       | -0.45297144 | 0.1723513 | 0.01655715 | 0.7004574        | 0.0185944926   | 1.0000000  |
| Anaerotruncus colihominis et rel. | 0.05420558  | 0.4605402 | 0.90754073 | 0.9892442        | 0.0028354658   | 1.0000000  |
| Anaerovorax odorimutans et rel.   | -0.55577241 | 0.3180729 | 0.09672749 | 0.9892442        | 0.0044828621   | 1.0000000  |
| Aneurinibacillus                  | -0.20493605 | 0.5227462 | 0.69939327 | 0.9892442        | 0.0007296989   | 0.9545455  |

By inspecting these metrics, researchers can gain insights into the relative importance and ubiquity of each taxon within the analyzed samples, fostering an informed approach to subsequent analyses.

Before we delve into the detailed examination of the `generate_taxa_boxplot_single` function, it's essential to first understand some critical parameters that play a pivotal role in shaping the visualization:

* `transform`: This parameter is a string indicating the transformation to apply to the axis when plotting. The options include:
  * `"identity"`: No transformation (default)
  * `"sqrt"`: Square root transformation
  * `"log"`: Logarithmic transformation. Zeros are replaced with half of the minimum non-zero value for each taxon before log transformation.
* `features.plot`: This parameter dictates which features (or taxa) should be visualized. For instance, after executing a differential abundance analysis, you might be inclined to closely inspect taxa that exhibit significant variations. By defining `features.plot` with a list of taxa with a p-value less than a certain threshold from your differential abundance results, you're streamlining the visualization to spotlight these select taxa. When `features.plot` is determined, the values for `prev.filter` and `abund.filter` are instantly set to 0, indicating that these won't be applied for further filtering in visualization.
* `top.k.plot` and `top.k.func`: In many scenarios, especially when navigating through expansive datasets, you may want to focus on a select subset of taxa that stand out either due to their sheer abundance or because they manifest a pronounced variability. `top.k.plot` lets you cap the maximum number of taxa to visualize. For instance, if you set it to 10, only the top 10 taxa, as determined by the criteria detailed in `top.k.func`, will be visualized.
  * `top.k.func` assists in determining the criteria for this selection, offering two choices:
    * `"mean"`: Highlights taxa with the highest average abundances spread across samples.
    * `"sd"`: Focuses on taxa that exhibit the most pronounced variability (standard deviation) across samples. This proves particularly insightful when you're keen on understanding taxa that display marked differences across different conditions or over distinct time frames.

With this foundational understanding in place, let's proceed with the function:

1. `generate_taxa_boxplot_single` outputs a singular comprehensive plot with taxa data:

```r
generate_taxa_boxplot_single(
  data.obj = peerj32.obj,
  subject.var = "subject",
  time.var = "time",
  t.level = "1",
  group.var = "group",
  strata.var = NULL,
  feature.level = c("Family"),
  feature.dat.type = "count",
  features.plot = NULL,
  top.k.plot = 8,
  top.k.func = "mean",
  transform = "log",
  prev.filter = 0,
  abund.filter = 0,
  base.size = 16,
  theme.choice = "bw",
  custom.theme = NULL,
  palette = NULL,
  pdf = TRUE,
  file.ann = NULL,
  pdf.wid = 11,
  pdf.hei = 8.5
)
```

<figure><img src="../.gitbook/assets/Screenshot 2023-10-10 at 21.03.21.png" alt=""><figcaption><p>This boxplot, generated by MicrobiomeStat's <code>generate_taxa_boxplot_single</code> function, visually showcases the distribution of microbial genus abundance between LGG and placebo groups at time point 1. The white lines represent the median abundance, while the violin shapes showcase the abundance distribution. Genera like Akkermansia and Parabacteroides exhibit lower abundance among LGG samples compared to placebo. This plot swiftly highlights patterns of differential abundance between groups, priming us for subsequent in-depth analysis.</p></figcaption></figure>

2. For more granularity, `generate_taxa_indiv_boxplot_single` provides separate plots for each taxon. When the `pdf` parameter is set to `TRUE`, you can find the file in your default directory. Each page of this file represents a boxplot for a specific taxon or feature.

```r
generate_taxa_indiv_boxplot_single(
  data.obj = peerj32.obj,
  subject.var = "subject",
  time.var = "time",
  t.level = "1",
  group.var = "group",
  strata.var = "sex",
  feature.level = c("Family"),
  features.plot = NULL,
  feature.dat.type = "count",
  top.k.plot = NULL,
  top.k.func = NULL,
  transform = "log",
  prev.filter = 0,
  abund.filter = 0,
  base.size = 20,
  theme.choice = "bw",
  custom.theme = NULL,
  palette = NULL,
  pdf = TRUE,
  file.ann = NULL,
  pdf.wid = 11,
  pdf.hei = 8.5
)
```

<figure><img src="../.gitbook/assets/Screenshot 2023-08-05 at 20.04.16.png" alt=""><figcaption><p>These individual boxplots, produced by MicrobiomeStat's <code>generate_taxa_indiv_boxplot_single</code> function, offer a nuanced view of microbial family abundance distribution across subjects, faceted by sex. The x-axis represents subjects grouped by LGG/placebo, and y-axis shows log-transformed abundance. Differing abundance ranges and distribution shapes are observed between taxa like Ruminococcaceae and Lachnospiraceae. Such customized inspection uncovers fine-grained sample-level variations, setting the stage for subsequent integration of patterns.</p></figcaption></figure>

Transitioning from bar plots, we venture into the domain of heatmaps using the `generate_taxa_heatmap_single` function. Heatmaps offer a unique, colorful perspective on data, visually encapsulating the richness and patterns of microbial abundance across samples.

Heatmaps are particularly insightful for discerning clusters of microbial families that exhibit similar abundance profiles across different samples. By default, the function will cluster rows (features or taxa) based on their abundance patterns. If researchers wish to see the taxa in their original order without clustering, they can achieve this by setting `cluster.rows = FALSE`. However, when clustering is enabled, patterns of microbial families with congruent abundance become readily discernible, painting a vivid picture of microbial dynamics.

The function also allows for column clustering when `cluster.cols = TRUE`, which can be instrumental in revealing samples that share analogous abundance characteristics. This could be pivotal in unearthing hidden sample groups or conditions that exhibit similar microbial compositions.

Let's dive into the function's implementation:

```r
generate_taxa_heatmap_single(
  data.obj = peerj32.obj,
  subject.var = "subject",
  time.var = NULL,
  t.level = NULL,
  group.var = "group",
  strata.var = NULL,
  feature.level = c("Family"),
  feature.dat.type = "count",
  features.plot = NULL,
  top.k.plot = NULL,
  top.k.func = NULL,
  prev.filter = 0.1,
  abund.filter = 0.1,
  base.size = 10,
  palette = NULL,
  pdf = TRUE,
  file.ann = NULL,
  pdf.wid = 11,
  pdf.hei = 8.5
)
```

<figure><img src="../.gitbook/assets/Screenshot 2023-10-10 at 21.16.15.png" alt=""><figcaption><p>This heatmap, generated by MicrobiomeStat's <code>generate_taxa_heatmap_single</code> function, vividly showcases the abundance of microbial families across our groups of interest. The color-coding allows for swift detection of abundance patterns, setting the stage for an in-depth comparative analysis.</p></figcaption></figure>

Subsequent analysis can employ the `generate_taxa_dotplot_single` function to juxtapose average abundance and taxa prevalence across groups:

```r
generate_taxa_dotplot_single(
  data.obj = peerj32.obj,
  subject.var = "subject",
  time.var = NULL,
  t0.level = NULL,
  group.var = "group",
  strata.var = "sex",
  feature.level = c("Family"),
  feature.dat.type = "count",
  features.plot = NULL,
  top.k.plot = NULL,
  top.k.func = NULL,
  prev.filter = 0.1,
  abund.filter = 0.001,
  base.size = 16,
  theme.choice = "bw",
  custom.theme = NULL,
  palette = NULL,
  pdf = TRUE,
  file.ann = NULL,
  pdf.wid = 15,
  pdf.hei = 8.5
)
```

<figure><img src="../.gitbook/assets/Screenshot 2023-10-10 at 21.34.06.png" alt=""><figcaption><p>With the help of the <code>generate_taxa_dotplot_single</code> function, this dot plot presents a clear comparison of the average abundance and prevalence of each microbial family across our groups. The graphical representation makes it easier to spot potential differences in microbial presence and abundance across different groups.</p></figcaption></figure>

Before we dive into the visual interpretation offered by the `generate_taxa_barplot_single` function, let's touch upon an essential parameter that fundamentally steers the visual output – the `feature.number`.

* `feature.number`: This parameter determines the maximum number of features (or taxa) that will be visualized directly in the barplot. When confronted with datasets teeming with numerous features, it's practical to limit our visual focus to the most abundant or significant taxa, ensuring that the visualization remains informative and isn't cluttered. When the number of taxa surpasses the value defined in `feature.number`, the function smartly aggregates all excess, low-abundance taxa into a collective category labeled "other". This means, for instance, if there are over 20 features in the dataset but `feature.number` is set to 20, the least abundant features that exceed this count will be collectively presented as "other" in the visualization. This approach ensures that the chart remains legible, highlighting the most dominant features, while still accounting for the contributions of less abundant taxa.

With this insight, let's now examine the function and the resultant visuals:

```r
generate_taxa_barplot_single(
  data.obj = peerj32.obj,
  subject.var = "subject",
  time.var = NULL,
  t.level = NULL,
  group.var = "group",
  strata.var = "sex",
  feature.level = "Family",
  feature.dat.type = "count",
  feature.number = 30,
  base.size = 10,
  theme.choice = "bw",
  custom.theme = NULL,
  palette = NULL,
  pdf = TRUE,
  file.ann = NULL,
  pdf.wid = 11,
  pdf.hei = 8.5
)
```

<figure><img src="../.gitbook/assets/Screenshot 2023-10-10 at 21.37.34.png" alt=""><figcaption><p>This stacked bar plot, produced by the <code>generate_taxa_barplot_single</code> function, provides a detailed overview of individual species composition in our groups. By layering the information, we're able to see the distinct microbial composition of each subject, allowing us to appreciate the individual variability in the microbiome.</p></figcaption></figure>

<figure><img src="../.gitbook/assets/Screenshot 2023-10-10 at 21.38.40.png" alt=""><figcaption><p>Leveraging the same <code>generate_taxa_barplot_single</code> function, this stacked bar plot reveals the average species composition across our groups. The layers illustrate the overall microbial balance, offering a birds-eye view on group-level differences in microbiome structure.</p></figcaption></figure>

These visual tools, particularly the stacked bar plots, are paramount in understanding the species distribution both at individual and group levels. They highlight the relative contributions of various species to the overall microbiome composition, presenting a clear picture of the microbial diversity landscape.
