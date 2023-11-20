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

* `count`: For raw count data, the function first performs rarefaction, followed by a Total Sum Scaling (TSS) normalization. This process ensures that the data is suitably normalized and comparable across samples.
* `proportion`: Data presented as proportions remains unaltered. However, it's worth noting that during the LinDA differential abundance analysis, zeroes in the dataset are substituted with half of the smallest non-zero count for each feature. This adjustment is done to mitigate the impact of zero-inflation.
* `other`: In scenarios where the data originates from non-compositional sources, like transcriptomics and metabolomics, a different data transformation approach might be more appropriate. When `feature.dat.type` is set to "other", the function does not perform any normalization or scaling operations, allowing users to apply domain-specific transformations if necessary.

Further enhancing data robustness, the `prev.filter` and `abund.filter` parameters filter taxa based on prevalence and average abundance, respectively. Specifically:

* `prev.filter` targets taxa retention based on their prevalence across samples.
* `abund.filter` focuses on the average abundance of taxa across all samples.

Such filtering ensures that the analysis centers on taxa both prevalent and abundant, enhancing result reliability by excluding potential outliers or noise.

For those directly analyzing entities like OTU, ASV, Gene, KEGG, etc., that don't require aggregation, it's recommended to set the `feature.level` parameter to "original". Or we can perform testing on aggregated levels such as "Phylum", "Family" and "Genus" levels for microbiome data.

Furthermore, when interpreting the results, it's essential to understand `feature.sig.level` and `feature.mt.method` parameters:

* `feature.sig.level`: This parameter determines the significance level, primarily influencing the position of the dashed lines in the volcano plot. It sets the threshold for distinguishing between significant and non-significant differences in taxa abundance.
* `feature.mt.method`: There are two options available for this parameter: "fdr" (False Discovery Rate) and "none". Regardless of how this parameter is set, it's crucial to note that the `generate_taxa_test_single` function always performs adjustments post-testing. However, the `feature.mt.method` specifically influences the visualization in the volcano plot.

By understanding and appropriately setting these parameters, users can ensure a more accurate  interpretation of the plotted results.

```r
# Load data
data(peerj32.obj)

# Generate taxa test
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

# Generate volcano plots
volcano_plots <- generate_taxa_volcano_single(
    data.obj = peerj32.obj,
    group.var = "group",
    test.list = test.list,
    feature.sig.level = 0.1,
    feature.mt.method = "none"
)
```

<figure><img src="../.gitbook/assets/Screenshot 2023-10-07 at 15.18.08.png" alt=""><figcaption></figcaption></figure>

The resultant volcano plot visualizes the relationship between the magnitude of change (fold-change) and its statistical significance. This graphical representation aids researchers in pinpointing taxa with substantial differential abundance.

The `generate_taxa_test_single` also outputs a table of LinDA association statistics for all tested taxa/features.

In the context of this table:

* `Variable`: This corresponds to the specific feature level you have set in `feature.level`. It identifies the taxon or feature being analyzed.
* `Coefficient`: Also known as log2FoldChange, this represents the bias-corrected coefficients. It indicates the degree and direction of change in the abundance of a specific taxon.
* `SE`: Standard errors of the coefficients. It measures the variability of the coefficient estimates.
* `Mean Abundance`: This average abundance of a particular taxon (variable) across all samples.
* `Prevalence`: The proportion of samples where a specific taxon is present.

#### First 10 Rows of Differential Abundance Results at Genus Level

<table><thead><tr><th>Variable</th><th width="135">Coefficient</th><th width="54">SE</th><th>P.Value</th><th>Adjusted.P.Value</th><th>Mean.Abundance</th><th>Prevalence</th></tr></thead><tbody><tr><td>Actinomycetaceae</td><td>0.43088737</td><td>0.8125191</td><td>0.60204023</td><td>0.9892442</td><td>0.0001950405</td><td>0.7272727</td></tr><tr><td>Aerococcus</td><td>-0.09734179</td><td>0.7893169</td><td>0.90314569</td><td>0.9892442</td><td>0.0002352668</td><td>0.5909091</td></tr><tr><td>Aeromonas</td><td>0.02022775</td><td>1.0311377</td><td>0.98455351</td><td>0.9892442</td><td>0.0002829477</td><td>0.6363636</td></tr><tr><td>Akkermansia</td><td>-0.70914707</td><td>0.5667601</td><td>0.22603834</td><td>0.9892442</td><td>0.0202212889</td><td>1.0000000</td></tr><tr><td>Allistipes et rel.</td><td>-0.54628088</td><td>0.4985462</td><td>0.28688461</td><td>0.9892442</td><td>0.0083365762</td><td>1.0000000</td></tr><tr><td>Anaerofustis</td><td>0.37736758</td><td>0.3988262</td><td>0.35592789</td><td>0.9892442</td><td>0.0013725489</td><td>1.0000000</td></tr><tr><td>Anaerostipes caccae et rel.</td><td>-0.45297144</td><td>0.1723513</td><td>0.01655715</td><td>0.7004574</td><td>0.0185944926</td><td>1.0000000</td></tr><tr><td>Anaerotruncus colihominis et rel.</td><td>0.05420558</td><td>0.4605402</td><td>0.90754073</td><td>0.9892442</td><td>0.0028354658</td><td>1.0000000</td></tr><tr><td>Anaerovorax odorimutans et rel.</td><td>-0.55577241</td><td>0.3180729</td><td>0.09672749</td><td>0.9892442</td><td>0.0044828621</td><td>1.0000000</td></tr><tr><td>Aneurinibacillus</td><td>-0.20493605</td><td>0.5227462</td><td>0.69939327</td><td>0.9892442</td><td>0.0007296989</td><td>0.9545455</td></tr></tbody></table>

