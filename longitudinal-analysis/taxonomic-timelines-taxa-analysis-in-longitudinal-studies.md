# Feature-level Analysis

This guide will walk you through the process of investigating how taxonomic composition changes within a longitudinal context, providing insights into microbial dynamics over time.

MicrobiomeStat provides a set of functions to illuminate the intricate taxonomic changes over time. The `generate_taxa_trend_test_long()` function executes a trend test on longitudinal microbiome data. It commences with the normalization of count data, followed by the generation of a mixed-effects model that incorporates time, group, and subject variables. Special attention is given to the interaction between time and group variables. Finally, the function applies the `linda()` method to discern whether these variables significantly influence taxa abundance.

The `feature.dat.type` parameter plays a crucial role in the data preprocessing phase:

* `"count"`: For raw count data, the function first performs sparsity treatment, followed by a Total Sum Scaling (TSS) normalization. This process ensures that the data is suitably normalized and comparable across samples.
* `"proportion"`: Data presented as proportions remains unaltered.However, it's worth noting that during the LinDA differential abundance analysis, zeroes in the dataset are substituted with half of the smallest non-zero count for each feature. This adjustment is done to mitigate the impact of zero-inflation.
* `"other"`: In scenarios where the data originates from non-compositional sources, like transcriptomics and metabolomics, a different data transformation approach might be more appropriate. When `feature.dat.type` is set to "other", the function does not perform any normalization or scaling operations, allowing users to apply domain-specific transformations if necessary.

Further enhancing data robustness, the `prev.filter` and `abund.filter` parameters filter taxa based on prevalence and average abundance, respectively. Specifically:

* `prev.filter` targets taxa retention based on their prevalence across samples.
* `abund.filter` focuses on the average abundance of taxa across all samples.

Such filtering ensures that the analysis centers on taxa both prevalent and abundant, enhancing result reliability by excluding potential outliers or noise.

For those directly analyzing entities like OTU, ASV, Gene, KEGG, etc., that don't require aggregation, it's recommended to set the `feature.level` parameter to "original". Or we can perform testing on aggregated levels such as "Phylum", "Family" and "Genus" levels for microbiome data.

Furthermore, when interpreting the results, it's essential to understand `feature.sig.level` and `feature.mt.method` parameters:

* `feature.sig.level`: This parameter determines the significance level, primarily influencing the position of the dashed lines in the volcano plot. It sets the threshold for distinguishing between significant and non-significant differences in taxa abundance.
* `feature.mt.method`: There are two options available for this parameter: "fdr" (False Discovery Rate) and "none". Regardless of how this parameter is set, it's crucial to note that the `generate_taxa_test_single` function always performs adjustments post-testing. However, the `feature.mt.method` specifically influences the visualization in the volcano plot.

Another important parameter is `feature.change.func`, which specifies the method or function used to compute the change between two time points. The options include:

* `"absolute change"` (default): Computes the absolute difference between the values at the two time points (`value_time_2` and `value_time_1`).
* `"log fold change"`: Computes the log2 fold change between the two time points. For zero values, imputation is performed using half of the minimum nonzero value for each feature level at the respective time point before taking the logarithm.
* `"relative change"`: Computes the relative change as `(value_time_2 - value_time_1) / (value_time_2 + value_time_1)`. If both time points have a value of 0, the change is defined as 0.
* A custom function: If a user-defined function is provided, it should take two numeric vectors as input corresponding to the values at the two time points (`value_time_1` and `value_time_2`) and return a numeric vector of the computed change. This custom function will be applied directly to calculate the difference.

By understanding and appropriately setting these parameters, users can ensure a more accurate and contextually relevant interpretation of the plotted results.

```r
data("subset_T2D.obj")
test.list <- generate_taxa_trend_test_long(
  data.obj = subset_T2D.obj,
  subject.var = "subject_id",
  time.var = "visit_number_num",
  group.var = "subject_race",
  adj.vars = "sample_body_site",
  prev.filter = 0.1,
  abund.filter = 0.001,
  feature.level = c("Family"),
  feature.dat.type = c("count")
)
plot.list <- generate_taxa_trend_volcano_long(
  data.obj = subset_T2D.obj,
  group.var = "subject_race",
  time.var = "visit_number_num",
  test.list = test.list,
  feature.sig.level = 0.1,
  feature.mt.method = "none"
)
```

