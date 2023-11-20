# Alpha Diversity Analysis

Alpha diversity indices offer insights into the species richness and evenness within individual microbiome samples.&#x20;

MicrobiomeStat supports a variety of indices including "shannon", "simpson", "observed\_species", "chao1", "ace", and "pielou". Each of these metrics furnishes distinct insights into the species richness and evenness inherent in the microbiome samples. The alpha diversity indices can be conveniently calculated using `mStat_calculate_alpha_diversity` function.

For those functions performing alpha diversity analysis, they all include an alpha.obj parameter, which is a list output from calling `mStat_calculate_alpha_diversity` If alpha.obj parameter is NULL, `mStat_calculate_alpha_diversity` will be called autonomously. To speed up computation, we recommend calling `mStat_calculate_alpha_diversity` once and store the alpha diversity indices in the alpha.obj, which can be used later repeatedly.

To note, `mStat_calculate_alpha_diversity` by deafult, undertakes data rarefaction. This process ensures the datasets are rendered more comparable by equalizing the sequencing depth across samples. Such standardization is vital for comparative analyses. You can set the desired rarefaction depth in `mStat_calculate_alpha_diversity` or if not specified, the minimum sequencing depth will be used. If you aim to work with non-rarefied data when determining alpha diversity, You can pre-calculate your own alpha diversity indices and pass them to the alpha.obj parameter in these alpha diversity analysis functions.

Next, we will illustrate how to perform alpha diversity analysis for cross-sectional data (or case-control data) using peerj32.obj.&#x20;

The first task is to test the association between alpha diversity and a variable of interest while adjusting for other variables (such as confounders and independent predictors). We achieve this use`generate_alpha_test_single`, which is based on multiple linear models.

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

If one wishes to focus on a specific time point, values for `time.var` (name of time variable) and `t.level`(the name of the specific time point) should be specified. In this example, we did not provide a pre-caluclated alpha.obj (alpha.obj = NULL). Thus, `mStat_calculate_alpha_diversity` will be automatically called.

For each specified index in `alpha.name`, a linear model is fit, with `group.var` as the primary predictor, covariates are adjusted through `adj.vars`. The function's output consists of model output that contains coefficients, standard errors, test statistics, and p-values.

In scenarios where `group.var` encapsulates multiple categories, ANOVA will be employed to test the global hypothesis of no difference in means between all the groups.

#### Shannon Index Results

| Term         | Estimate | Std.Error | Statistic | P.Value  |
| ------------ | -------- | --------- | --------- | -------- |
| (Intercept)  | 3.60     | 0.0386    | 93.2      | 9.47e-27 |
| sexmale      | -0.0633  | 0.0451    | -1.40     | 1.77e-1  |
| groupPlacebo | 0.0415   | 0.0437    | 0.950     | 3.54e-1  |

#### Observed Species Results

| Term         | Estimate | Std.Error | Statistic | P.Value  |
| ------------ | -------- | --------- | --------- | -------- |
| (Intercept)  | 100.     | 3.72      | 26.9      | 1.36e-16 |
| sexmale      | 2.29     | 4.35      | 0.528     | 6.04e-1  |
| groupPlacebo | 1.99     | 4.21      | 0.473     | 6.42e-1  |

The visualization of these findings can be performed using `generate_alpha_boxplot_single` function. First, plot all observations and ignore the time.var and strata.var.

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

Next, we will plot the particular time point (time point 2 in this example) by setting `t.level = '2'`.

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

For a detailed inspection, we can further stratify the analysis based on sex by specifying "strata.var = "sex".

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
