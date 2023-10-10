---
description: >-
  Explore alpha diversity analysis in cross-sectional studies using
  MicrobiomeStat. Learn to interpret these biodiversity measures for valuable
  microbiome dataset insights.
---

# Cross-Sectional Snapshot: Alpha Diversity Analysis with MicrobiomeStat

Alpha diversity indices provide insights into the species richness and evenness within individual microbiome samples. Utilizing **MicrobiomeStat**, we can adeptly analyze these metrics in the `peerj32` dataset. For a comprehensive view, we'll start by analyzing the dataset without distinguishing time-based and strata variables.

To ascertain significant differences in alpha diversity between groups, it's essential to employ appropriate statistical tests. The `generate_alpha_test_single` function facilitates this by executing association tests between the indices and the selected grouping variable, framed within linear models.

```r
generate_alpha_test_single(
  data.obj = peerj32.obj,
  alpha.obj = NULL, 
  alpha.name = c("shannon", "observed_species"),
  depth = NULL,
  time.var = "time",
  t.level = "2",
  group.var = "group",
  adj.vars = "sex")
```

If one wishes to focus on a specific time point, values for `time.var` and `t.level` should be specified. In the absence of `alpha.obj`, the function derives alpha diversity indices from the OTU table.

For each specified index in `alpha.name`, a linear model is applied, designating `group.var` as the primary predictor. Covariates are integrated through `adj.vars`. The function's output consists of model results that enumerate coefficients, standard errors, test statistics, and p-values.

In scenarios where `group.var` encapsulates multiple categories, ANOVA gets employed to statistically validate alpha diversity disparities between the groups, considering potential confounding elements.

### Shannon Index and Observed Species Results

| Term         | Estimate | Std.Error | Statistic | P.Value  |
| ------------ | -------- | --------- | --------- | -------- |
| (Intercept)  | 3.59     | 0.0390    | 92.2      | 1.17e-26 |
| sexmale      | -0.0598  | 0.0455    | -1.31     | 2.04e-1  |
| groupPlacebo | 0.0455   | 0.0441    | 1.03      | 3.15e-1  |

| Term         | Estimate | Std.Error | Statistic | P.Value  |
| ------------ | -------- | --------- | --------- | -------- |
| (Intercept)  | 0.952    | 0.00311   | 306.      | 1.49e-36 |
| sexmale      | -0.00223 | 0.00364   | -0.614    | 5.47e-1  |
| groupPlacebo | 0.00428  | 0.00352   | 1.21      | 2.39e-1  |

The visualization of these findings is made possible through the `generate_alpha_boxplot_single` function. Initially, to gain an overarching perspective, we'll sidestep the time-based and strata variables.

```r
# Generate alpha diversity boxplot (simpson index) for all data
generate_alpha_boxplot_single(
  data.obj = peerj32.obj,
  alpha.obj = NULL,
  alpha.name = c("simpson"),
  depth = NULL,
  subject.var = "subject",
  time.var = NULL,
  t.level = NULL,
  group.var = "group",
  strata.var = NULL,
  base.size = 16,
  theme.choice = "bw",
  palette = NULL,
  pdf = TRUE,
  file.ann = NULL,
  pdf.wid = 11,
  pdf.hei = 8.5)
```

<figure><img src="../.gitbook/assets/Screenshot 2023-06-11 at 19.45.33.png" alt=""><figcaption><p>Boxplot of the Simpson alpha diversity index across all samples, disregarding the time and strata variables. The boxplot provides an overview of the species complexity within the samples from the two groups: Probiotic LGG and Placebo.</p></figcaption></figure>

Zooming into the particular time point, `t.level = "2`, provides a more nuanced understanding.

```r
generate_alpha_boxplot_single(
  data.obj = peerj32.obj,
  alpha.obj = NULL,
  alpha.name = c("simpson"),
  depth = NULL,
  subject.var = "subject",
  time.var = "time",
  t.level = "2",
  group.var = "group",
  strata.var = NULL,
  base.size = 16,
  theme.choice = "bw",
  palette = NULL,
  pdf = TRUE,
  file.ann = NULL,
  pdf.wid = 11,
  pdf.hei = 8.5)
```

<figure><img src="../.gitbook/assets/Screenshot 2023-06-11 at 19.46.41.png" alt=""><figcaption><p>Boxplot of the Simpson alpha diversity index for samples at the specific time point '2'. The plot shows the species complexity for each group at this particular time point, offering insights into the potential impact of the probiotic intervention.</p></figcaption></figure>

For a detailed inspection, we can further stratify the analysis using the variable `sex`.

```r
generate_alpha_boxplot_single(
  data.obj = peerj32.obj,
  alpha.obj = NULL,
  alpha.name = c("simpson"),
  depth = NULL,
  subject.var = "subject",
  time.var = "time",
  t.level = "2",
  group.var = "group",
  strata.var = "sex",
  base.size = 16,
  theme.choice = "bw",
  palette = NULL,
  pdf = TRUE,
  file.ann = NULL,
  pdf.wid = 11,
  pdf.hei = 8.5)
```

<figure><img src="../.gitbook/assets/Screenshot 2023-06-11 at 19.47.28.png" alt=""><figcaption><p>Boxplot of the Simpson alpha diversity index for samples at time point '2', considering the strata variable 'sex'. The plot provides an added layer of understanding by showcasing the species complexity stratified by sex and group. This helps to identify any sex-specific effects of the intervention on the microbiome's species complexity.</p></figcaption></figure>

Although this example majorly employs the Shannon index and Observed Species for alpha diversity, **MicrobiomeStat** supports multiple indices, including **"shannon"**, **"simpson"**, **"observed_species"**, **"chao1"**, **"ace"**, and **"pielou"**. Each offers unique insights into the species richness and evenness in the microbiome samples.