---
description: >-
  Diving into alpha diversity analysis for longitudinal studies. How does
  within-sample diversity change over time? This page provides the tools to
  answer this question.
---

# Chronological Journey: Alpha Diversity Analysis in Longitudinal Studies

This section introduces alpha diversity analysis in longitudinal studies, focusing on how within-sample diversity changes over time. The first step is to filter out samples with low sequencing depth, as alpha diversity analysis is sensitive to this factor.

It's worth noting that while our primary focus here lies on the Shannon index and Observed Species, MicrobiomeStat is versatile, supporting a variety of indices. These encompass "shannon", "simpson", "observed\_species", "chao1", "ace", and "pielou". Each of these metrics furnishes distinct insights into the species richness and evenness inherent in the microbiome samples.

Before delving further, it's pivotal to comprehend the significance of `alpha.obj`. This parameter encapsulates pre-computed alpha diversity indices, a byproduct of the `mStat_calculate_alpha_diversity` function. If this element isn't pre-processed and integrated into the alpha-centric functions, those functions default to invoking `mStat_calculate_alpha_diversity` autonomously.

Additionally, the alpha functions in MicrobiomeStat, by default, undertake data rarefaction. This process ensures the datasets are rendered more comparable by harmonizing the sequencing depth across samples. Such standardization is vital for comparative analyses. However, if you're aiming to work with the original, non-rarefied data when determining alpha diversity, it's imperative to pre-calculate `alpha.obj`. By doing so, the intrinsic rarefaction process is sidestepped, thereby maintaining the data's originality.

To account for potential confounding factors that may influence the relationship between alpha diversity and time, the function allows for the inclusion of additional variables (`adj.vars`) in the model. These variables are adjusted for in the linear mixed-effects model, ensuring that the influence of time on alpha diversity is evaluated more accurately.

After the initial preparation, we can use MicrobiomeStat's functions to test for differences in alpha diversity across timepoints. One such function is `generate_alpha_trend_test_long`, which uses a linear mixed-effects model to analyze longitudinal alpha diversity data. It tests whether alpha diversity changes significantly over time, while taking into account individual variability and other potential confounding factors. The mixed-effects model allows for within-subject and between-subject variability, making it suitable for unbalanced data and repeated measures. The trend test checks if the association between alpha diversity and time is statistically significant, providing insights into temporal dynamics in microbiome diversity.

The `time.var` argument in the function `generate_alpha_trend_test_long` needs to be numeric. If you provide a string, it will automatically be converted to a factor and then to numeric, which might cause issues. Please ensure that the time variable is numeric to avoid any potential problems.

```r
data("subset_T2D.obj") 
alpha_trend_test_results <- generate_alpha_trend_test_long(
  data.obj = subset_T2D.obj,
  alpha.name = c("shannon","observed_species"),
  time.var = "visit_number_num",
  subject.var = "subject_id",
  group.var = "subject_race",
  adj.vars = NULL
)
```

**Shannon Diversity**

| Term                                                 | Estimate | Std.Error | Statistic | P.Value  |
| ---------------------------------------------------- | -------- | --------- | --------- | -------- |
| (Intercept)                                          | 2.59     | 0.204     | 12.7      | 5.13e-27 |
| subject\_racecaucasian                               | 0.214    | 0.231     | 0.928     | 3.55e-1  |
| subject\_racehispanic\_or\_latino                    | 0.197    | 0.441     | 0.446     | 6.56e-1  |
| visit\_number\_num                                   | -0.00852 | 0.0547    | -0.156    | 8.76e-1  |
| subject\_racecaucasian:visit\_number\_num            | -0.0105  | 0.0617    | -0.170    | 8.65e-1  |
| subject\_racehispanic\_or\_latino:visit\_number\_num | 0.0515   | 0.116     | 0.443     | 6.58e-1  |
| subject\_race:visit\_number\_num                     | NA       | NA        | 0.175     | 8.40e-1  |

**Observed Species Diversity**

| Term                                                 | Estimate | Std.Error | Statistic | P.Value  |
| ---------------------------------------------------- | -------- | --------- | --------- | -------- |
| (Intercept)                                          | 118      | 16.1      | 7.36      | 3.22e-12 |
| subject\_racecaucasian                               | 23.2     | 18.2      | 1.28      | 2.02e-1  |
| subject\_racehispanic\_or\_latino                    | 20.1     | 34.6      | 0.580     | 5.62e-1  |
| visit\_number\_num                                   | -0.152   | 4.35      | -0.0349   | 9.72e-1  |
| subject\_racecaucasian:visit\_number\_num            | -0.792   | 4.90      | -0.162    | 8.72e-1  |
| subject\_racehispanic\_or\_latino:visit\_number\_num | 2.71     | 9.28      | 0.292     | 7.70e-1  |
| subject\_race:visit\_number\_num                     | NA       | NA        | 0.0910    | 9.13e-1  |

In the trend test, our primary focus is on the interaction term between `group.var` and `time.var`. When the levels of `group.var` are greater than 2, an ANOVA is performed, which is represented in the last row of the table.

Another useful function is `generate_alpha_volatility_test_long`. This function calculates the volatility of alpha diversity measures in longitudinal data and tests the association between the volatility and a group variable. Volatility is calculated as the mean of absolute differences between consecutive alpha diversity measures, normalized by the time difference.

In mathematical terms, volatility (V) can be represented like this:

First, let's define a few terms:

* "alpha\_i" is the alpha diversity measure at time "i".
* "delta\_t\_i" is the time difference between time "i" and "i+1".

With these terms, the volatility for a subject is calculated as:

$$
V = \frac{1}{N} \sum \left| \frac{\alpha_{i+1} - \alpha_i}{\Delta t_i} \right|
$$

