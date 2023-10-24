---
description: >-
  This tutorial focuses on techniques for differential abundance analysis,
  highlighting significant changes in microbial abundance between paired
  conditions.
---

# Inspecting Paired Samples: Feature-level Analysis

In this section, we focus on the variations within microbial ecosystems, particularly taxa variations. MicrobiomeStat provides tools for differential abundance analysis, enabling us to study significant changes in microbial abundance between paired conditions.

Our goal is to understand patterns in taxa composition, prevalence, abundance, and changes, and to identify samples with similar trends of taxa variation. The tutorial will guide you through the process of analyzing your microbial data and understanding the complexity of microbial communities.

Before visual exploration, statistical tests are performed to identify differentially abundant taxa. The function `generate_taxa_test_pair()` employs the LinDA method to facilitate differential abundance analysis at the feature level. This method is essential for discerning taxa with distinct abundance and change trend between groups.

> Zhou H, He K, Chen J, Zhang X. LinDA: linear models for differential abundance analysis of microbiome compositional data. Genome Biol. 2022 Apr 14;23(1):95. doi: 10.1186/s13059-022-02655-5. PMID: 35421994; PMCID: PMC9012043.

The `feature.dat.type` parameter plays a crucial role in the data preprocessing phase:

* `"count"`: For raw count data, the function first performs sparsity treatment, followed by a Total Sum Scaling (TSS) normalization. This process ensures that the data is suitably normalized and comparable across samples.
* `"proportion"`: Data presented as proportions remains unaltered.However, it's worth noting that during the LinDA differential abundance analysis, zeroes in the dataset are substituted with half of the smallest non-zero count for each feature. This adjustment is done to mitigate the impact of zero-inflation.
* `"other"`: In scenarios where the data originates from non-microbiome sources, like single-cell studies, spatial transcriptomics, KEGG pathways, or gene data, a different data transformation approach might be more apt. When `feature.dat.type` is set to `"other"`, the function refrains from any normalization or scaling operations, allowing users to apply domain-specific transformations if necessary.

Further enhancing data robustness, the `prev.filter` and `abund.filter` parameters filter taxa based on prevalence and average abundance, respectively. Specifically:

* `prev.filter` targets taxa retention based on their prevalence across samples.
* `abund.filter` focuses on the average abundance of taxa across all samples.

Such filtering ensures that the analysis centers on taxa both prevalent and abundant, enhancing result reliability by excluding potential outliers or noise.

For those directly analyzing entities like OTU, ASV, Gene, KEGG, etc., that don't require aggregation, it's recommended to set the `feature.level` parameter to `"original"`.

Furthermore, when interpreting the results, it's essential to understand the role of `feature.sig.level` and `feature.mt.method` parameters:

* `feature.sig.level`: This parameter determines the significance level, primarily influencing the position of the dashed lines in the volcano plot. It sets the threshold for distinguishing between significant and non-significant differences in taxa abundance.
* `feature.mt.method`: There are two options available for this parameter: `"fdr"` (False Discovery Rate) and `"none"`. Regardless of how this parameter is set, it's crucial to note that the test function always performs adjustments post-testing. However, the `feature.mt.method` specifically influences the visualization in the volcano plot, guiding how p-values are adjusted in that context.

Another important parameter is `feature.change.func`, which specifies the method or function used to compute the change between two time points. The options include:

* `"absolute change"` (default): Computes the absolute difference between the values at the two time points (`value_time_2` and `value_time_1`).
* `"log fold change"`: Computes the log2 fold change between the two time points. For zero values, imputation is performed using half of the minimum nonzero value for each feature level at the respective time point before taking the logarithm.
* `"relative change"`: Computes the relative change as `(value_time_2 - value_time_1) / (value_time_2 + value_time_1)`. If both time points have a value of 0, the change is defined as 0.
* A custom function: If a user-defined function is provided, it should take two numeric vectors as input corresponding to the values at the two time points (`value_time_1` and `value_time_2`) and return a numeric vector of the computed change. This custom function will be applied directly to calculate the difference.

By understanding and appropriately setting these parameters, users can ensure a more accurate and contextually relevant interpretation of the plotted results.

```r
#' Load the dataset
data(peerj32.obj)

#' Perform the differential abundance test
test.list <- generate_taxa_test_pair(
  data.obj = peerj32.obj,
  subject.var = "subject",
  time.var = "time",
  group.var = "group",
  adj.vars = c("sex"),
  feature.level = c("Genus"),
  prev.filter = 0.1,
  abund.filter = 0.0001,
  feature.dat.type = "count"
)

#' Generate the volcano plot
plot.list <- generate_taxa_volcano_single(
  data.obj = peerj32.obj,
  group.var = "group",
  test.list = test.list,
  feature.sig.level = 0.1,
  feature.mt.method = "none"
)

plot.list
```

