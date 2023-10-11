---
description: >-
  An introduction to exploring alpha diversity indices in a paired sample
  setting, highlighting species complexity within paired samples.
---

# Unraveling Paired Samples: Alpha Diversity Analysis

Welcome to this tutorial on Alpha Diversity Analysis within Paired Samples. This analysis will involve studying variations in alpha diversity across different time points and groups. Our primary tools will be illustrative boxplots, which will assist in identifying potential changes or disparities in alpha diversity. One of our main objectives is to explore transformations in alpha diversity within the same group across two time points.

The first step in this process involves understanding how to test for differences in alpha diversity across timepoints or groups. To this end, MicrobiomeStat provides the `generate_alpha_test_pair()` function. This function employs a linear mixed model to detect changes in alpha diversity within paired samples designs. The model takes into account the time variable, group variable, and any additional adjustment variables as fixed effects, and the subject variable as a random effect. The function generates a list of coefficient tables, one for each alpha diversity index. These tables present the term, estimate, standard error, t-value, and p-value for each fixed effect in the model.

Here is an example of its application:

```r
data(peerj32.obj)

generate_alpha_test_pair(
   data.obj = peerj32.obj,
   alpha.obj = NULL,
   time.var = "time",
   alpha.name = c("shannon"),
   subject.var = "subject",
   group.var = "group",
   adj.vars = "sex"
)
```

| Term               | Estimate | Std.Error | Statistic | P.Value   |
|--------------------|----------|----------|-----------|----------|
| (Intercept)        | 3.53     | 0.0372   | 95.0      | 1.25e-43 |
| groupPlacebo       | 0.0994   | 0.0436   | 2.28      | 2.87e-2  |
| time2              | 0.0589   | 0.0433   | 1.36      | 1.88e-1  |
| sexmale            | -0.0548  | 0.0353   | -1.55     | 1.37e-1  |
| groupPlacebo:time2 | -0.0548  | 0.0542   | -1.01     | 3.25e-1  |

The output from this function allows us to test differences in alpha diversity across timepoints or groups, contributing to our understanding of changes in microbial diversity. 

For a different perspective on these differences, MicrobiomeStat provides the `generate_alpha_change_test_pair()` function. This function compares alpha diversity metrics between two time points using linear models and ANOVA, offering a different angle on the analysis of diversity changes.

The output of this function is a list of summary tables for each alpha diversity metric tested. Each table contains columns for Term (variable name), Estimate (coefficient), Std.Error, Statistic (t or F), and P.Value.

Here is an example of its application:

```r
library(vegan)
data(peerj32.obj)

generate_alpha_change_test_pair(
  data.obj = peerj32.obj,
  alpha.obj = NULL,
  alpha.name = c("shannon"), 
  depth = NULL,
  time.var = "time",
  subject.var = "subject",
  group.var = "group",
  adj.vars = "sex",
  change.base = "1"  
)
```

| Term         | Estimate  | Std.Error | Statistic | P.Value |
| ------------ | --------- | --------- | --------- | ------- |
| (Intercept)  | 0.0174    | 0.0140    | 1.24      | 0.230   |
| sexmale      | -0.000390 | 0.0164    | -0.0238   | 0.981   |
| groupPlacebo | -0.0154   | 0.0158    | -0.975    | 0.342   |

The output from this function allows us to test for alpha diversity changes across timepoints and aids in understanding between-group versus within-group differences. 

In the MicrobiomeStat toolbox, there are three types of functions: those ending in 'single', 'pair', and 'long'. The 'single' functions are designed to handle single time point designs, a specific time point in paired samples designs, or a particular time point in longitudinal study designs. The 'pair' functions are specifically for paired samples designs, and the 'long' functions are applicable to both paired and multiple time point scenarios. However, it's important to note that certain visualizations may not be as effective when applied to a paired samples design. As we explore `generate_alpha_boxplot_long`, this distinction will become clearer.

To begin our alpha diversity analysis, we use the `generate_alpha_boxplot_long` function:

```r
generate_alpha_boxplot_long(
  data.obj = peerj32.obj,
  alpha.obj = NULL,
  alpha.name = c("simpson"),
  depth = NULL,
  subject.var = "subject",
  time.var = "time",
  t0.level = "1",
  ts.levels = "2",
  group.var = "group",
  strata.var = NULL,
  base.size = 20,
  theme.choice = "bw",
  palette = NULL,
  pdf = TRUE,
  file.ann = NULL,
  pdf.wid = 11,
  pdf.hei = 8.5
)
```

<figure><img src="../.gitbook/assets/Screenshot 2023-06-12 at 14.35.00.png" alt=""><figcaption><p>This boxplot, crafted by <strong>generate_alpha_boxplot_long()</strong>, showcases the Simpson diversity index across multiple time points for each group. The connecting lines represent individual subjects over time, intuitively visualizing each subject's alpha diversity change.</p></figcaption></figure>

This function generates an informative alpha diversity boxplot. The lines connecting the boxplots represent the same subject across different time points, providing a visual representation of how individual subjects' alpha diversity changes over time. 

To further delve into the analysis, we use the `generate_alpha_change_boxplot_pair()` function:

```r
generate_alpha_change_boxplot_pair(
   data.obj = peerj32.obj,
   alpha.obj = NULL,
   alpha.name = c("simpson"),
   depth = NULL,
   subject.var = "subject",
   time.var = "time",
   group.var = "group",
   strata.var = NULL,
   change.base = "1",
   alpha.change.func = "log fold change",
   base.size = 20,
   theme.choice = "bw",
   palette = NULL,
   pdf = TRUE,
   file.ann = "test",
   pdf.wid = 11,
   pdf.hei = 8.5
)
```

<figure><img src="../.gitbook/assets/Screenshot 2023-06-12 at 14.53.57.png" alt=""><figcaption><p>This boxplot, generated by <strong>generate_alpha_change_boxplot_pair()</strong>, illustrates the log 2 fold change in alpha diversity between groups. The connecting lines highlight the changes for individual subjects, providing an intuitive view of group differences.</p></figcaption></figure>

This function is designed to visualize shifts in alpha diversity. The 'absolute change' represents the direct difference between two timepoints, while 'log fold change' captures the log2 fold change. If a custom function is provided for `alpha.change.func`, it will be applied to compute the diversity changes as per the function's definition.

{% hint style="info" %}
Hint: You can create your own `alpha.change.func` to customize how the change in alpha diversity is calculated between two time points. Here's an example:

```r
# Define custom function
change_ratio <- function(time_2, time_1) {
  return(time_2 / time_1)
}
```

In this case, the function `change_ratio` is defined to calculate the ratio of alpha diversity at `time_2` to `time_1`. You can then use `change_ratio` as your `alpha.change.func` in the `generate_alpha_change_boxplot_pair()` function. This will result in the plot showing the fold-change in alpha diversity for each subject across the two time points.
{% endhint %}