<figure><img src="../.gitbook/assets/Screenshot 2023-10-12 at 19.37.52.png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/Screenshot 2023-10-12 at 19.38.25.png" alt=""><figcaption></figcaption></figure>

The function `generate_taxa_volatility_test_long()` is designed to analyze longitudinal microbiome data by calculating the volatility of taxa abundances and testing for an association with a grouping variable. Prior to the calculation of volatility, a centered log-ratio (CLR) transformation is performed on the data.

After the transformation, the volatility, denoted as 'V', is computed as the mean rate of change in abundance over time, defined as:

$$
V = \frac{1}{n} \sum_{i=1}^{n} \left| \frac{A(t_{i+1}) - A(t_i)}{t_{i+1} - t_i} \right|
$$

where 'A(t)' is the abundance at time 't' and 'Δt = t\_{i+1} - t\_i' is the time difference.

This function fits a linear model to the volatility data with the formula 'V = β\_0 + β\_1\_G + β\_2\_X + ε', where 'V' is the volatility, 'G' is the group variable, 'X' represents adjustment variables, and 'ε' is the error term. If the group variable is multi-categorical, an ANOVA test is also performed to test the overall significance of the group variable, while considering the adjustment variables.

The function is applied as follows:

```r
data("subset_T2D.obj")
test.list <- generate_taxa_volatility_test_long(
  data.obj = subset_T2D.obj,
  time.var = "visit_number_num",
  subject.var = "subject_id",
  group.var = "subject_race",
  adj.vars = "sample_body_site",
  prev.filter = 0.1,
  abund.filter = 0.001,
  feature.mt.method = "fdr",
  feature.sig.level = 0.1,
  feature.level = c("Species"),
  feature.dat.type = "count",
  transform = "CLR"
)
plot.list <- generate_taxa_volatility_volcano_long(
  data.obj = subset_T2D.obj,
  group.var = "subject_race",
  test.list = test.list,
  feature.sig.level = 0.1,
  feature.mt.method = "none"
)
```

<figure><img src="../.gitbook/assets/Screenshot 2023-10-12 at 20.04.46.png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/Screenshot 2023-10-12 at 20.05.53.png" alt=""><figcaption></figcaption></figure>

To extend our analysis further, we use the `generate_taxa_association_test_long` function. This function analyzes the microbiome data by treating longitudinal data as cross-sectional, effectively ignoring the time component. It is especially useful in situations where the focus is on understanding the overall association between microbiome composition and other variables, without considering the fluctuations over time. This approach can yield more robust results by diminishing the influence of time-point specific variations on the microbiome.

```r
data("subset_T2D.obj")
test.list_T2D <- generate_taxa_association_test_long(
  data.obj = subset_T2D.obj,
  subject.var = "subject_id",
  feature.level = "Genus",
  group.var = "subject_race",
  feature.dat.type = c("count"),
  prev.filter = 0.1,
  abund.filter = 0.001
)

volcano_plots_T2D <- generate_taxa_volcano_single(
  data.obj = subset_T2D.obj,
  group.var = "subject_race",
  test.list = test.list_T2D,
  feature.sig.level = 0.1,
  feature.mt.method = "none"
)
```

<figure><img src="../.gitbook/assets/Screenshot 2024-01-18 at 18.07.50.png" alt=""><figcaption></figcaption></figure>

Building on this, we also use the `generate_taxa_per_time_test_long` function to analyze the microbiome data at each individual time point. This function performs a subset analysis on the dataset, followed by a statistical test using the linda method for each time point. It enables a detailed investigation of taxa changes over time, considering various factors such as subject characteristics and sample sites.

```r
result2 <- generate_taxa_per_time_test_long(
  data.obj = subset_T2D.obj,
  subject.var = "subject_id",
  time.var = "visit_number",
  group.var = "subject_race",
  adj.vars = "sample_body_site",
  prev.filter = 0.1,
  abund.filter = 0.001,
  feature.level = c("Genus", "Family"),
  feature.dat.type = "count"
)
```

To effectively visualize these results, we employ the `generate_taxa_per_time_dotplot_long` function. This function creates dot plots that provide a clear and intuitive visualization of the taxa changes at different time points, facilitating an understanding of the temporal dynamics within the dataset.

