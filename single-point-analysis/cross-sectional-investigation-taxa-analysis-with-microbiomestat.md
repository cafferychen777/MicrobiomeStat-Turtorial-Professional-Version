# Feature-level Analysis

A central task in feature-level analysis is to identify taxa with differential abundance between groups. The `generate_taxa_test_single` function performs differential abundance analysis (DAA) based on compositional data using the LinDA method, which is a DAA method based on log linear model with bias correction due to composiitonal effects.  If the data are not compositional, regular linear regression analysis will be used.

> Zhou H, He K, Chen J, Zhang X. LinDA: linear models for differential abundance analysis of microbiome compositional data. Genome Biol. 2022 Apr 14;23(1):95. doi: 10.1186/s13059-022-02655-5. PMID: 35421994; PMCID: PMC9012043.

In the `generate_taxa_test_single` function,  `feature.dat.type` parameter specifies the data type in the feature matrix.  Depending on the data type, different preprocessing steps and methods are used.

* `count` and `proportion`: For count and proportion data, which are both compositional, `linda` will be called. To address zeros, a pseudo-count of 0.5 is added to the count data, and zeros are substituted with half of the nonzero minimum for proportion data (feature-wise).  Winorization at 97% quantile is performed to reduce the influence of outliers.
* `other`:  When `feature.dat.type` is set to "other", the data are considered to be non-compositional and ordinary linear regression analysis will be performed. Users need to determine the appropriate normalization and transformation of the data before running the function.  This option increases the applicability of MicrobiomeStat to other data types.

The `prev.filter` and `abund.filter` parameters filter taxa based on their prevalence and average relative abundance, so those rare and less abundant taxa will be excluded from testing. As the statistical power for these rare/less abundant taxa tends to be low, excluding them can reduce multiple testing burden. 

The `feature.level` determines what aggregation level(s) the tests will be performed. If `feature.level` is set to "original", the original features in `feature.tab`  will be tested. The `feature.level` can also be set to the aggregated levels specified in the `feature.ann` matrix.

The `generate_taxa_test_single` outputs a table of LinDA association statistics for all tested taxa/features. The table contains the following components:

* `Variable`: It identifies the taxon/feature being analyzed.
* `Coefficient`: Expressed as log2FoldChange,  bias-corrected. It indicates the degree and direction of change in the abundance of a specific taxon/feature.
* `SE`: Standard errors of the coefficients. It measures the variability of the coefficient estimates.
* `Mean Abundance`: The average abundance of a particular taxon/feature across all samples.
* `Prevalence`: The proportion of samples where a specific taxon/feature is present.
  
`generate_taxa_volcano_single` will produce a volcano plot based on the `generate_taxa_test_single` output. It visualizes the relationship between the effect size (log foldchange) and its statistical significance. The function has the `feature.sig.level` and `feature.mt.method` parameters:
* `feature.sig.level`: This parameter determines the significance level, influencing the position of the dashed lines in the volcano plot. It sets the threshold for distinguishing between significant and non-significant differences.
* `feature.mt.method`: Thi parameter determines whether the fdr-adjusted p-values or raw p-values will be plotted . There are two options available currently: "fdr" (false discovery rate) and "none" (raw p-value). 

Following shows an example:

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

#### First 10 Rows of Differential Abundance Results at Genus Level

<table><thead><tr><th>Variable</th><th width="135">Coefficient</th><th width="54">SE</th><th>P.Value</th><th>Adjusted.P.Value</th><th>Mean.Abundance</th><th>Prevalence</th></tr></thead><tbody><tr><td>Actinomycetaceae</td><td>0.43088737</td><td>0.8125191</td><td>0.60204023</td><td>0.9892442</td><td>0.0001950405</td><td>0.7272727</td></tr><tr><td>Aerococcus</td><td>-0.09734179</td><td>0.7893169</td><td>0.90314569</td><td>0.9892442</td><td>0.0002352668</td><td>0.5909091</td></tr><tr><td>Aeromonas</td><td>0.02022775</td><td>1.0311377</td><td>0.98455351</td><td>0.9892442</td><td>0.0002829477</td><td>0.6363636</td></tr><tr><td>Akkermansia</td><td>-0.70914707</td><td>0.5667601</td><td>0.22603834</td><td>0.9892442</td><td>0.0202212889</td><td>1.0000000</td></tr><tr><td>Allistipes et rel.</td><td>-0.54628088</td><td>0.4985462</td><td>0.28688461</td><td>0.9892442</td><td>0.0083365762</td><td>1.0000000</td></tr><tr><td>Anaerofustis</td><td>0.37736758</td><td>0.3988262</td><td>0.35592789</td><td>0.9892442</td><td>0.0013725489</td><td>1.0000000</td></tr><tr><td>Anaerostipes caccae et rel.</td><td>-0.45297144</td><td>0.1723513</td><td>0.01655715</td><td>0.7004574</td><td>0.0185944926</td><td>1.0000000</td></tr><tr><td>Anaerotruncus colihominis et rel.</td><td>0.05420558</td><td>0.4605402</td><td>0.90754073</td><td>0.9892442</td><td>0.0028354658</td><td>1.0000000</td></tr><tr><td>Anaerovorax odorimutans et rel.</td><td>-0.55577241</td><td>0.3180729</td><td>0.09672749</td><td>0.9892442</td><td>0.0044828621</td><td>1.0000000</td></tr><tr><td>Aneurinibacillus</td><td>-0.20493605</td><td>0.5227462</td><td>0.69939327</td><td>0.9892442</td><td>0.0007296989</td><td>0.9545455</td></tr></tbody></table>

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


