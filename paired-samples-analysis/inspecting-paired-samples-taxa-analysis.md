# Feature-level Analysis

In feature-level data analysis for paired samples design, the major tasks include identifying (1) features that change over time, (2) features that are differenital between groups over time,  and (3) features whose changes over time differ between groups. Features in (3) are a subset of features in (2). These tasks can be achieved by `generate_taxa_test_single` and `generate_taxa_change_test_pair`. The `generate_taxa_test_pair` function performs differential abundance analysis (DAA) based on compositional data using the mixed effects mode of LinDA, which is a DAA method based on log linear model with bias correction due to composiitonal effects. If the data are not compositional, standard linear mixed effects model (lme4) will be used.

> Zhou H, He K, Chen J, Zhang X. LinDA: linear models for differential abundance analysis of microbiome compositional data. Genome Biol. 2022 Apr 14;23(1):95. doi: 10.1186/s13059-022-02655-5. PMID: 35421994; PMCID: PMC9012043.

In the `generate_taxa_test_pair` function,  `feature.dat.type` parameter specifies the data type in the feature matrix.  Depending on the data type, different preprocessing steps and methods are used.

* `count` and `proportion`: For count and proportion data, which are both compositional, `linda` will be called. To address zeros, a pseudo-count of 0.5 is added to the count data, and zeros are substituted with half of the nonzero minimum for proportion data (feature-wise).  Winorization at 97% quantile is performed to reduce the influence of outliers.
* `other`:  When `feature.dat.type` is set to "other", the data are considered to be non-compositional and standard linear mixed effects model will be used. Users need to determine the appropriate normalization and transformation of the data before running the function.  This option increases the applicability of MicrobiomeStat to other data types.

The `prev.filter` and `abund.filter` parameters filter taxa based on their prevalence and average relative abundance, so those rare and less abundant taxa will be excluded from testing. As the statistical power for these rare/less abundant taxa tends to be low, excluding them can reduce multiple testing burden. 

The `feature.level` determines what aggregation level(s) the tests will be performed. If `feature.level` is set to "original", the original features in `feature.tab`  will be tested. The `feature.level` can also be set to the aggregated levels specified in the `feature.ann` matrix.

The `generate_taxa_test_pair` outputs a table of LinDA association statistics for all tested taxa/features. The table contains the following components:

* `Variable`: It identifies the taxon/feature being analyzed.
* `Coefficient`: Expressed as log2FoldChange,  bias-corrected. It indicates the degree and direction of change in the abundance of a specific taxon/feature.
* `SE`: Standard errors of the coefficients. It measures the variability of the coefficient estimates.
* `Mean Abundance`: The average abundance of a particular taxon/feature across all samples.
* `Prevalence`: The proportion of samples where a specific taxon/feature is present.
  
The `generate_taxa_change_test_pair` function contains an additional parameter `feature.change.func`, which specifies the method or function used to compute the change between the two time points. The options include:

* `"absolute change"` (default): Computes the absolute difference between the values at the two time points (`value_time_2 - value_time_1`).
* `"log fold change"`: Computes the log2 fold change between the two time points. 
* `"relative change"`: Computes the relative change as `(value_time_2 - value_time_1) / (value_time_2 + value_time_1)`. If both time points have 0 values, the change is defined as 0.
* A custom function: If a user-defined function is provided to calculate the change, it should take two numeric vectors as input corresponding to the values at the two time points (`value_time_1` and `value_time_2`) and return a numeric vector of the computed changes. 

Results from both functions can be visualized using `generate_taxa_volcano_single`, will produce a volcano plot. It visualizes the relationship between the effect size (log foldchange) and its statistical significance. The function has the `feature.sig.level` and `feature.mt.method` parameters:
* `feature.sig.level`: This parameter determines the significance level, influencing the position of the dashed lines in the volcano plot. It sets the threshold for distinguishing between significant and non-significant differences.
* `feature.mt.method`: Thi parameter determines whether the fdr-adjusted p-values or raw p-values will be plotted . There are two options available currently: "fdr" (false discovery rate) and "none" (raw p-value).
  
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

<figure><img src="../.gitbook/assets/Screenshot 2023-10-11 at 16.07.17.png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/Screenshot 2023-10-11 at 16.07.38.png" alt=""><figcaption></figcaption></figure>

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

<figure><img src="../.gitbook/assets/Screenshot 2023-10-11 at 16.37.16.png" alt=""><figcaption></figcaption></figure>