```r
dotplot_T2D <- generate_taxa_per_time_dotplot_long(
  data.obj = subset_T2D.obj,
  test.list = result2,
  group.var = "subject_race",
  time.var = "visit_number",
  feature.level = c("Genus", "Family")
)
```

<figure><img src="../.gitbook/assets/Screenshot 2024-01-18 at 17.38.01.png" alt=""><figcaption></figcaption></figure>

These methods, including both the `generate_taxa_per_time_test_long` and `generate_taxa_per_time_dotplot_long`, enhance our ability to scrutinize and understand the intricate patterns and variations in taxa abundance throughout the course of the Type 2 Diabetes study, thereby enriching our analysis and insights.

Further advancing our analysis, we introduce the `generate_taxa_change_test_long` function. This function is designed to analyze the change in taxa abundance relative to a baseline time point (`t0.level`). It provides insights into the changes in microbiome composition from the baseline to subsequent time points, considering the diversity at different taxonomic levels such as Genus and Family.

```r
result <- generate_taxa_change_test_long(
  data.obj = subset_T2D.obj,
  subject.var = "subject_id",
  time.var = "visit_number",
  t0.level = unique(subset_T2D.obj$meta.dat$visit_number)[1],
  ts.levels = unique(subset_T2D.obj$meta.dat$visit_number)[-1],
  group.var = "subject_race",
  adj.vars = "sample_body_site",
  prev.filter = 0.1,
  abund.filter = 0.001,
  feature.level = c("Genus", "Family"),
  feature.dat.type = "count"
)
```

The results from this function can be visualized using `generate_taxa_per_time_dotplot_long`, which provides a comprehensive view of the taxa changes from the baseline across different time points. This visualization aids in identifying significant shifts in the microbiome composition over the course of the study.

```r
dotplot_T2D <- generate_taxa_per_time_dotplot_long(
  data.obj = subset_T2D.obj,
  test.list = result,
  t0.level = unique(subset_T2D.obj$meta.dat$visit_number)[1],
  ts.levels = unique(subset_T2D.obj$meta.dat$visit_number)[-1],
  group.var = "subject_race",
  time.var = "visit_number",
  feature.level = c("Genus", "Family")
)
```

<figure><img src="../.gitbook/assets/Screenshot 2024-01-18 at 17.52.59.png" alt=""><figcaption></figcaption></figure>

To visualize the abundance trajectories of specific taxa across multiple timepoints, the function `generate_taxa_areaplot_long()` can be used. Let's touch upon an essential parameter that fundamentally steers the visual output – the `feature.number`.

* `feature.number`: This parameter determines the maximum number of taxa/features that will be visualized directly in the barplot. For datasets with numerous features, it's practical to limit to the most abundant or significant taxa, ensuring that the visualization remains informative and isn't cluttered. When the number of taxa surpasses the value defined in `feature.number`, the function aggregates low-abundance taxa into a collective category labeled "other". This means, for instance, if there are over 20 features in the dataset but `feature.number` is set to 20, the least abundant features that exceed this count will be collectively presented as "other" in the visualization. This approach ensures that the chart remains legible, highlighting the most dominant features, while still accounting for the contributions of less abundant taxa.

```r
generate_taxa_areaplot_long(
  data.obj = ecam.obj,
  subject.var = "studyid", 
  time.var = "month",
  group.var = "diet",
  strata.var = "antiexposedall",
  feature.level = "Family",
  feature.dat.type = "proportion",  
  feature.number = 8,
  t0.level = unique(ecam.obj$meta.dat$month)[1],
  ts.levels = unique(ecam.obj$meta.dat$month)[-1],
  base.size = 10,
  theme.choice = "bw",
  palette = NULL,
  pdf = TRUE,
  file.ann = NULL
)
```

<figure><img src="../.gitbook/assets/Screenshot 2023-08-05 at 14.28.37.png" alt=""><figcaption></figcaption></figure>

This longitudinal area plot allows us to track the changes of specific taxa over time, comparing trajectories between groups.

For insights into the taxonomic composition within groups at each timepoint, `generate_taxa_barplot_long()` can be utilized:

```r
generate_taxa_barplot_long(
  data.obj = ecam.obj,
  subject.var = "studyid",
  time.var = "month", 
  group.var = "delivery",
  strata.var = "diet",
  feature.level = "Family",
  feature.dat.type = "proportion",
  feature.number = 10, 
  t0.level = NULL,
  ts.levels = NULL,
  theme.choice = "bw",
  palette = NULL,
  pdf = TRUE,
  file.ann = NULL
)
```

