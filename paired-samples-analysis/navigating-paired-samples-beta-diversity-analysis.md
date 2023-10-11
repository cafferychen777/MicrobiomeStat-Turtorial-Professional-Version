---
description: >-
  A deep-dive into beta diversity metrics and ordination methods for paired
  samples, elucidating differences in microbial community structure.
---

# Navigating Paired Samples: Beta Diversity Analysis

Welcome to this tutorial on Beta Diversity Analysis within Paired Samples. Our focus here is to understand the variations in microbial community structure across different groups and within individual subjects over time. We aim to identify patterns and extract meaningful insights from microbiome data using various functions provided by MicrobiomeStat.

Before starting with pre-computed data objects, it's beneficial to grasp the utility of `dist.obj` and `pc.obj` in the context of this toolkit. By design, if these aren't provided, `MicrobiomeStat` auto-generates them. The `dist.obj` is computed using the `mStat_calculate_beta_diversity` function. When covariates (`adj.vars`) are identified, the `mStat_calculate_adjusted_distance` function refines the microbial community dissimilarities using a method based on linear models and multidimensional scaling. This process ensures the extracted microbial patterns are not confounded by the specified covariates, thus presenting a more accurate representation of the microbial community structures.

`pc.obj`, in its essence, is derived from the `mStat_calculate_PC` function. While users are free to select between "mds" and "nmds" as their ordination method, in scenarios where `pc.obj` isn't provided beforehand, the toolkit defaults to the "mds" method. Researchers with a penchant for advanced ordination techniques like t-SNE or UMAP can employ external tools to compute results. Subsequently, these results can be reformatted to align with the `pc.obj` structure, facilitating effortless integration with `MicrobiomeStat`.

Further, it's worth noting that the range of distance measures (`dist.name`) supported includes "BC" (Bray-Curtis), "Jaccard", "UniFrac" (unweighted UniFrac), "GUniFrac" (generalized UniFrac), "WUniFrac" (weighted UniFrac), and "JS" (Jensen-Shannon divergence). It's pivotal to understand that some of these methods, especially those like "UniFrac", necessitate the presence of a phylogenetic tree. Thus, prior to leveraging these metrics, ensure that the `tree` component exists within the `data.obj`.

We begin our analysis by statistically testing beta diversity differences using the `generate_beta_change_test_pair()` function. This function fits linear models where the dependent variable is the distance between two points from the same subject in a paired design, relating these beta diversity measures to grouping and covariate variables.

```r
generate_beta_change_test_pair(
  data.obj = peerj32.obj,
  dist.obj = NULL,
  time.var = "time",
  subject.var = "subject",
  group.var = "group",
  adj.vars = c("sex"),
  change.base = "1",
  dist.name = c('BC', 'Jaccard') 
)
```

The function returns coefficient tables with p-values for assessing variable associations with each beta diversity metric. These tables enable rigorous hypothesis testing before visualizing beta diversity changes in paired designs.

| Distance | Term         | Estimate | Std.Error | Statistic | P.Value  |
| -------- | ------------ | -------- | --------- | --------- | -------- |
| BC       | (Intercept)  | 0.175    | 0.0186    | 9.42      | 8.24e-12 |
| BC       | sexmale      | 0.0671   | 0.0217    | 3.09      | 3.58e-3  |
| BC       | groupPlacebo | -0.0157  | 0.0210    | -0.746    | 4.60e-1  |

| Distance | Term         | Estimate | Std.Error | Statistic | P.Value  |
| -------- | ------------ | -------- | --------- | --------- | -------- |
| Jaccard  | (Intercept)  | 0.294    | 0.0246    | 12.0      | 6.04e-15 |
| Jaccard  | sexmale      | 0.0853   | 0.0288    | 2.96      | 5.06e-3  |
| Jaccard  | groupPlacebo | -0.0207  | 0.0279    | -0.744    | 4.61e-1  |

Next, we explore the `generate_beta_ordination_pair()` function. This function generates ordination plots based on the microbial community structures of samples.

```r
generate_beta_ordination_pair(
  data.obj = peerj32.obj,
  dist.obj = NULL,
  pc.obj = NULL,
  subject.var = "subject",
  time.var = "time",
  group.var = "group",
  strata.var = "sex",
  dist.name = c("BC"),
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

<figure><img src="../.gitbook/assets/Screenshot 2023-10-11 at 15.07.10.png" alt=""><figcaption><p>Resulting from <code>generate_beta_ordination_pair()</code>, this beta diversity ordination plot maps out the intricate relationships between samples based on their microbial community structures. Look at how elegantly the arrows connect the same subject across different time points, presenting a dynamic journey of their microbiome composition over time.</p></figcaption></figure>

The `generate_beta_change_boxplot_pair()` function is another tool that aids in our beta diversity analysis. This function quantifies the beta diversity changes within individual subjects over time, using metrics like Bray-Curtis dissimilarity.

```r
generate_beta_change_boxplot_pair(
  data.obj = peerj32.obj,
  dist.obj = NULL,
  subject.var = "subject",
  time.var = "time",
  group.var = "group",
  strata.var = "sex",
  change.base = "1",
  dist.name = c('BC'),
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

<figure><img src="../.gitbook/assets/Screenshot 2023-10-11 at 15.08.07.png" alt=""><figcaption><p>Courtesy of <code>generate_beta_change_boxplot_pair()</code>, this insightful plot unravels the intricate shifts in beta diversity within subjects over time. The neatly arranged boxplots symbolize different time points, while the horizontal lines represent the beta diversity changes of the same subject. Together, they illustrate a captivating narrative of microbiome dynamics.</p></figcaption></figure>

The `generate_beta_pc_change_boxplot_pair()` function provides a more granular perspective of our microbial data by elucidating the differences in specific ordination Axes between different groups.

```r
generate_beta_pc_change_boxplot_pair(
  data.obj = peerj32.obj,
  dist.obj = NULL,
  pc.obj = NULL,
  pc.ind = c(1, 2),
  subject.var = "subject",
  time.var = "time",
  group.var = "group",
  strata.var = "sex",
  change.base = "1",
  change.func = "absolute change",
  dist.name = c('BC'),
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

<figure><img src="../.gitbook/assets/Screenshot 2023-10-11 at 15.09.03.png" alt=""><figcaption><p>Produced by <code>generate_beta_pc_change_boxplot_pair()</code>, this boxplot unveils the differences in specific ordination Axes between different groups. It provides a granular look into the shifting sands of our microbial data, offering valuable insights into community shifts and their biological implications.</p></figcaption></figure>

Lastly, the `generate_beta_pc_boxplot_long()` function traces the trajectory of each subject's beta diversity across two time points on specific ordination axes.

```r
generate_beta_pc_boxplot_long(
  data.obj = peerj32.obj,
  dist.obj = NULL,
  pc.obj = NULL,
  subject.var = "subject",
  time.var = "time",
  t0.level = "1",
  ts.levels = "2",
  group.var = "group",
  strata.var = "sex",
  dist.name = c('BC'),
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

<figure><img src="../.gitbook/assets/Screenshot 2023-10-11 at 15.10.07.png" alt=""><figcaption><p>Emerging from <code>generate_beta_pc_boxplot_long()</code>, this compelling plot maps the dynamic journey of each subject's microbiome across two distinct time points. Each line elegantly depicts the evolution of beta diversity within an individual subject along specified ordination axes, providing a clear visualization of their microbial narrative.</p></figcaption></figure>

By using these functions, we can gain a comprehensive understanding of the shifts in microbial communities over time and across different groups.
