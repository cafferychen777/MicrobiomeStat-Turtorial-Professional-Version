# Beta Diversity Analysis

Welcome to this tutorial on Beta Diversity Analysis within Paired Samples. Our focus here is to understand the variations in microbial community structure and composition across different groups and within individual subjects over time. We aim to identify patterns and extract meaningful insights from microbiome data using various functions provided by MicrobiomeStat.

For those beta diversity-based functions, they all have a `dist.obj` parameter, which accepts a list of distance matrices. If the parameter is not specified, `mStat_calculate_beta_diversity` will be automatically called. MicrobiomeStats supports  "BC" (Bray-Curtis), "Jaccard", "UniFrac" (unweighted), "GUniFrac" (generalized), "WUniFrac" (weighted), and "JS" (Jensen-Shannon divergence).   The user can create his own list of distance matrices and pass it to `dist.obj`. We recommend the user to create a list of distance matrices once in the analysis and then use it repeatedly.

In beta diversity-based visualization functions as well as those testing functions based on changes, when covariates (`adj.vars`) are presented, the covariate effects will be removed using `mStat_calculate_adjusted_distance` function. The method used is referenced below. However, we recommend the user to perform covariate adjustment explicitly and store the adjusted beta diversity object for future repeated use. 

> Shi, Y., Zhang, L., Do, K. A., Peterson, C. B., & Jenq, R. R. (2020). aPCoA: covariate adjusted principal coordinates analysis. Bioinformatics, 36(13), 4099-4101. PMID: 32339223 PMCID: PMC7332564.

This process ensures the extracted microbial patterns are not confounded by the specified covariates, thus presenting a cleaner representation of the microbial community structure.

Based on the distance matrices, we can also extract the first several principal coordinates (PCs), which capture the main variation in the data. PCs can be calculated by calling `mstat_calculate_PC` function. While users are free to select between "mds" and "nmds" as their ordination method, in scenarios where `pc.obj` is not provided beforehand, the toolkit defaults to the "mds" method. Researchers favoring advanced ordination techniques like t-SNE or UMAP can employ external tools to compute results. For those PC-based functions, they all have a `pc.obj` parameter, which accepts a list of PC matrices based on a variety of beta diversity measures. If the parameter is not specified, `mStat_calculate_PC` will be called automatically. Or the users can create their own list of PC matrices and pass it to `pc.obj`. We recommend the user to create the list once in the analysis and use it repeatedly. 

We begin our analysis by statistically testing beta diversity differences using the `generate_beta_change_test_pair()` function. This function fits linear models where the dependent variable is the distance between two time points from the same subject in a paired design, relating these beta diversity measures to  the grouping variable and covariates.

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

The function returns coefficient tables with p-values for assessing  associations with each beta diversity measure. These tables enable rigorous hypothesis testing complementing visualization for paired designs.

<table><thead><tr><th width="134">Distance</th><th>Term</th><th>Estimate</th><th>Std.Error</th><th>Statistic</th><th>P.Value</th></tr></thead><tbody><tr><td>BC</td><td>(Intercept)</td><td>0.175</td><td>0.0186</td><td>9.42</td><td>8.24e-12</td></tr><tr><td>BC</td><td>sexmale</td><td>0.0671</td><td>0.0217</td><td>3.09</td><td>3.58e-3</td></tr><tr><td>BC</td><td>groupPlacebo</td><td>-0.0157</td><td>0.0210</td><td>-0.746</td><td>4.60e-1</td></tr></tbody></table>

<table><thead><tr><th width="123">Distance</th><th>Term</th><th>Estimate</th><th>Std.Error</th><th>Statistic</th><th>P.Value</th></tr></thead><tbody><tr><td>Jaccard</td><td>(Intercept)</td><td>0.294</td><td>0.0246</td><td>12.0</td><td>6.04e-15</td></tr><tr><td>Jaccard</td><td>sexmale</td><td>0.0853</td><td>0.0288</td><td>2.96</td><td>5.06e-3</td></tr><tr><td>Jaccard</td><td>groupPlacebo</td><td>-0.0207</td><td>0.0279</td><td>-0.744</td><td>4.61e-1</td></tr></tbody></table>

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

<figure><img src="../.gitbook/assets/Screenshot 2023-10-11 at 15.07.10.png" alt=""><figcaption></figcaption></figure>

The `generate_beta_change_boxplot_pair()` function is another tool that aids in beta diversity-based visualization. This function quantifies the beta diversity changes within individual subjects over time.

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

<figure><img src="../.gitbook/assets/Screenshot 2023-10-11 at 15.08.07.png" alt=""><figcaption></figcaption></figure>

The `generate_beta_pc_change_boxplot_pair()` function provides the functionality to explore the differences in specific ordination Axes between different groups. The `pc.ind` parameter is used to specify which Principal Coordinates (PCs) or axes to analyze after conducting a Multidimensional Scaling (MDS) or Nonmetric Multidimensional Scaling (NMDS).

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

<figure><img src="../.gitbook/assets/Screenshot 2023-10-11 at 15.09.03.png" alt=""><figcaption></figcaption></figure>

Lastly, the `generate_beta_pc_boxplot_long()` function traces the trajectory of each subject's beta diversity across two time points on specific ordination axes. However, when the number of subjects is large, the individual connection lines can become too cluttered, making the plot difficult to interpret. To address this, we have set a threshold: when the number of subjects (`n_subjects`) exceeds 25, the individual connection lines are replaced with overall connection lines, improving the clarity and readability of the plot.

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

<figure><img src="../.gitbook/assets/Screenshot 2023-10-11 at 15.10.07.png" alt=""><figcaption></figcaption></figure>