Here:

* "N" is the total number of time points for the subject.
* The summation "sum" is over all time points for which "alpha\_(i+1)" is defined.

```r
data("subset_T2D.obj") 
alpha_volatility_test_results <- generate_alpha_volatility_test_long(
  data.obj = subset_T2D.obj,
  alpha.obj = NULL,
  alpha.name = c("shannon","observed_species"),
  time.var = "visit_number_num",
  subject.var = "subject_id",
  group.var = "subject_race",
  adj.vars = "sample_body_site"
)
```

**Shannon**

| Term                              | Estimate | Std.Error | Statistic |     P.Value |
| --------------------------------- | -------: | --------: | --------: | ----------: |
| (Intercept)                       |    0.544 |    0.0959 |      5.67 | 0.000000423 |
| subject\_racecaucasian            |   0.0222 |     0.108 |     0.205 |       0.838 |
| subject\_racehispanic\_or\_latino |  -0.0705 |     0.222 |    -0.318 |       0.752 |
| subject\_race                     |       NA |        NA |     0.114 |       0.893 |
| Residuals                         |       NA |        NA |        NA |          NA |

**Observed Species**

| Term                              | Estimate | Std.Error | Statistic | P.Value |
| --------------------------------- | -------: | --------: | --------: | ------: |
| (Intercept)                       |     40.6 |      5.82 |      6.98 | 2.51e-9 |
| subject\_racecaucasian            |    -1.95 |      6.56 |    -0.297 | 7.67e-1 |
| subject\_racehispanic\_or\_latino |    -7.04 |      13.4 |    -0.524 | 6.02e-1 |
| subject\_race                     |       NA |        NA |     0.143 | 8.67e-1 |
| Residuals                         |       NA |        NA |        NA |      NA |

Before we proceed with the visualization, it's crucial to understand the `time.var`, `t0.level`, and `ts.levels` parameters used in the functions `generate_alpha_spaghettiplot_long` and `generate_alpha_boxplot_long`.

The `time.var` parameter can take three forms:

- Numeric: When `time.var` is numeric, you don't need to set `t0.level` and `ts.levels`, as they will be automatically arranged in ascending order.

- Factor: If `time.var` is a factor, and if `t0.level` and `ts.levels` are not set, they will be automatically arranged according to the levels of the factor. However, if you have specific needs for the order of the levels, you can manually adjust `t0.level` and `ts.levels`.

- Character: If `time.var` is character, it's recommended to manually set `t0.level` and `ts.levels`, as the automatic arrangement might not reflect the correct order.

The `t0.level` parameter represents the first time point, while `ts.levels` represents the other time points, arranged in the order you desire, excluding `t0.level`.

Understanding these parameters is pivotal for effective visualization of temporal changes in alpha diversity. Now, let's proceed with creating the spaghetti plot and box plot to visualize alpha diversity changes over time for each individual in the study, grouped by race.

```r
# Generate a spaghetti plot of alpha diversity over time
generate_alpha_spaghettiplot_long(
  data.obj = subset_T2D.obj,
  alpha.obj = NULL,
  alpha.name = c("shannon"),
  depth = NULL,
  subject.var = "subject_id",
  time.var = "visit_number",
  t0.level = sort(unique(subset_T2D.obj$meta.dat$visit_number))[1],
  ts.levels = sort(unique(subset_T2D.obj$meta.dat$visit_number))[-1],
  group.var = "subject_race",
  strata.var = NULL,
  theme.choice = "bw",
  palette = NULL,
  pdf = TRUE,
  file.ann = NULL,
  pdf.wid = 11,
  pdf.hei = 8.5
)
```

<figure><img src="../.gitbook/assets/Screenshot 2023-10-12 at 09.38.19.png" alt=""><figcaption><p>This plot illustrates the temporal changes in the alpha diversity of each individual, stratified by subject race. This provides insights into the impact of race on microbiome diversity trends over time.</p></figcaption></figure>

Before we proceed with the `generate_alpha_boxplot_long()` function, it's important to note that when dealing with multiple time points, a boxplot might not provide the most effective visualization. The boxplot can become cluttered and harder to interpret with the addition of more time points. In such cases, a spaghetti plot, as generated by the `generate_alpha_spaghettiplot_long()` function, is often a better choice as it can more clearly illustrate the individual changes over time.

However, if you still wish to use a boxplot for visualizing alpha diversity over time, here's how you can do it with the `generate_alpha_boxplot_long()` function:

```r
# Render a boxplot encapsulating alpha diversity across chosen time points
generate_alpha_boxplot_long(
  data.obj = subset_T2D.obj,
  alpha.obj = NULL,
  alpha.name = c("shannon"),
  depth = NULL,
  subject.var = "subject_id",
  time.var = "visit_number_num",
  t0.level = sort(unique(subset_T2D.obj$meta.dat$visit_number_num))[1],
  ts.levels = sort(unique(subset_T2D.obj$meta.dat$visit_number_num))[2:4],
  group.var = "subject_race",
  strata.var = NULL,
  base.size = 20,
  theme.choice = "bw",
  palette = NULL,
  pdf = TRUE,
  file.ann = NULL,
  pdf.wid = 20,
  pdf.hei = 8.5
)
```

<figure><img src="../.gitbook/assets/Screenshot 2023-10-12 at 09.42.30.png" alt=""><figcaption><p>This boxplot displays the distribution of Shannon diversity over selected time points, segmented by subject race. This plot provides insights into the influence of race on shifts in microbiome diversity over time.</p></figcaption></figure>

In the following sections, we will continue to explore the temporal intricacies of microbiome data using methods provided by MicrobiomeStat.