<figure><img src="../.gitbook/assets/Screenshot 2023-10-11 at 16.07.17.png" alt=""><figcaption><p>Volcano plot for main effect presents the volcano plot for the Main Effect, reflecting the primary differences in taxa abundance between the compared groups, excluding the influence of other factors. Each point represents a genus, with its position on the x-axis indicating the log fold change and on the y-axis indicating the adjusted p-value (-log10). Points located at the top and on either side of the vertical line represent genera with statistically significant differences in abundance.</p></figcaption></figure>

<figure><img src="../.gitbook/assets/Screenshot 2023-10-11 at 16.07.38.png" alt=""><figcaption><p>Volcano plot for interaction effect displays the volcano plot for the Interaction Effect, revealing the relationship between group differences and the time variable. It indicates if the difference in taxa abundance between groups varies depending on other factors in the model. Each point represents a genus, with its position on the x-axis indicating the log fold change and on the y-axis indicating the adjusted p-value (-log10). Points located at the top and on either side of the vertical line represent genera with statistically significant differences in abundance.</p></figcaption></figure>

For specific comparisons, labels like `$Genus$Placebo vs LGG (Reference) [Main Effect]` and `$Genus$Placebo vs LGG (Reference) [Interaction]` may appear.

* The `[Main Effect]` label indicates the primary difference in taxa abundance between the compared groups, excluding the influence of other factors.
* The `[Interaction]` label reveals the relationship between group differences and time variable. It indicates if the difference in taxa abundance between groups varies depending on other factors in the model.

If the aim is to specifically investigate the shifts in taxa abundance across distinct timepoints, the `generate_taxa_change_test_pair()` function is advantageous. This function employs a linear model (lm) to evaluate the changes in taxa abundance and to distinguish the differences between groups.

```r
# Generate taxa change test pair
test.list <- generate_taxa_change_test_pair(
  data.obj = peerj32.obj,
  subject.var = "subject",
  time.var = "time",
  group.var = "group",
  adj.vars = c("sex"),
  change.base = "1",
  feature.change.func = "log fold change",
  feature.level = "Genus",
  prev.filter = 0.01,
  abund.filter = 0.001,
  feature.dat.type = "count"
)

# Generate the volcano plot
plot.list <- generate_taxa_volcano_single(
  data.obj = peerj32.obj,
  group.var = "group",
  test.list = test.list,
  feature.sig.level = 0.1,
  feature.mt.method = "none"
)
```

<figure><img src="../.gitbook/assets/Screenshot 2023-10-11 at 16.37.16.png" alt=""><figcaption><p>Volcano plot visualizing the taxa changes between groups. Each point represents a taxon, plotted according to its log2 fold change (x-axis) and -log10 adjusted p-value (y-axis) derived from the <code>generate_taxa_change_test_pair()</code>function. The plot helps to identify taxa with significant differences in abundance across timepoints. The horizontal line indicates the threshold for statistical significance (p=0.1). Taxa that pass this threshold are considered significantly different between groups.</p></figcaption></figure>

The outcomes enable researchers to recognize taxa that exhibit notable changes in abundance across timepoints, and to determine whether these changes are associated with group affiliations. In the following sections, we will demonstrate how to visualize features of interest from the differential abundance analysis, or other specific features, using various methods.

In the analysis of large-scale microbiome datasets, it's often necessary to concentrate on a subset of taxa that are either most abundant or exhibit significant variability. This focus is facilitated by two parameters, `top.k.plot` and `top.k.func`, which are particularly useful when creating boxplot visualizations.

The `top.k.plot` parameter limits the number of taxa visualized. For instance, setting it to 10 will visualize only the top 10 taxa. The criteria for this selection is defined by `top.k.func`, which can be set to `"mean"` to prioritize taxa with the highest mean abundances across samples, or `"sd"` to emphasize taxa with the greatest standard deviation across samples. This is especially beneficial when studying taxa with substantial differences across conditions or time frames.

These parameters are integral to our next topic of discussion: boxplot visualization analyses for individual taxa. We introduce two functions for this purpose, `generate_taxa_indiv_boxplot_long()` and `generate_taxa_boxplot_long()`.