The outcomes enable researchers to recognize taxa that exhibit notable changes in abundance across timepoints, and to determine whether these changes are associated with group affiliations. In the following sections, we will demonstrate how to visualize features of interest from the differential abundance analysis, or other specific features, using various methods.

In the analysis of large-scale microbiome datasets, it's often necessary to concentrate on a subset of taxa that are either most abundant or exhibit significant variability. This focus is facilitated by two parameters, `top.k.plot` and `top.k.func`, which are particularly useful when creating boxplot visualizations.

To include a description of the custom function within the context of the `top.k.plot` parameter, you can modify the paragraph as follows:

The `top.k.plot` parameter specifies the maximum number of taxa to be displayed in visualizations. For instance, setting it to 10 means only the top 10 taxa, as determined by the criteria specified in `top.k.func`, will be visualized. The `top.k.func` parameter offers several predefined choices:

* `"mean"`: Highlights taxa with the highest average abundances spread across samples.
* `"sd"`: Selects taxa with the greatest variability (standard deviation) across samples, useful for identifying taxa with notable differences under varying conditions.
* `"prevalence"`: Chooses taxa with the highest occurrence across samples, targeting those most consistently present.
* Custom function: Users can input their own function to rank taxa based on a numeric vector it returns when applied to the abundance matrix. This allows for custom criteria beyond "mean", "sd", or "prevalence".

Additionally, `top.k.func` can accept a custom function defined by the user. This function should take a matrix of taxa abundances as input and return a numeric vector of values. By using a custom function, researchers can apply unique criteria to rank and select taxa based on specialized research questions or distinct patterns of interest. The top taxa, according to the output of this custom function or the predefined options, will then be selected for visualization up to the number specified by `top.k.plot`.

This tailored approach ensures that the visualizations are most relevant to the user's specific analytical needs and research focus.

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

<figure><img src="../.gitbook/assets/Screenshot 2023-10-11 at 16.50.17.png" alt=""><figcaption></figcaption></figure>

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

<figure><img src="../.gitbook/assets/Screenshot 2023-10-11 at 16.52.12.png" alt=""><figcaption></figcaption></figure>

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

<figure><img src="../.gitbook/assets/Screenshot 2023-10-11 at 18.18.32.png" alt=""><figcaption></figcaption></figure>

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

<figure><img src="../.gitbook/assets/Screenshot 2023-10-11 at 18.20.52.png" alt=""><figcaption></figcaption></figure>

Before we dive into the visual interpretation offered by the `generate_taxa_barplot_pair` function, let's touch upon an essential parameter that fundamentally steers the visual output – the `feature.number`.

* `feature.number`: This parameter determines the maximum number of taxa/features that will be visualized directly in the barplot. For datasets with numerous features, it's practical to limit to the most abundant or significant taxa, ensuring that the visualization remains informative and isn't cluttered. When the number of taxa surpasses the value defined in `feature.number`, the function aggregates low-abundance taxa into a collective category labeled "other". This means, for instance, if there are over 20 features in the dataset but `feature.number` is set to 20, the least abundant features that exceed this count will be collectively presented as "other" in the visualization. This approach ensures that the chart remains legible, highlighting the most dominant features, while still accounting for the contributions of less abundant taxa.

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

<figure><img src="../.gitbook/assets/Screenshot 2023-10-11 at 18.23.36.png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/Screenshot 2023-10-11 at 18.24.47.png" alt=""><figcaption></figcaption></figure>

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

<figure><img src="../.gitbook/assets/Screenshot 2023-06-12 at 20.34.39.png" alt=""><figcaption></figcaption></figure>

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

<figure><img src="../.gitbook/assets/Screenshot 2023-10-08 at 21.12.29.png" alt=""><figcaption></figcaption></figure>

Heatmaps are particularly insightful for discerning clusters of microbial families that exhibit similar abundance profiles across different samples. Two critical parameters, `cluster.rows` and `cluster.cols`, control the clustering behavior in these functions:

* `cluster.rows`: By default, the `generate_taxa_heatmap_pair()` function will cluster rows (taxa/features) based on their abundance patterns, set by `cluster.rows = TRUE`. If researchers wish to see the taxa in their original order without clustering, they can achieve this by setting `cluster.rows = FALSE`. However, when clustering is enabled, patterns of microbial families with congruent abundance become readily discernible, painting a vivid picture of microbial dynamics.
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

<figure><img src="../.gitbook/assets/Screenshot 2023-10-08 at 21.21.36.png" alt=""><figcaption></figcaption></figure>

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

<figure><img src="../.gitbook/assets/Screenshot 2023-10-08 at 21.30.23.png" alt=""><figcaption></figcaption></figure>