Next, we will introduce functions to plot the taxa/features data. They can be used to visualize specific taxa/features, for example, those selected from differential abundance anlysis. Or they can be used to visualize all taxa with some basic filtering. The first function is `generate_taxa_boxplot_single`, which generate boxplots of abundance data. It has the following relevant parameters:
* `feature.dat.type`: One of "count", "proportion" or "other".  For "count", the data will be converted to proportion data before the visualization. 
* `transform`: This parameter indicates the transformation to apply to the abundance data when plotting. Transformations are only applied when the `feature.dat.type` is set to either "count" or "proportion".  When  `feature.dat.type` is "other",  no transformation will be performed. User should deterimne the appropriate transformation to better visualize the data. The available options for `transform` include:
  * `"identity"`: No transformation (default)
  * `"sqrt"`: Square root transformation
  * `"log"`: Logarithmic transformation. Zeros are replaced with half of the non-zero minimum  for each taxon before log transformation.
* `feature.level`: Specifiy which level of the data to be plotted. Same meaning as in  `generate_taxa_test_single`.
* `features.plot`: This parameter can be used in all feature-level visualization functions to specify which taxa or features should be visualized. This is particularly useful for focusing on the results of differential abundance analyses. When you provide a vector of taxa or feature names to `features.plot`, this will directly select these features for visualization, overriding any settings in `prev.filter` and `abund.filter`. By using this parameter, you can directly highlight and examine the taxa or features that are significantly different in abundance across your comparisons.
* `top.k.plot` and `top.k.func`: In many scenarios, especially when navigating through expansive datasets, you may want to focus on a select subset of taxa that stand out either due to their high abundance or  large variability. `top.k.plot` lets you visualize the top k taxa/features based on the criterion you selected in  `top.k.func`, which have four choices:
  * `"mean"`: Highlights taxa with the highest average abundances across samples.
  * `"sd"`: Selects taxa with the greatest variability (standard deviation) across samples, useful for identifying taxa with notable differences under varying conditions.
  * `"prevalence"`: Chooses taxa with the highest occurrence across samples, targeting those most consistently present.
  * `top.k.func` can also accept a user-defined function so that the users can rank taxa based on their own criterion.  The user-defined function should take the abundance matrix (those in `feature.tab` and `feature.agg.list`) as the input and returns a numeric vector of the feature importance values. This allows for criteria beyond "mean", "sd", or "prevalence".

  Certainly. Here's the revised content in English, maintaining consistency with the context and using more professional and concise language:

After performing differential abundance analysis with `generate_taxa_test_single` and visualizing results with `generate_taxa_volcano_single`, we can further explore phylogenetic relationships and abundance patterns using the `generate_taxa_cladogram_single` function. This function creates a circular cladogram with an integrated heatmap.

Key features of `generate_taxa_cladogram_single`:

1. Visualization of phylogenetic relationships
2. Integrated heatmap of abundance or coefficient data
3. Simultaneous visualization of multiple taxonomic levels
4. Filtering based on statistical significance
5. Customizable color-coding of cladogram branches by taxonomic level

Example usage:

```r
plot.list <- generate_taxa_cladogram_single(
  data.obj = peerj32.obj,
  test.list = test.list,
  group.var = "group",
  feature.level = c("Phylum","Genus","Family"),
  feature.mt.method = "none",
  cutoff = 0.9,
  color.group.level = "Family"
)
```

Key parameters:
- `color.group.level`: Taxonomic level for branch color-coding

The function returns a list of ggplot objects, one for each comparison in the data. These plots provide a holistic view of the microbiome data, combining phylogenetic information, abundance patterns, and statistical significance.

<figure><img src="../.gitbook/assets/cladogram.png" alt=""><figcaption></figcaption></figure>

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

<figure><img src="../.gitbook/assets/Screenshot 2023-10-10 at 21.03.21.png" alt=""><figcaption></figcaption></figure>

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

<figure><img src="../.gitbook/assets/Screenshot 2023-08-05 at 20.04.16.png" alt=""><figcaption></figcaption></figure>

Besides boxplots, MicrobiomeStat can also generate heatmaps using the `generate_taxa_heatmap_single` function.  Heatmaps are particularly insightful for discerning clusters of taxa/features that exhibit similar abundance profiles across amples. By default, the function will only cluster rows (taxa/features) based on their abundance patterns. The columns are ordered by `group.var` and `strata.var`. If users wish to see the taxa in their original order without clustering, they can achieve this by setting `cluster.rows = FALSE`.  The function also allows for column clustering by setting `cluster.cols = TRUE`, which can be instrumental in revealing groups of samples with similar abundance profiles. Note that when `feature.dat.type` is "count” or "proportion", the data will be visualized using proportions. For "other" type, the original scale will be used. Following is an example:

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

<figure><img src="../.gitbook/assets/Screenshot 2023-10-10 at 21.16.15.png" alt=""><figcaption></figcaption></figure>

We can use `generate_taxa_dotplot_single` function as an alternative to the heatmap. Here is the example:

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

<figure><img src="../.gitbook/assets/Screenshot 2023-10-10 at 21.34.06.png" alt=""><figcaption></figcaption></figure>

We can use `generate_taxa_barplot_single` function to generate starked bar plots of taxa. The function has an important parameter `feature.number`.

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

<figure><img src="../.gitbook/assets/Screenshot 2023-10-10 at 21.37.34.png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/Screenshot 2023-10-10 at 21.38.40.png" alt=""><figcaption></figcaption></figure>