By inspecting these metrics, researchers can gain insights into the relative importance of each taxon within the analyzed samples, informing subsequent analyses and visualization.

Next, we will introduce functions to plot the taxa/features data. They can be used to visualize specific taxa/features, for example, those selected from differential abundance anlysis. Or they can be used to visualize all taxa with some basic filtering. The first function is `generate_taxa_boxplot_single`, which has the following relevant parameters:

* `transform`: This parameter indicates the transformation to apply to the axis when plotting. Transformations are only applied when the `feature.dat.type` is set to either `"count"` or `"proportion"`. The available options for `transform` include:
  * `"identity"`: No transformation (default)
  * `"sqrt"`: Square root transformation
  * `"log"`: Logarithmic transformation. Zeros are replaced with half of the minimum non-zero value for each taxon before log transformation.
* `features.plot`: This parameter dictates which taxa/features should be visualized. For instance, after executing a differential abundance analysis, you might be inclined to closely inspect taxa that exhibit significant variations. By defining `features.plot` with a list of taxa with a p-value less than a certain threshold from your differential abundance results, you're streamlining the visualization to spotlight these select taxa. When `features.plot` is determined, the values for `prev.filter` and `abund.filter` are instantly set to 0, indicating that these won't be applied for further filtering in visualization.
* `top.k.plot` and `top.k.func`: In many scenarios, especially when navigating through expansive datasets, you may want to focus on a select subset of taxa that stand out either due to their sheer abundance or because they manifest a pronounced variability. `top.k.plot` lets you cap the maximum number of taxa to visualize. For instance, if you set it to 10, only the top 10 taxa, as determined by the criteria detailed in `top.k.func`, will be visualized.
* `top.k.func` assists in determining the criteria for this selection, offering three choices:
    * `"mean"`: Highlights taxa with the highest average abundances spread across samples.
    * `"sd"`: Selects taxa with the greatest variability (standard deviation) across samples, useful for identifying taxa with notable differences under varying conditions.
    * `"prevalence"`: Chooses taxa with the highest occurrence across samples, targeting those most consistently present.
    * Custom function: Users can input their own function to rank taxa based on a numeric vector it returns when applied to the abundance matrix. This allows for custom criteria beyond "mean", "sd", or "prevalence".

The following shows the usage and output of the function `generate_taxa_boxplot_single`. All the taxa are plotted together.

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

For more clarity, `generate_taxa_indiv_boxplot_single` provides separate plots for each taxon. When the `pdf` parameter is set to `TRUE`, you can find the file in your default directory. Each page of this file represents a boxplot for a specific taxon or feature.

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

Heatmaps are particularly insightful for discerning clusters of microbial families that exhibit similar abundance profiles across different samples. By default, the function will cluster rows (taxa/features) based on their abundance patterns. If researchers wish to see the taxa in their original order without clustering, they can achieve this by setting `cluster.rows = FALSE`. However, when clustering is enabled, patterns of microbial families with congruent abundance become readily discernible, painting a vivid picture of microbial dynamics.

The function also allows for column clustering when `cluster.cols = TRUE`, which can be instrumental in revealing samples that share analogous abundance characteristics. This could be pivotal in unearthing hidden sample groups or conditions that exhibit similar microbial compositions.

Let's dive into the function's implementation:

```r
generate_taxa_heatmap_single(
  data.obj = peerj32.obj,
  subject.var = "subject",
  time.var = NULL,
  t.level = NULL,
  group.var = "group",
  strata.var = "sex",
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

We can use `generate_taxa_dotplot_single` function to juxtapose average abundance and taxa prevalence across groups:

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

We can use `generate_taxa_barplot_single` function to generate starked bar plots of taxa. The function has an important parameter `feature.number`. &#x20;

* `feature.number`: This parameter determines the maximum number of taxa/features that will be visualized directly in the barplot. For datasets with numerous features, it's practical to limit to the most abundant or significant taxa, ensuring that the visualization remains informative and isn't cluttered. When the number of taxa surpasses the value defined in `feature.number`, the function aggregates low-abundance taxa into a collective category labeled "other". This means, for instance, if there are over 20 features in the dataset but `feature.number` is set to 20, the least abundant features that exceed this count will be collectively presented as "other" in the visualization. This approach ensures that the chart remains legible, highlighting the most dominant features, while still accounting for the contributions of less abundant taxa.

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