Before delving into these functions, it's worth noting the `transform` parameter. It's a string indicating the transformation to apply to the axis when plotting. The options include:

* `"identity"`: No transformation (default)
* `"sqrt"`: Square root transformation
* `"log"`: Logarithmic transformation. Zeros are replaced with half of the minimum non-zero value for each taxon before log transformation.

Additionally, three other parameters play significant roles in these functions:

First, we look at `generate_taxa_indiv_boxplot_long()`:

```r
generate_taxa_indiv_boxplot_long(
   data.obj = peerj32.obj,
   subject.var = "subject",
   time.var = "time",
   t0.level = "1",
   ts.levels = "2",
   group.var = "group",
   strata.var = "sex",
   feature.level = c("Genus"),
   feature.dat.type = "count",
   features.plot = NULL,
   top.k.plot = NULL,
   top.k.func = NULL,
   transform = "log",
   prev.filter = 0.1,
   abund.filter = 0.001,
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

<figure><img src="../.gitbook/assets/Screenshot 2023-10-11 at 16.50.17.png" alt=""><figcaption><p>Longitudinal Boxplot of Individual Taxa: The <code>generate_taxa_indiv_boxplot_long()</code> function enables an in-depth investigation of the variation in abundance for each taxon within paired samples across time. Each page of the multi-page PDF represents a single taxon, providing a focused perspective on the dynamic changes under paired conditions.</p></figcaption></figure>

This function creates a series of boxplots, one for each taxon, and outputs them into a multi-page PDF.

Next, we have `generate_taxa_boxplot_long()`:

```r
generate_taxa_boxplot_long(
   data.obj = peerj32.obj,
   subject.var = "subject",
   time.var = "time",
   t0.level = "1",
   ts.levels = "2",
   group.var = "group",
   strata.var = "sex",
   feature.level = c("Genus"),
   feature.dat.type = "count",
   features.plot = NULL,
   top.k.plot = 8,
   top.k.func = "mean",
   transform = "log",
   prev.filter = 0.1,
   abund.filter = 0.001,
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

<figure><img src="../.gitbook/assets/Screenshot 2023-10-11 at 16.52.12.png" alt=""><figcaption><p>Comprehensive Boxplot of Individual Taxa: The <code>generate_taxa_boxplot_long()</code> function generates a combined boxplot of all taxa, allowing an immediate comparison of abundance variations across paired samples over time. This visualization is particularly useful in highlighting taxa with significant differential abundance, offering a clear overview of the microbiome dynamics within paired samples.</p></figcaption></figure>

This function places all taxa onto a single page, providing an overview of all your taxa at once.

We present two key functions for conducting paired taxa analysis: `generate_taxa_indiv_change_boxplot_pair()` and `generate_taxa_change_boxplot_pair()`.

The initial function, `generate_taxa_indiv_change_boxplot_pair()`, is utilized as follows:

```R
generate_taxa_indiv_change_boxplot_pair(
   data.obj = peerj32.obj,
   subject.var = "subject",
   time.var = "time",
   group.var = "group",
   strata.var = "sex",
   change.base = "1",
   feature.change.func = "log fold change",
   feature.level = c("Genus"),
   feature.dat.type = "count",
   features.plot = NULL,
   top.k.plot = NULL,
   top.k.func = NULL,
   prev.filter = 0.1,
   abund.filter = 0.001,
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

<figure><img src="../.gitbook/assets/Screenshot 2023-10-11 at 18.18.32.png" alt=""><figcaption><p>Individual Taxa Change Boxplot Pair: Created by the <code>generate_taxa_indiv_change_boxplot_pair()</code> function, this plot visualizes changes in taxonomic composition at the family level. The log-fold-change metric provides a clear depiction of the shifts in abundance over time, offering a more detailed understanding of the dynamics between different conditions.</p></figcaption></figure>

Next, we move onto the second function `generate_taxa_change_boxplot_pair()`:

```R
generate_taxa_change_boxplot_pair(
   data.obj = peerj32.obj,
   subject.var = "subject",
   time.var = "time",
   group.var = "group",
   strata.var = "sex",
   change.base = "1",
   feature.change.func = "log fold change",
   feature.level = c("Genus"),
   feature.dat.type = "count",
   features.plot = NULL,
   top.k.plot = 8,
   top.k.func = "sd",
   prev.filter = 0.1,
   abund.filter = 0.001,
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

<figure><img src="../.gitbook/assets/Screenshot 2023-10-11 at 18.20.52.png" alt=""><figcaption><p>Individual Taxa Change Boxplot Pair with Standard Deviation: Generated by the <code>generate_taxa_change_boxplot_pair()</code> function, this plot also illustrates changes in abundance within paired samples. However, the focus is on the top six taxa based on their standard deviation, providing a unique perspective on the microbial dynamics.</p></figcaption></figure>

Before we dive into the visual interpretation offered by the `generate_taxa_barplot_pair` function, let's touch upon an essential parameter that fundamentally steers the visual output – the `feature.number`.

* `feature.number`: This parameter determines the maximum number of features (or taxa) that will be visualized directly in the barplot. When confronted with datasets teeming with numerous features, it's practical to limit our visual focus to the most abundant or significant taxa, ensuring that the visualization remains informative and isn't cluttered. When the number of taxa surpasses the value defined in `feature.number`, the function smartly aggregates all excess, low-abundance taxa into a collective category labeled "other". This means, for instance, if there are over 20 features in the dataset but `feature.number` is set to 20, the least abundant features that exceed this count will be collectively presented as "other" in the visualization. This approach ensures that the chart remains legible, highlighting the most dominant features, while still accounting for the contributions of less abundant taxa.

With this insight, let's now examine the function and the resultant visuals:

```r
generate_taxa_barplot_pair(
  data.obj = peerj32.obj,
  subject.var = "subject",
  time.var = "time",
  group.var = "group",
  strata.var = "sex",
  feature.level = "Genus",
  feature.dat.type = c("count"),
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

<figure><img src="../.gitbook/assets/Screenshot 2023-10-11 at 18.23.36.png" alt=""><figcaption><p>Individual Stacked Bar Plot of Taxa Composition: Created by the <code>generate_taxa_barplot_pair()</code> function, this visualization displays the changes in taxonomic composition within each subject over time. Connecting lines across the stacked bars emphasize the individual longitudinal journey of the microbiome.</p></figcaption></figure>

<figure><img src="../.gitbook/assets/Screenshot 2023-10-11 at 18.24.47.png" alt=""><figcaption><p>Average Group Stacked Bar Plot of Taxa Composition: Also produced by the <code>generate_taxa_barplot_pair()</code> function, this plot illustrates the average changes in taxonomic composition within groups over time. This representation sheds light on the broader microbial dynamics across different subject groups.</p></figcaption></figure>

The function `generate_taxa_dotplot_pair()` visually evaluates variations in mean abundance and prevalence across distinct groups. In the context of paired samples, this function produces dotplots of each feature's mean abundance and prevalence.

```r
generate_taxa_dotplot_pair(
  data.obj = peerj32.obj,
  subject.var = "subject",
  time.var = "time",
  group.var = "group",
  strata.var = "sex",
  feature.level = c("Family"),
  feature.dat.type = "count",
  features.plot = NULL,
  top.k.plot = NULL,
  top.k.func = NULL,
  prev.filter = 0.01,
  abund.filter = 0.01,
  base.size = 16,
  theme.choice = "bw",
  custom.theme = NULL,
  palette = NULL,
  pdf = TRUE,
  file.ann = NULL,
  pdf.wid = 45,
  pdf.hei = 8.5
)
```

<figure><img src="../.gitbook/assets/Screenshot 2023-06-12 at 20.34.39.png" alt=""><figcaption><p>Dotplot of Mean Abundance and Prevalence Between Paired Groups: Generated by the <code>generate_taxa_dotplot_pair()</code> function, this comprehensive dotplot displays the variation in mean abundance (represented by the size of the dots) and prevalence (indicated by the color intensity) of each taxon at the Family level across different paired groups. This visualization enables a nuanced understanding of significant changes in taxa abundance and prevalence across conditions, highlighting the complex dynamics within the microbiome.</p></figcaption></figure>

The `generate_taxa_change_dotplot_pair()` function visualizes microbiome fluctuations within your dataset. This function allows for the tracking of dynamic changes in taxa abundance across different time points.

An example of how to use this function is as follows:

```r
generate_taxa_change_dotplot_pair(
  data.obj = peerj32.obj,
  subject.var = "subject",
  time.var = "time",
  group.var = "group",
  strata.var = NULL,
  change.base = "2",
  feature.change.func = "log fold change",
  feature.level = "Family",
  feature.dat.type = "count",
  features.plot = NULL,
  top.k.plot = NULL,
  top.k.func = NULL,
  prev.filter = 0.001,
  abund.filter = 0.01,
  base.size = 16,
  theme.choice = "bw",
  custom.theme = NULL,
  pdf = TRUE,
  file.ann = NULL,
  pdf.wid = 11,
  pdf.hei = 8.5
)
```

<figure><img src="../.gitbook/assets/Screenshot 2023-10-08 at 21.12.29.png" alt=""><figcaption><p>Dotplot of Taxa Change Between Paired Groups: Crafted by the <code>generate_taxa_change_dotplot_pair()</code> function, this visualization emphasizes the significant shifts in taxa abundance at the Family level across different time points. The size of the dots corresponds to the change magnitude, dictated by log 2 fold change, while the color intensity indicates the prevalence of each taxon. This detailed depiction accentuates the differing microbial dynamics between groups, inviting a thorough exploration of the complex interactions within the microbiome.</p></figcaption></figure>

Heatmaps are particularly insightful for discerning clusters of microbial families that exhibit similar abundance profiles across different samples. Two critical parameters, `cluster.rows` and `cluster.cols`, control the clustering behavior in these functions:

* `cluster.rows`: By default, the `generate_taxa_heatmap_pair()` function will cluster rows (features or taxa) based on their abundance patterns, set by `cluster.rows = TRUE`. If researchers wish to see the taxa in their original order without clustering, they can achieve this by setting `cluster.rows = FALSE`. However, when clustering is enabled, patterns of microbial families with congruent abundance become readily discernible, painting a vivid picture of microbial dynamics.
* `cluster.cols`: The `generate_taxa_change_heatmap_pair()` function allows for column clustering when `cluster.cols = TRUE`, which can be instrumental in revealing samples that share analogous abundance characteristics. This could be pivotal in unearthing hidden sample groups or conditions that exhibit similar microbial compositions. By default, this function clusters both rows and columns (`cluster.rows = TRUE` and `cluster.cols = TRUE`).

With these parameters in mind, let's look at the implementation of these functions:

The `generate_taxa_heatmap_pair()` function creates a paired heatmap, which visualizes taxonomic shifts between paired design stages. Here is its implementation:

```r
generate_taxa_heatmap_pair(
   data.obj = peerj32.obj,
   subject.var = "subject",
   time.var = "time",
   group.var = "group",
   strata.var = "sex",
   feature.level = c("Family"),
   feature.dat.type = "count",
   features.plot = NULL,
   top.k.plot = NULL,
   top.k.func = NULL,
   prev.filter = 0.1,
   abund.filter = 0.001,
   base.size = 12,
   custom.theme = NULL,
   palette = NULL,
   pdf = TRUE,
   file.ann = NULL,
   pdf.wid = 11,
   pdf.hei = 8.5
)
```

<figure><img src="../.gitbook/assets/Screenshot 2023-10-08 at 21.21.36.png" alt=""><figcaption><p>Individual Taxa Heatmap: This detailed heatmap, created using the <code>generate_taxa_heatmap_pair()</code> function, provides a vibrant visual display of taxa changes at the Family level within individual samples over time. The color intensity indicates the relative abundance of each taxon, revealing shared abundance patterns among different taxa and offering insights into the intricate ecology of the microbiome.</p></figcaption></figure>

The `generate_taxa_change_heatmap_pair()` function helps assist in analyzing patterns of change.

The function demonstrated here:

```
generate_taxa_change_heatmap_pair(
   data.obj = peerj32.obj,
   subject.var = "subject",
   time.var = "time",
   group.var = "group",
   strata.var = "sex",
   change.base = "1",
   feature.change.func = "relative change",
   feature.level = "Family",
   feature.dat.type = "count",
   features.plot = NULL,
   top.k.plot = NULL,
   top.k.func = NULL,
   prev.filter = 0.1,
   abund.filter = 0.001,
   base.size = 12,
   palette = NULL,
   cluster.rows = NULL,
   cluster.cols = FALSE,
   pdf = TRUE,
   file.ann = NULL,
   pdf.wid = 11,
   pdf.hei = 8.5
)
```

<figure><img src="../.gitbook/assets/Screenshot 2023-10-08 at 21.30.23.png" alt=""><figcaption><p>Taxa Change Heatmap: Generated with the <code>generate_taxa_change_heatmap_pair()</code> function, this heatmap reveals the relative changes in taxa abundance at the Family level. The color intensity signifies the magnitude of change, providing an in-depth look at how microbial populations shift over time. Clustering reveals groups of taxa with similar abundance change patterns, shedding light on potential functional or ecological relationships.</p></figcaption></figure>
