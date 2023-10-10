---
description: >-
  Explore alpha diversity analysis in cross-sectional studies using
  MicrobiomeStat. Learn to interpret these biodiversity measures for valuable
  microbiome dataset insights.
---

# Cross-Sectional Snapshot: Alpha Diversity Analysis with MicrobiomeStat

Alpha diversity indices provide insights into the species richness and evenness within individual microbiome samples. Utilizing **MicrobiomeStat**, we can adeptly analyze these metrics in the `peerj32.obj` dataset. For a comprehensive view, we'll start by analyzing the dataset without distinguishing time-based and strata variables.

Prior to diving deep, it's noteworthy to explain the role of `alpha.obj`. It represents pre-computed alpha diversity indices, derived from the `mStat_calculate_alpha_diversity` function. If this isn't calculated beforehand and supplied to alpha-related functions, those functions will, by default, invoke `mStat_calculate_alpha_diversity` internally.

By default, the alpha functions in MicrobiomeStat perform data rarefaction, ensuring the datasets become more comparable by leveling the sequencing depth across samples. Rarefaction standardizes the sampling depth, rendering it instrumental in comparative analyses. If your research objective is to retain the original, non-rarefied data for alpha diversity calculations, it's crucial to pre-compute `alpha.obj`. This approach bypasses the internal rarefaction step, preserving the integrity of the original data.

Determining significant disparities in alpha diversity across groups mandates the application of robust statistical testing. For this, the `generate_alpha_test_single` function is invaluable. It conducts association tests juxtaposing the indices with a designated grouping variable, all within the confines of linear models.

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

### Shannon Index Results

| Term         | Estimate | Std.Error | Statistic | P.Value  |
| ------------ | -------- | --------- | --------- | -------- |
| (Intercept)  | 3.60     | 0.0386    | 93.2      | 9.47e-27 |
| sexmale      | -0.0633  | 0.0451    | -1.40     | 1.77e-1  |
| groupPlacebo | 0.0415   | 0.0437    | 0.950     | 3.54e-1  |

### Observed Species Results

| Term         | Estimate | Std.Error | Statistic | P.Value  |
| ------------ | -------- | --------- | --------- | -------- |
| (Intercept)  | 100.     | 3.72      | 26.9      | 1.36e-16 |
| sexmale      | 2.29     | 4.35      | 0.528     | 6.04e-1  |
| groupPlacebo | 1.99     | 4.21      | 0.473     | 6.42e-1  |

The visualization of these findings is made possible through the `generate_alpha_boxplot_single` function. Initially, to gain an overarching perspective, we'll sidestep the time-based and strata variables.

```r
generate_alpha_boxplot_single(
  data.obj = peerj32.obj,
  alpha.obj = NULL,
  alpha.name = c("shannon"),
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
  alpha.name = c("shannon"),
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