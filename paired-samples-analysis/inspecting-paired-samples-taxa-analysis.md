---
description: >-
  This tutorial focuses on techniques for differential abundance analysis,
  highlighting significant changes in microbial abundance between paired
  conditions.
---

# Inspecting Paired Samples: Feature-level Analysis

In **Inspecting Paired Samples: Taxa Analysis**, we focus on the variations within microbial ecosystems, particularly taxa variations. **MicrobiomeStat** provides tools for differential abundance analysis, enabling us to study significant changes in microbial abundance between paired conditions.

Our goal is to understand patterns in taxa composition, prevalence, abundance, and changes, and to identify samples with similar trends of taxa variation. The tutorial will guide you through the process of analyzing your microbial data and understanding the complexity of microbial communities.

Before visual exploration, statistical tests are performed to identify differentially abundant taxa. The function `generate_taxa_test_pair()` employs the LinDA method to facilitate differential abundance analysis at the feature level. This method is essential for discerning taxa with distinct abundance and change trend between groups.

> Zhou H, He K, Chen J, Zhang X. LinDA: linear models for differential abundance analysis of microbiome compositional data. Genome Biol. 2022 Apr 14;23(1):95. doi: 10.1186/s13059-022-02655-5. PMID: 35421994; PMCID: PMC9012043.

The `feature.dat.type` parameter plays a crucial role in the data preprocessing phase:

* `"count"`: For raw count data, the function first performs sparsity treatment, followed by a Total Sum Scaling (TSS) normalization. This process ensures that the data is suitably normalized and comparable across samples.
* `"proportion"`: Data presented as proportions remains unaltered.
* `"other"`: In scenarios where the data originates from non-microbiome sources, like single-cell studies, spatial transcriptomics, KEGG pathways, or gene data, a different data transformation approach might be more apt. When `feature.dat.type` is set to `"other"`, the function refrains from any normalization or scaling operations, allowing users to apply domain-specific transformations if necessary.

Further enhancing data robustness, the `prev.filter` and `abund.filter` parameters filter taxa based on prevalence and average abundance, respectively. Specifically:

* `prev.filter` targets taxa retention based on their prevalence across samples.
* `abund.filter` focuses on the average abundance of taxa across all samples.

Such filtering ensures that the analysis centers on taxa both prevalent and abundant, enhancing result reliability by excluding potential outliers or noise.

For those directly analyzing entities like OTU, ASV, Gene, KEGG, etc., that don't require aggregation, it's recommended to set the `feature.level` parameter to `"original"`.

Furthermore, when interpreting the results, it's essential to understand the role of `feature.sig.level` and `feature.mt.method` parameters:

* `feature.sig.level`: This parameter determines the significance level, primarily influencing the position of the dashed lines in the volcano plot. It sets the threshold for distinguishing between significant and non-significant differences in taxa abundance.
* `feature.mt.method`: There are two options available for this parameter: `"fdr"` (False Discovery Rate) and `"none"`. Regardless of how this parameter is set, it's crucial to note that the test function always performs adjustments post-testing. However, the `feature.mt.method` specifically influences the visualization in the volcano plot, guiding how p-values are adjusted in that context.

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

