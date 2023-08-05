---
description: >-
  Explore alpha diversity analysis in cross-sectional studies using
  MicrobiomeStat. Learn to interpret these biodiversity measures for valuable
  microbiome dataset insights.
---

# Cross-Sectional Snapshot: Alpha Diversity Analysis with MicrobiomeStat

With **MicrobiomeStat**, we have the capability to delve into the richness and evenness of our microbiome dataset via **alpha diversity indices**. These indices paint a picture of the species complexity within individual samples. For our `peerj32` dataset, we'll start with a broad view, ignoring time and strata variables by setting both `time.var` and `t.level` as `NULL`.

While visualization provides an overview, statistical tests are required to quantify group differences in alpha diversity. The `generate_alpha_test_single` function carries out **association tests between alpha diversity indices and a grouping variable using linear models**.

```r
generate_alpha_test_single(
  data.obj = peerj32.obj,
  alpha.obj = NULL, 
  time.var = "time",
  t.level = "2",
  alpha.name = c("shannon", "simpson"),
  group.var = "group",
  adj.vars = "sex")
```

This function subsets the data to a given time point if `time.var` and `t.level` are specified. It calculates the alpha diversity indices from the OTU table if `alpha.obj` is not provided.

For each index in `alpha.name`, it fits a linear model with `group.var` as the predictor and `adj.vars` as adjustors. The model results are returned as a list of dataframes containing coefficients, standard errors, test statistics and p-values.

ANOVA is conducted if `group.var` has multiple categories. This provides a statistical test for alpha diversity differences between groups, adjusting for potential confounders.

#### Shannon Index

| term                              | Estimate | Std.Error | Statistic | P.Value  |
| --------------------------------- | -------- | --------- | --------- | -------- |
| (Intercept)                       | 3.12     | 0.362     | 8.64      | 5.40e-14 |
| subject\_gendermale               | 0.233    | 0.201     | 1.16      | 2.47e-1  |
| subject\_genderunknown            | 0.135    | 1.03      | 0.131     | 8.96e-1  |
| subject\_raceasian                | -0.753   | 0.431     | -1.75     | 8.32e-2  |
| subject\_racecaucasian            | -0.575   | 0.381     | -1.51     | 1.34e-1  |
| subject\_raceethnic\_other        | -0.941   | 0.815     | -1.15     | 2.51e-1  |
| subject\_racehispanic\_or\_latino | -0.437   | 0.527     | -0.830    | 4.08e-1  |
| subject\_gender                   | NA       | NA        | 0.480     | 6.20e-1  |
| subject\_race                     | NA       | NA        | 0.846     | 4.99e-1  |
| Residuals                         | NA       | NA        | NA        | NA       |

#### Simpson Index

| term                              | Estimate | Std.Error | Statistic | P.Value  |
| --------------------------------- | -------- | --------- | --------- | -------- |
| (Intercept)                       | 0.835    | 0.0581    | 14.4      | 7.03e-27 |
| subject\_gendermale               | 0.0507   | 0.0322    | 1.57      | 1.19e-1  |
| subject\_genderunknown            | -0.0271  | 0.166     | -0.163    | 8.70e-1  |
| subject\_raceasian                | -0.0889  | 0.0692    | -1.28     | 2.02e-1  |
| subject\_racecaucasian            | -0.0778  | 0.0612    | -1.27     | 2.07e-1  |
| subject\_raceethnic\_other        | -0.146   | 0.131     | -1.11     | 2.68e-1  |
| subject\_racehispanic\_or\_latino | 0.00504  | 0.0847    | 0.0596    | 9.53e-1  |
| subject\_gender                   | NA       | NA        | 1.17      | 3.13e-1  |
| subject\_race                     | NA       | NA        | 0.868     | 4.86e-1  |
| Residuals                         | NA       | NA        | NA        | NA       |

Next, we visualize the alpha diversity using `generate_alpha_boxplot_single`:

```r
# Generate alpha diversity boxplot (simpson index) for all data
generate_alpha_boxplot_single(
  data.obj = peerj32.obj,
  alpha.obj = NULL,
  alpha.name = c("simpson"),
  subject.var = "subject",
  time.var = NULL,
  t.level = NULL,
  group.var = "group",
  strata.var = NULL,
  base.size = 16,
  theme.choice = "classic",
  palette = NULL,
  pdf = TRUE,
  file.ann = NULL,
  pdf.wid = 11,
  pdf.hei = 8.5)
```

<figure><img src="../.gitbook/assets/Screenshot 2023-06-11 at 19.45.33.png" alt=""><figcaption><p>Boxplot of the Simpson alpha diversity index across all samples, disregarding the time and strata variables. The boxplot provides an overview of the species complexity within the samples from the two groups: Probiotic LGG and Placebo.</p></figcaption></figure>

Next, we focus our lens on a specific time point, `t.level = "2"`, to assess if there are significant alpha diversity differences:

```r
generate_alpha_boxplot_single(
  data.obj = peerj32.obj,
  alpha.obj = NULL,
  alpha.name = c("simpson"),
  subject.var = "subject",
  time.var = "time",
  t.level = "2",
  group.var = "group",
  strata.var = NULL,
  base.size = 16,
  theme.choice = "classic",
  palette = NULL,
  pdf = TRUE,
  file.ann = NULL,
  pdf.wid = 11,
  pdf.hei = 8.5)
```

<figure><img src="../.gitbook/assets/Screenshot 2023-06-11 at 19.46.41.png" alt=""><figcaption><p>Boxplot of the Simpson alpha diversity index for samples at the specific time point '2'. The plot shows the species complexity for each group at this particular time point, offering insights into the potential impact of the probiotic intervention.</p></figcaption></figure>

Finally, we incorporate the strata variable `sex` into our analysis, adding another layer of complexity:

```r
generate_alpha_boxplot_single(
  data.obj = peerj32.obj,
  alpha.obj = NULL,
  alpha.name = c("simpson"),
  subject.var = "subject",
  time.var = "time",
  t.level = "2",
  group.var = "group",
  strata.var = "sex",
  base.size = 16,
  theme.choice = "classic",
  palette = NULL,
  pdf = TRUE,
  file.ann = NULL,
  pdf.wid = 11,
  pdf.hei = 8.5)
```

<figure><img src="../.gitbook/assets/Screenshot 2023-06-11 at 19.47.28.png" alt=""><figcaption><p>Boxplot of the Simpson alpha diversity index for samples at time point '2', considering the strata variable 'sex'. The plot provides an added layer of understanding by showcasing the species complexity stratified by sex and group. This helps to identify any sex-specific effects of the intervention on the microbiome's species complexity.</p></figcaption></figure>

The alpha diversity index in our examples is the Simpson index, but users have the flexibility to input a vector of indices to generate a **list of plots**. The supported indices include **"shannon"**, **"simpson"**, **"observed\_species"**, **"chao1"**, **"ace"**, and **"pielou"**. Each of these measures offers unique insights into the richness and evenness of species in your samples.
