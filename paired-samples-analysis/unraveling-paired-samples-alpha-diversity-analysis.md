---
description: >-
  An introduction to exploring alpha diversity indices in a paired sample
  setting, highlighting species complexity within paired samples.
---

# Unraveling Paired Samples: Alpha Diversity Analysis

Prepare for an insightful journey into **Unraveling Paired Samples: Alpha Diversity Analysis**! We'll traverse through the diverse landscape of **alpha diversity**, examining variations across **different time points** and **groups**. Through the lens of **illustrative boxplots**, we'll probe into the existence of significant **changes or disparities** in alpha diversity. Additionally, we're set to explore if there are **transformations** in alpha diversity within the **same group across two time points**. So, let's dive in and witness the dynamic fluctuations within the microscopic world of microbiomes!

Before delving deeper into **alpha diversity analysis**, it is prudent to understand how to test for differences in alpha diversity across timepoints or groups. MicrobiomeStat provides the `generate_alpha_test_pair()` function, which utilizes a **linear mixed model** to detect changes in alpha diversity for paired samples designs.

The linear mixed model incorporates the time variable, group variable, and any additional adjustment variables as **fixed effects**, and the **subject variable** as a **random effect**.

The output is a list of **coefficient tables**, one for each alpha diversity index. Each table includes the term, estimate, standard error, **t-value**, and **p-value** for each fixed effect in the model.

Here is an example usage:

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

This allows us to **test** for differences in alpha diversity across timepoints or groups, and understand changes in microbial diversity. Next, we will further **visualize** the results using the `generate_alpha_change_boxplot_pair()` function.

Before visually exploring differences in **alpha diversity** changes between groups, it is essential to grasp how to test for differences in alpha diversity across timepoints. MicrobiomeStat provides the `generate_alpha_change_test_pair()` function to compare alpha diversity metrics between two time points.

The function supports various options for adjusting the tests and calculating alpha diversity changes. It will utilize **linear models** and **ANOVA** to differentiate between-group and within-group alpha diversity differences.

The output is a list of **summary tables** for each alpha diversity metric tested. Each table contains columns for: **Term** (variable name), **Estimate** (coefficient), **Std.Error**, **Statistic** (t or F), and **P.Value**.

Here is an example usage:

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

This allows us to **test** for alpha diversity changes across timepoints and understand between-group versus within-group differences. Next, we will **visualize** using `generate_alpha_change_boxplot_pair()`.

A quick note before we proceed further with our **alpha diversity analysis**. In the MicrobiomeStat toolbox, the functions ending in '**single**' are versatile and adaptable, capable of handling **single time point designs**, a **specific time point in paired samples designs**, or a **particular time point in longitudinal study designs**. Functions with the suffix '**pair**' are specifically tailored for **paired samples designs**, and those concluding with '**long**' are applicable to both **paired and multiple time point scenarios**. However, bear in mind, certain **visualizations may not appear aesthetically pleasing or insightful when applied to a paired samples design**. As we explore `generate_alpha_change_boxplot_pair` and `generate_alpha_boxplot_long`, this distinction will become clearer!

Delving into our alpha diversity analysis, we first employ the `generate_alpha_boxplot_long` function:

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

This function produces an **informative alpha diversity boxplot**. Should you choose not to provide an **alpha.obj**, the function will conveniently call **mStat\_calculate\_alpha\_diversity** to do the calculations for you. What's particularly striking about this plot is the **lines linking the boxplots**; these lines represent the same subject across different time points. This provides an immediate visual of how **individual subjects' alpha diversity changes over time**. Please note that if you have more thRan **10 time points or more than 25 subjects**, the connecting lines will automatically be adjusted to represent the **average alpha diversity index within the same time point**, ensuring the plot remains clear and interpretable.

Diving deeper, we now explore the **generate\_alpha\_change\_boxplot\_pair()** function. Take a look at the sample code below:

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

This powerful tool is designed to **visualize shifts in alpha diversity**. With this function, **complex data** transforms into an easily **digestible format**, allowing you to swiftly pinpoint significant disparities between **two timepoints' alpha diversity changes**. This tool incorporates the **alpha.change.func** parameter, offering options of 'absolute change', 'log fold change', or even a custom function tailored to your analytical preferences. The 'absolute change' represents the **direct difference** between two timepoints, while 'log fold change' captures the **log2 fold change**. If a custom function is provided for **alpha.change.func**, it will be applied to compute the diversity changes as per the function's definition.

{% hint style="info" %}
**Hint**: You can **create your own `alpha.change.func`** to customize how the change in alpha diversity is calculated between two time points. Here's an example of how you can do it:

```r
# Define custom function
change_ratio <- function(time_2, time_1) {
  return(time_2 / time_1)
}
```

In this case, the function `change_ratio` is defined to calculate the **ratio** of alpha diversity at `time_2` to `time_1`. You can then use `change_ratio` as your `alpha.change.func` in the `generate_alpha_change_boxplot_pair()` function. This will result in the plot showing the **fold-change** in alpha diversity for each subject across the two time points.
{% endhint %}