```

<figure><img src="../.gitbook/assets/Screenshot 2023-10-11 at 16.07.17.png" alt=""><figcaption><p>Volcano plot for main effect presents the volcano plot for the Main Effect, reflecting the primary differences in taxa abundance between the compared groups, excluding the influence of other factors. Each point represents a genus, with its position on the x-axis indicating the log fold change and on the y-axis indicating the adjusted p-value (-log10). Points located at the top and on either side of the vertical line represent genera with statistically significant differences in abundance.</p></figcaption></figure>

<figure><img src="../.gitbook/assets/Screenshot 2023-10-11 at 16.07.38.png" alt=""><figcaption><p>Volcano plot for interaction effect displays the volcano plot for the Interaction Effect, revealing the relationship between group differences and the time variable. It indicates if the difference in taxa abundance between groups varies depending on other factors in the model. Each point represents a genus, with its position on the x-axis indicating the log fold change and on the y-axis indicating the adjusted p-value (-log10). Points located at the top and on either side of the vertical line represent genera with statistically significant differences in abundance.</p></figcaption></figure>

For specific comparisons, labels like `$Genus$Placebo vs LGG (Reference) [Main Effect]` and `$Genus$Placebo vs LGG (Reference) [Interaction]` may appear.

* The `[Main Effect]` label indicates the primary difference in taxa abundance between the compared groups, excluding the influence of other factors.
* The `[Interaction]` label reveals the relationship between group differences and time variable. It indicates if the difference in taxa abundance between groups varies depending on other factors in the model.

If the focus is specifically to explore the shifts in taxa abundance across distinct timepoints, the `generate_taxa_change_test_pair()` function is useful. The function not only assesses the changes but also highlights the differences between groups.

```r
generate_taxa_change_test_pair(
  data.obj = peerj32.obj,
  subject.var = "subject",
  time.var = "time",
  group.var = "group",
  adj.vars = c("sex"),
  change.base = "1",
  feature.change.func = "log fold change",
  feature.level = "Family",
  prev.filter = 0.01,
  abund.filter = 0.01,
  feature.dat.type = "count"
)
```

The comprehensive results allow researchers to identify taxa that have significant alterations in abundance across timepoints, and if these changes are influenced by group affiliations. This analysis paves the way for insightful visual representations or more intricate explorations.

After the Differential Abundance Analysis, we introduce two other functions: `generate_taxa_indiv_boxplot_long()` and `generate_taxa_boxplot_long()`. These functions provide an in-depth visualization analysis for individual taxa.

First, we look at `generate_taxa_indiv_boxplot_long()`:

```r
generate_taxa_indiv_boxplot_long(
   data.obj = peerj32.obj,
   subject.var = "subject",
   time.var = "time",
   t0.level = "1",
   ts.levels = "2",
   group.var = "group",
   strata.var = NULL,
   feature.level = c("Family"),
   features.plot = NULL,
   feature.dat.type = "other",
   top.k.plot = NULL,
   top.k.func = NULL,
   transform = "log",
   prev.filter = 0.1,
   abund.filter = 0.1,
   base.size = 20,
   theme.choice = "classic",
   custom.theme = NULL,
   palette = NULL,
   pdf = TRUE,
   file.ann = NULL,
   pdf.wid = 11,
   pdf.hei = 8.5
)
```

<figure><img src="../.gitbook/assets/Screenshot 2023-06-13 at 10.29.38.png" alt=""><figcaption><p>Longitudinal Boxplot of Individual Taxa: The <code>generate_taxa_indiv_boxplot_long()</code> function enables an in-depth investigation of the variation in abundance for each taxon within paired samples across time. Each page of the multi-page PDF represents a single taxon, providing a focused perspective on the dynamic changes under paired conditions.</p></figcaption></figure>

This function creates a series of boxplots, one for each taxon, and outputs them into a multi-page PDF. Each page provides a focused look at a single taxon, helping to analyze complex data in manageable pieces.

Next, we have `generate_taxa_boxplot_long()`:

```r
generate_taxa_boxplot_long(
   data.obj = peerj32.obj,
   subject.var = "subject",
   time.var = "time",
   t0.level = "1",
   ts.levels = "2",
   group.var = "group",
   strata.var = NULL,
   feature.level = c("Family"),
   feature.dat.type = "count",
   features.plot = NULL,
   top.k.plot = NULL,
   top.k.func = NULL,
   transform = "log",
   prev.filter = 0.1,
   abund.filter = 0.1,
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

<figure><img src="../.gitbook/assets/Screenshot 2023-06-13 at 10.32.08.png" alt=""><figcaption><p>Comprehensive Boxplot of Individual Taxa: The <code>generate_taxa_boxplot_long()</code> function generates a combined boxplot of all taxa, allowing an immediate comparison of abundance variations across paired samples over time. This visualization is particularly useful in highlighting taxa with significant differential abundance, offering a clear overview of the microbiome dynamics within paired samples.</p></figcaption></figure>

This function places all taxa onto a single page, providing an overview of all your taxa at once. This can be especially useful for visualizing features with significant differential abundance (<0.05).

We introduce two essential tools for our paired taxa analysis: `generate_taxa_indiv_change_boxplot_pair()` and `generate_taxa_change_boxplot_pair()`. These functions offer an in-depth look into changes in the taxonomic composition at the family level.

The first function, `generate_taxa_indiv_change_boxplot_pair()`, is applied as follows:

```R
generate_taxa_indiv_change_boxplot_pair(
   data.obj = peerj32.obj,
   subject.var = "subject",
   time.var = "time",
   group.var = "group",
   strata.var = NULL,
   change.base = "1",
   feature.change.func = "log fold change",
   feature.level = c("Family"),
   features.plot = NULL,
   feature.dat.type = "count",
   top.k.plot = NULL,
   top.k.func = NULL,
   prev.filter = 0.1,
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

<figure><img src="../.gitbook/assets/Screenshot 2023-06-13 at 10.54.42.png" alt=""><figcaption><p>Individual Taxa Change Boxplot Pair: Created by the <code>generate_taxa_indiv_change_boxplot_pair()</code> function, this plot visualizes changes in taxonomic composition at the family level. The log-fold-change metric provides a clear depiction of the shifts in abundance over time, offering a more detailed understanding of the dynamics between different conditions.</p></figcaption></figure>

Next, we move onto the second function `generate_taxa_change_boxplot_pair()`:

```R
generate_taxa_change_boxplot_pair(
   data.obj = peerj32.obj,
   subject.var = "subject",
   time.var = "time",
   group.var = "group",
   strata.var = NULL,
   change.base = "1",
   feature.change.func = "log fold change",
   feature.level = c("Family"),
   feature.dat.type = "count",
   features.plot = NULL,
   top.k.plot = 6,
   top.k.func = "sd",
   prev.filter = 0.1,
   abund.filter = 0.01,
   base.size = 20,
   theme.choice = "bw",
   custom.theme = NULL,
   palette = NULL,
   pdf = TRUE,
   file.ann = "test",
   pdf.wid = 11,
   pdf.hei = 8.5
)
```

<figure><img src="../.gitbook/assets/Screenshot 2023-06-13 at 10.56.58.png" alt=""><figcaption><p>Individual Taxa Change Boxplot Pair with Standard Deviation: Generated by the <code>generate_taxa_change_boxplot_pair()</code> function, this plot also illustrates changes in abundance within paired samples. However, the focus is on the top six taxa based on their standard deviation, providing a unique perspective on the microbial dynamics.</p></figcaption></figure>

We introduce `generate_taxa_barplot_pair()`, a function that generates two insightful stacked bar plots, providing a clear picture of taxonomic composition from both individual and group perspectives.

```r
generate_taxa_barplot_pair(
  data.obj = peerj32.obj,
  subject.var = "subject",
  time.var = "time",
  group.var = "group",
  strata.var = NULL,
  feature.level = "Family",
  feature.dat.type = c("count"),
  feature.number = 8,
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

<figure><img src="../.gitbook/assets/Screenshot 2023-06-12 at 18.41.04.png" alt=""><figcaption><p>Individual Stacked Bar Plot of Taxa Composition: Created by the <code>generate_taxa_barplot_pair()</code> function, this visualization displays the changes in taxonomic composition within each subject over time. Connecting lines across the stacked bars emphasize the individual longitudinal journey of the microbiome.</p></figcaption></figure>

<figure><img src="../.gitbook/assets/Screenshot 2023-06-12 at 18.41.49.png" alt=""><figcaption><p>Average Group Stacked Bar Plot of Taxa Composition: Also produced by the <code>generate_taxa_barplot_pair()</code> function, this plot illustrates the average changes in taxonomic composition within groups over time. This representation sheds light on the broader microbial dynamics across different subject groups.</p></figcaption></figure>

{% hint style="info" %}
**Hint**: The `feature.number` parameter is a common feature in all **taxa\_barplot** functions within MicrobiomeStat. It sets a limit on the **number of taxa** displayed in the bar plot. If you set it to a specific number, let's say 9, then only the **9 most abundant taxa** are distinctly represented. All other taxa are collectively grouped into an '**Other**' category. This feature ensures clarity by focusing on the **most abundant taxa**, while still accounting for the collective presence of other, less abundant taxa.
{% endhint %}

{% hint style="info" %}
**Hint** : The `feature.level` parameter is highly flexible and plays a key role in refining your taxa analysis. It can be set to any column name in `feature.ann`. Moreover, if you want to perform analysis directly on **OTUs, ASVs, KEGG, Genes**, etc., you can simply set `feature.level` to '**original**'. This flexibility allows MicrobiomeStat to cater to your specific analytical needs, offering personalized insights into your microbiome data.
{% endhint %}

With its vibrant visuals, this function illuminates the shift in **taxa composition** for each subject across different time points, while simultaneously showcasing the average taxa composition change within groups. These **graphical representations provide a holistic understanding of the dynamic microbial landscape**, capturing the unique microbiome journey of each subject and revealing overarching patterns across groups. A true feast for the eyes and the mind!

Delving further into the microbiome landscape, we turn our attention to the `generate_taxa_dotplot_pair()` function. This powerful tool offers a visual way to assess the disparities in **mean abundance** and **mean prevalence** across different groups.

When exploring paired samples, `generate_taxa_dotplot_pair()` displays dotplots of each feature's (like taxa at the Family level) mean abundance and mean prevalence. It provides a rich visual overview that aids in deciphering differences between microbial communities.

```r
generate_taxa_dotplot_pair(
  data.obj = peerj32.obj,
  subject.var = "subject",
  time.var = "time",
  group.var = "group",
  strata.var = NULL,
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

In this journey, you have the ability to filter out low-abundance and low-prevalence features by adjusting the `prev.filter` and `abund.filter`. This enables a more focused and meaningful exploration of your microbiome data.

Moving on to `generate_taxa_change_dotplot_pair()`, we find a function perfectly tailored to **visualize microbiome fluctuations** within your dataset. By leveraging this function, you can track the **dynamic changes** in taxa abundance across different time points.

Let's see an example of how to use this function:

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

With `feature.change.func` set to "log fold change" (log 2 fold change), you can **visually articulate** these microbiome shifts, unveiling the mysterious dance of your microbial communities over time.

However, the true power of this function isn't in tracking individual taxa changes, but in its capacity to **contrast these changes between different groups**. This capability allows you to dive deeper, uncovering the significant differences that propel your research forward towards more comprehensive conclusions.

Indeed, `generate_taxa_change_dotplot_pair()` serves as a potent tool in your MicrobiomeStat toolbox, focusing your analysis by filtering out the noise and shining a light on the most impactful shifts within your microbiome data.

Next on our **MicrobiomeStat** journey is the `generate_taxa_heatmap_pair()` function. This powerful tool allows us to craft an **illuminating heatmap** for an in-depth visualization of taxonomic shifts over time. Here's how we call it:

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

This function crafts a heatmap that not only lets us observe individual samples' taxa shifts at the **Family** level, but also helps us identify **taxa that share similar abundance patterns** across different conditions. Such insights can be instrumental in uncovering potential biological interactions and ecological behaviors within your microbiome data!

Diving deep into **patterns of change** is crucial for insightful microbiome analysis, and the **`generate_taxa_change_heatmap_pair()`** function from MicrobiomeStat is perfectly suited to assist in this quest.

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

Generates a **meticulously detailed heatmap** that spotlights patterns of change in taxa abundance at the Family level. You are equipped to identify similar patterns among both **taxa and samples** based on relative changes in abundance.

In the function, 'relative change' is calculated using the formula `(change.after - change.before) / (change.before + change.after)`. This means the **intensity of color** in the heatmap is directly proportional to the **magnitude of change**. This **intuitive and visually engaging** method aids in understanding shifts in the microbial community over time.

By using this, you will journey into the depths of your data and **uncover the rich and intricate details** that shape your microbiome.
