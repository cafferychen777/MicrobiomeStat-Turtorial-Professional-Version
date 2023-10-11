---
description: >-
  Explore alpha diversity analysis in cross-sectional studies using
  MicrobiomeStat. Learn to interpret these biodiversity measures for valuable
  microbiome dataset insights.
---

Alpha diversity indices offer insights into the species richness and evenness within individual microbiome samples. Utilizing MicrobiomeStat, we're equipped to delve deep into these metrics using the `peerj32.obj` dataset. For an encompassing perspective, our initial approach will be to scrutinize the dataset in its entirety, eschewing time-based and strata considerations.

It's worth noting that while our primary focus here lies on the Shannon index and Observed Species, MicrobiomeStat is versatile, supporting a variety of indices. These encompass "shannon", "simpson", "observed_species", "chao1", "ace", and "pielou". Each of these metrics furnishes distinct insights into the species richness and evenness inherent in the microbiome samples.

Before delving further, it's pivotal to comprehend the significance of `alpha.obj`. This parameter encapsulates pre-computed alpha diversity indices, a byproduct of the `mStat_calculate_alpha_diversity` function. If this element isn't pre-processed and integrated into the alpha-centric functions, those functions default to invoking `mStat_calculate_alpha_diversity` autonomously.

Additionally, the alpha functions in MicrobiomeStat, by default, undertake data rarefaction. This process ensures the datasets are rendered more comparable by harmonizing the sequencing depth across samples. Such standardization is vital for comparative analyses. However, if you're aiming to work with the original, non-rarefied data when determining alpha diversity, it's imperative to pre-calculate `alpha.obj`. By doing so, the intrinsic rarefaction process is sidestepped, thereby maintaining the data's originality.

Lastly, ascertaining pronounced variations in alpha diversity across distinct groups necessitates rigorous statistical scrutiny. The `generate_alpha_test_single` function serves this purpose, executing association tests that compare the indices with a specified grouping variable, all under the ambit of linear models.

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

<figure><img src="../.gitbook/assets/Screenshot 2023-10-10 at 19.16.07.png" alt=""><figcaption><p>Boxplot of the Shannon alpha diversity index across all samples, disregarding the time and strata variables. The boxplot provides an overview of the species complexity within the samples from the two groups: Probiotic LGG and Placebo.</p></figcaption></figure>

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

<figure><img src="../.gitbook/assets/Screenshot 2023-10-10 at 19.17.06.png" alt=""><figcaption><p>Boxplot of the Shannon alpha diversity index for samples at the specific time point '2'. The plot shows the species complexity for each group at this particular time point, offering insights into the potential impact of the probiotic intervention.</p></figcaption></figure>

For a detailed inspection, we can further stratify the analysis using the variable `sex`.

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
  strata.var = "sex",
  base.size = 16,
  theme.choice = "bw",
  palette = NULL,
  pdf = TRUE,
  file.ann = NULL,
  pdf.wid = 11,
  pdf.hei = 8.5)
```

<figure><img src="../.gitbook/assets/Screenshot 2023-10-10 at 19.18.43.png" alt=""><figcaption><p>Boxplot of the Shannon alpha diversity index for samples at time point '2', considering the strata variable 'sex'. The plot provides an added layer of understanding by showcasing the species complexity stratified by sex and group. This helps to identify any sex-specific effects of the intervention on the microbiome's species complexity.</p></figcaption></figure>