<figure><img src="../.gitbook/assets/Screenshot 2023-08-05 at 14.33.16.png" alt=""><figcaption></figcaption></figure>

In the context of large-scale microbiome datasets, it is often beneficial to focus on a subset of taxa that are either abundant or exhibit significant variability. The parameters `top.k.plot` and `top.k.func` facilitate this focus:

* `top.k.func` assists in determining the criteria for this selection, offering three choices:
  * `"mean"`: Highlights taxa with the highest average abundances spread across samples.
  * `"sd"`: Selects taxa with the greatest variability (standard deviation) across samples, useful for identifying taxa with notable differences under varying conditions.
  * `"prevalence"`: Chooses taxa with the highest occurrence across samples, targeting those most consistently present.
  * Custom function: Users can input their own function to rank taxa based on a numeric vector it returns when applied to the abundance matrix. This allows for custom criteria beyond "mean", "sd", or "prevalence".

These parameters are particularly effective when generating heatmaps, as they enable concentration on the most pertinent taxa for the research question at hand.

Heatmaps are particularly insightful for discerning clusters of microbial families that exhibit similar abundance profiles across different samples. Two critical parameters, `cluster.rows` and `cluster.cols`, control the clustering behavior in these functions:

* `cluster.rows`: By default, the `generate_taxa_heatmap_long()` function will cluster rows (taxa/features) based on their abundance patterns, set by `cluster.rows = TRUE`. If researchers wish to see the taxa in their original order without clustering, they can achieve this by setting `cluster.rows = FALSE`. However, when clustering is enabled, patterns of microbial families with congruent abundance become readily discernible, painting a vivid picture of microbial dynamics.
* `cluster.cols`: The `generate_taxa_change_heatmap_long()` function allows for column clustering when `cluster.cols = TRUE`, which can be instrumental in revealing samples that share analogous abundance characteristics. This could be pivotal in unearthing hidden sample groups or conditions that exhibit similar microbial compositions. By default, this function clusters rows (`cluster.rows = TRUE`) and does not cluster columns (`cluster.cols = FALSE`).

With these parameters in mind, let's look at the implementation of these functions:

To uncover intricate abundance change patterns, `generate_taxa_change_heatmap_long()` can be used:

```r
generate_taxa_change_heatmap_long(
  data.obj = ecam.obj,
  subject.var = "studyid",
  time.var = "month_num",
  t0.level = NULL,
  ts.levels = NULL,  
  group.var = "antiexposedall",
  strata.var = "diet",
  feature.level = c("Family"),
  feature.dat.type = "proportion",
  features.plot = NULL,
  cluster.rows = NULL,
  cluster.cols = NULL,
  top.k.plot = 15,
  top.k.func = "sd",
  feature.change.func = "log fold change",
  palette = NULL,
  prev.filter = 0.01,
  abund.filter = 0.01,
  pdf = TRUE,
  file.ann = NULL  
)
```

<figure><img src="../.gitbook/assets/Screenshot 2023-10-12 at 18.58.00.png" alt=""><figcaption></figcaption></figure>

For a comprehensive heatmap of taxonomic composition, `generate_taxa_heatmap_long()` can be employed:

```r
generate_taxa_heatmap_long(
  data.obj = ecam.obj,
  subject.var = "studyid",
  time.var = "month_num",
  t0.level = NULL,
  ts.levels = NULL,
  group.var = "delivery",
  strata.var = "diet",
  feature.level = "Family",
  feature.dat.type = "proportion",
  features.plot = NULL,
  top.k.plot = NULL,
  top.k.func = NULL,
  prev.filter = 0.01,
  abund.filter = 0.01,
  cluster.rows = NULL,
  cluster.cols = NULL,
  pdf = TRUE,
  file.ann = NULL,
  pdf.wid = 11,
  pdf.hei = 8.5
)
```

<figure><img src="../.gitbook/assets/Screenshot 2023-10-12 at 19.00.58.png" alt=""><figcaption><p>Longitudinal Microbiome Composition Heatmap: This heatmap visualizes the longitudinal taxonomic composition at the family level. Generated via <code>generate_taxa_heatmap_long()</code>, color intensity represents abundance. It provides an intuitive overview of community dynamics over time, stratified by delivery mode and diet. Filtering focuses the analysis on relevant families. The plot enables visualization of temporal abundance patterns.</p></figcaption></figure>

