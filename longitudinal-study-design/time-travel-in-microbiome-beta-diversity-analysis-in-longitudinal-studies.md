# Beta Diversity Analysis

This guide outlines how to use MicrobiomeStat for beta diversity analysis, specifically focusing on visualizing and quantifying temporal changes in microbial community composition. MicrobiomeStat offers tools for examining longitudinal patterns in beta diversity.

For those beta diversity-based functions, they all have a `dist.obj` parameter, which accepts a list of distance matrices. If the parameter is not specified, `mStat_calculate_beta_diversity` will be automatically called. Or the users can create their own list of distance matrices and pass it to `dist.obj`. We recommend the user to create a list of distance matrices once in the analysis and then use it repeatedly.

When covariates (`adj.vars`) are identified, the `mStat_calculate_adjusted_distance` function refines the microbial community dissimilarities using a method based on the reference below.

> Chen J, Zhang X. dICC: distance-based intraclass correlation coefficient for metagenomic reproducibility studies. Bioinformatics. 2022 Oct 31;38(21):4969-4971. doi: 10.1093/bioinformatics/btac618. PMID: 36083005; PMCID: PMC9801959.

This process ensures the extracted microbial patterns are not confounded by the specified covariates, thus presenting a more accurate representation of the microbial community structures.

Based on the distance matrices, we can also extract their first several principal coordinates (PCs), which capture the main variation in the data. PCs can be calculated by calling `mstat_calculate_PC` function. While users are free to select between "mds" and "nmds" as their ordination method, in scenarios where `pc.obj` isn't provided beforehand, the toolkit defaults to the "mds" method. Researchers favoring advanced ordination techniques like t-SNE or UMAP can employ external tools to compute results. For those PC-based functions, they all have a `pc.obj` parameter, which accepts a list of PC matrices based on a variety of beta diversity measures. If the parameter is not specified, `mStat_calculate_PC` will be called automatically. Or the users can create their own list of PC matrices and pass it to `pc.obj`. We recommend th user to create the list once in the analysis and use it repeatedly.

MicrobiomeStats supports a variety of beta diversity measures, including "BC" (Bray-Curtis), "Jaccard", "UniFrac" (unweighted), "GUniFrac" (generalized), "WUniFrac" (weighted), and "JS" (Jensen-Shannon divergence). These measures can be computed using the `mStat_calculate_beta_diversity` function, which is designed to handle a range of distance calculations for assessing the dissimilarity between microbial communities.

Building on these distance measures, the `generate_beta_trend_test_long()` function in MicrobiomeStat utilizes a linear mixed effects model for longitudinal beta diversity trend analysis. This function uses the distance matrix as the response, time as a fixed effect, and subject as a random effect. It also supports interaction with a grouping variable and inclusion of covariates for model refinement.

```r
data(subset_T2D.obj)
generate_beta_trend_test_long(
  data.obj = subset_T2D.obj,
  dist.obj = NULL,
  subject.var = "subject_id",
  time.var = "visit_number_num",
  group.var = "subject_race",
  adj.vars = c("subject_gender", "sample_body_site"),
  dist.name = c("BC", "Jaccard")
)
```

<table><thead><tr><th>Term</th><th width="133">Estimate</th><th>Std.Error</th><th>Statistic</th><th>P.Value</th></tr></thead><tbody><tr><td>(Intercept)</td><td>1.95</td><td>0.142</td><td>13.8</td><td>1.59e-20</td></tr><tr><td>subject_racecaucasian</td><td>-0.280</td><td>0.162</td><td>-1.73</td><td>8.90e-2</td></tr><tr><td>subject_racehispanic_or_latino</td><td>0.248</td><td>0.308</td><td>0.805</td><td>4.25e-1</td></tr><tr><td>visit_number_num</td><td>-0.0235</td><td>0.0245</td><td>-0.959</td><td>3.44e-1</td></tr><tr><td>subject_racecaucasian:visit_number_num</td><td>0.0490</td><td>0.0280</td><td>1.75</td><td>8.88e-2</td></tr><tr><td>subject_racehispanic_or_latino:visit_number_num</td><td>0.0264</td><td>0.0484</td><td>0.545</td><td>5.90e-1</td></tr><tr><td>subject_race:visit_number_num</td><td>NA</td><td>NA</td><td>1.56</td><td>2.27e-1</td></tr></tbody></table>

The `generate_beta_volatility_test_long()` function performs a volatility test for beta diversity in longitudinal data. This function calculates the beta diversity volatility by determining the average rate of change in beta diversity over time for each subject.

In mathematical terms, the volatility (V) can be calculated as follows:

First, let's define a few terms:

* "d\_i" represents the beta diversity (distance) between time "i" and "i+1".
* "delta\_t\_i" is the time difference between time "i" and "i+1".

With these terms, the volatility for a subject is calculated as:

$$
V = \frac{1}{N-1} \sum \left| \frac{d_i}{\Delta t_i} \right|
$$

Here:

* "N" is the total number of time points for the subject.
* The summation "sum" is over all time points for which "d\_i" is defined.

This formula calculates the average rate of change in distance per unit time, which is an appropriate measure of volatility in this context.

To account for the adjustment variables in the distance calculation, the function employs Multidimensional Scaling (MDS). This technique transforms the distance matrix into a set of coordinates. These coordinates are then adjusted to account for the influence of the covariates, and the adjusted coordinates are used to compute the adjusted distances.

After the volatility is calculated, the function tests the relationship between the calculated volatility and a specified group variable. This is done using linear regression or ANOVA for multi-category variables, allowing for the exploration of how beta diversity volatility differs across different groups.

```r
data(subset_T2D.obj)
generate_beta_volatility_test_long(
  data.obj = subset_T2D.obj,
  dist.obj = NULL,
  subject.var = "subject_id",
  time.var = "visit_number_num",
  group.var = "subject_race",
  adj.vars = "sample_body_site",
  dist.name = c("BC", "Jaccard")
)
```

<table><thead><tr><th>Term</th><th width="140">Estimate</th><th>Std.Error</th><th>Statistic</th><th>P.Value</th></tr></thead><tbody><tr><td>(Intercept)</td><td>0.956</td><td>0.161</td><td>5.93</td><td>0.000000164</td></tr><tr><td>subject_racecaucasian</td><td>-0.104</td><td>0.182</td><td>-0.571</td><td>0.570</td></tr><tr><td>subject_racehispanic_or_latino</td><td>-0.502</td><td>0.372</td><td>-1.35</td><td>0.183</td></tr><tr><td>subject_race</td><td>NA</td><td>NA</td><td>0.909</td><td>0.408</td></tr><tr><td>Residuals</td><td>NA</td><td>NA</td><td>NA</td><td>NA</td></tr></tbody></table>

In addition to these statistical analyses, MicrobiomeStat also provides visualization tools for beta diversity. For instance, the `generate_beta_pc_boxplot_long()` function creates a plot that shows individual trajectories across ordination axes, connecting data points from one timepoint to the next.

```{r
data(ecam.obj)
generate_beta_pc_boxplot_long(
  data.obj = ecam.obj,
  dist.obj = NULL,
  pc.obj = NULL,
  subject.var = "studyid",
  time.var = "month",
  t0.level = "0",
  ts.levels = as.character(sort(as.numeric(unique(ecam.obj$meta.dat$month))))[2:4],
  group.var = "diet", 
  strata.var = "delivery",
  dist.name = c('BC'),
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

<figure><img src="../.gitbook/assets/Screenshot 2023-10-12 at 10.49.33.png" alt=""><figcaption></figcaption></figure>

Expanding on this, the `generate_beta_ordination_long()` function creates a plot where samples cluster based on similarity, providing a global overview of the data.

```{r
generate_beta_ordination_long(
  data.obj = subset_T2D.obj,
  dist.obj = NULL,  
  pc.obj = NULL,
  subject.var = "subject_id",
  time.var = "visit_number",
  t0.level = sort(unique(subset_T2D.obj$meta.dat$visit_number))[1],
  ts.levels = sort(unique(subset_T2D.obj$meta.dat$visit_number))[2:4],
  group.var = "subject_race",
  strata.var = "subject_gender",
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

<figure><img src="../.gitbook/assets/Screenshot 2023-10-12 at 10.51.29.png" alt=""><figcaption></figcaption></figure>

Lastly, the `generate_beta_change_spaghettiplot_long()` function creates a plot that shows individual trajectories of microbial change, providing a detailed view of each subject's journey through time.

```{r
generate_beta_change_spaghettiplot_long(
  data.obj = ecam.obj,
  dist.obj = NULL,
  subject.var = "studyid",
  time.var = "month", 
  t0.level = NULL,
  ts.levels = NULL,
  group.var = "diet",
  strata.var = "antiexposedall",
  dist.name = c("BC"),
  base.size = 20,
  theme.choice = "bw",
  palette = NULL,
  pdf = TRUE,
  file.ann = NULL,
  pdf.wid = 11, 
  pdf.hei = 8.5
)
```

<figure><img src="../.gitbook/assets/Screenshot 2023-10-12 at 10.52.24.png" alt=""><figcaption></figcaption></figure>