For spaghetti plots showcasing individual trajectories, `generate_taxa_indiv_spaghettiplot_long()` can be used:

```R
generate_taxa_indiv_spaghettiplot_long(
  data.obj = ecam.obj,
  subject.var = "studyid",
  time.var = "month_num", 
  t0.level = NULL,
  ts.levels = NULL,
  group.var = "diet",
  strata.var = "antiexposedall",
  feature.level = c("Phylum"),
  features.plot = NULL,
  feature.dat.type = "proportion",
  top.k.plot = 5,
  top.k.func = "mean",
  prev.filter = 0.01,
  abund.filter = 0.01,
  base.size = 16,
  theme.choice = "bw",  
  palette = NULL,
  pdf = TRUE,
  file.ann = NULL
)
```

<figure><img src="../.gitbook/assets/Screenshot 2023-10-12 at 20.06.40.png" alt=""><figcaption><p>Individual Phylum Abundance Spaghetti Plots: This function generates spaghetti plots showing longitudinal abundance trajectories for specific phyla within each individual. It focuses on the top 5 phyla by mean abundance. Each line represents one subject's phylum abundance over time. These plots enable analysis of individual temporal dynamics.</p></figcaption></figure>

To assess group changes, `generate_taxa_spaghettiplot_long()` can be used:

```R
generate_taxa_spaghettiplot_long(
  data.obj = ecam.obj,
  subject.var = "studyid",
  time.var = "month_num",
  t0.level = NULL,
  ts.levels = NULL,
  group.var = "diet",
  strata.var = "antiexposedall",
  feature.level = c("Phylum"),
  features.plot = NULL,
  feature.dat.type = "proportion",
  top.k.plot = 3,
  top.k.func = "mean",
  prev.filter = 0,
  abund.filter = 0,
  base.size = 16,
  theme.choice = "bw",
  palette = NULL,
  pdf = TRUE,
  file.ann = NULL  
)
```

<figure><img src="../.gitbook/assets/Screenshot 2023-08-05 at 15.04.49.png" alt=""><figcaption></figcaption></figure>

Before delving into the specifics of the `generate_taxa_boxplot_long()` function, it's worth noting the `transform` parameter. This string indicates the transformation to apply to the axis when plotting. The options are:

* `"identity"`: No transformation (default)
* `"sqrt"`: Square root transformation
* `"log"`: Logarithmic transformation. Zeros are replaced with half of the minimum non-zero value for each taxon before log transformation.

Now, for an in-depth look at the distribution of specific phyla over time, `generate_taxa_boxplot_long()` can be used:

```r
generate_taxa_boxplot_long(
  data.obj = subset_T2D.obj,
  subject.var = "subject_id",
  time.var = "visit_number_num",
  t0.level = NULL,
  ts.levels = NULL,  
  group.var = "subject_race",
  strata.var = NULL,
  feature.level = c("Family"),
  features.plot = NULL,
  top.k.plot = 8,
  top.k.func = "sd",
  feature.dat.type = "count",
  transform = "sqrt",
  prev.filter = 0.01,
  abund.filter = 0.001,
  base.size = 12,
  theme.choice = "bw",
  custom.theme = NULL,
  palette = NULL,
  pdf = TRUE,  
  file.ann = NULL,
  pdf.wid = 11,
  pdf.hei = 8.5
)
```

<figure><img src="../.gitbook/assets/Screenshot 2023-10-12 at 18.56.27.png" alt=""><figcaption></figcaption></figure>

To construct detailed individual boxplots, `generate_taxa_indiv_boxplot_long()` can be used:

```R
generate_taxa_indiv_boxplot_long(
    data.obj = ecam.obj,
    subject.var = "studyid",
    time.var = "month",
    t0.level = unique(ecam.obj$meta.dat$month)[1],
    ts.levels = unique(ecam.obj$meta.dat$month)[2:5],
    group.var = "diet",  
    strata.var = NULL,
    feature.level = c("Phylum"),
    feature.dat.type = "proportion",
    transform = "log",
    prev.filter = 0.01,
    abund.filter = 0.01,
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

<figure><img src="../.gitbook/assets/Screenshot 2023-10-12 at 20.08.07.png" alt=""><figcaption></figcaption></figure>
