# Alpha Diversity Analysis

Welcome to this tutorial on Alpha Diversity Analysis within Paired Samples. This analysis will involve studying variations in alpha diversity across different time points and groups. Our primary tools will be illustrative boxplots, which will assist in identifying potential changes or disparities in alpha diversity. One of our main objectives is to explore transformations in alpha diversity within the same group across two time points.

MicrobiomeStat supports "shannon", "simpson", "observed\_species", "chao1", "ace", and "pielou". Each of these metrics furnishes distinct insights into the species richness and evenness inherent in the microbiome samples.

For those functions performing alpha diversity analysis, they all include an alpha.obj parameter, which is a list output from calling `mStat_calculate_alpha_diversity` If alpha.obj parameter is NULL, `mStat_calculate_alpha_diversity` will be called autonomously. To speed up computation, we recommend calling `mStat_calculate_alpha_diversity` once and store the alpha diversity indices in the alpha.obj, which can be used later repeatedly.

To note, `mStat_calculate_alpha_diversity` by deafult, undertakes data rarefaction. This process ensures the datasets are rendered more comparable by equalizing the sequencing depth across samples. Such standardization is vital for comparative analyses. You can set the desired rarefaction depth in `mStat_calculate_alpha_diversity` or if not specified, the minimum sequencing depth will be used. If you aim to work with non-rarefied data when determining alpha diversity, You can pre-calculate your own alpha diversity indices and pass them to the alpha.obj parameter in these alpha diversity analysis functions.

The first step in this process involves understanding how to test for differences in alpha diversity across timepoints or groups. To this end, MicrobiomeStat provides the `generate_alpha_test_pair()` function. This function fits a linear mixed model to detect changes in alpha diversity within paired samples designs. The model takes into account the time variable, group variable, and any additional adjustment variables as fixed effects, and the subject variable as a random effect. The function generates a list of coefficient tables, one for each alpha diversity index. These tables present the term, estimate, standard error, t-value, and p-value for each fixed effect in the model.

Here is an example of its application:

```r
data(peerj32.obj)

generate_alpha_test_pair(
   data.obj = peerj32.obj,
   alpha.obj = NULL,
   time.var = "time",
   alpha.name = c("shannon","observed_species"),
   subject.var = "subject",
   group.var = "group",
   adj.vars = "sex"
)
```

<table><thead><tr><th>Term</th><th width="121">Estimate</th><th width="133">Std.Error</th><th width="144">Statistic</th><th>P.Value</th></tr></thead><tbody><tr><td>(Intercept)</td><td>3.53</td><td>0.0372</td><td>95.0</td><td>1.25e-43</td></tr><tr><td>groupPlacebo</td><td>0.0994</td><td>0.0436</td><td>2.28</td><td>2.87e-2</td></tr><tr><td>time2</td><td>0.0589</td><td>0.0433</td><td>1.36</td><td>1.88e-1</td></tr><tr><td>sexmale</td><td>-0.0548</td><td>0.0353</td><td>-1.55</td><td>1.37e-1</td></tr><tr><td>groupPlacebo:time2</td><td>-0.0548</td><td>0.0542</td><td>-1.01</td><td>3.25e-1</td></tr></tbody></table>

The output from this function allows us to test differences in alpha diversity across timepoints or groups, contributing to our understanding of changes in microbial diversity.

To complement this analysis, MicrobiomeStat provides the `generate_alpha_change_test_pair()` function. This function offers a different perspective by comparing alpha diversity metrics between two time points using linear models and ANOVA.

The function `generate_alpha_change_test_pair()` includes a parameter called `alpha.change.func` that allows you to specify how the change in alpha diversity is calculated. By default, the function calculates the 'absolute change', which represents the direct difference between two timepoints. However, you can also specify 'log fold change' to capture the log2 fold change.

If you want to customize how the change in alpha diversity is calculated, you can provide your own function for `alpha.change.func`. For instance, you might want to calculate the ratio of alpha diversity at the second time point to the first time point. Here's an example of how you can do this:

```r
# Define custom function
change_ratio <- function(time_2, time_1) {
  return(time_2 / time_1)
}
```

In this case, the function `change_ratio` is defined to calculate the ratio of alpha diversity at `time_2` to `time_1`. You can then use `change_ratio` as your `alpha.change.func` in the `generate_alpha_change_test_pair()` function.

Let's see an example of its application:

```r
library(vegan)
data(peerj32.obj)

generate_alpha_change_test_pair(
  data.obj = peerj32.obj,
  alpha.obj = NULL,
  alpha.name = c("shannon","observed_species"), 
  depth = NULL,
  time.var = "time",
  subject.var = "subject",
  group.var = "group",
  adj.vars = "sex",
  change.base = "1"  
)
```

The output of this function is a list of summary tables for each alpha diversity metric tested. Each table contains columns for Term (variable name), Estimate (coefficient), Std.Error, Statistic (t or F), and P.Value.

Here is an example of what the output might look like:

<table><thead><tr><th>Term</th><th width="128">Estimate</th><th width="141">Std.Error</th><th>Statistic</th><th>P.Value</th></tr></thead><tbody><tr><td>(Intercept)</td><td>0.0174</td><td>0.0140</td><td>1.24</td><td>0.230</td></tr><tr><td>sexmale</td><td>-0.000390</td><td>0.0164</td><td>-0.0238</td><td>0.981</td></tr><tr><td>groupPlacebo</td><td>-0.0154</td><td>0.0158</td><td>-0.975</td><td>0.342</td></tr></tbody></table>

To further enhance our understanding of alpha diversity changes, MicrobiomeStat provides the `generate_alpha_boxplot_long` function. This function is particularly useful as it can handle multiple time point scenarios. It generates an informative alpha diversity boxplot, with lines connecting the boxplots representing the same subject across different time points. These lines provide a visual representation of how individual subjects' alpha diversity changes over time.

Here's how you can use this function:

```r
generate_alpha_boxplot_long(
  data.obj = peerj32.obj,
  alpha.obj = NULL,
  alpha.name = c("shannon","observed_species"),
  depth = NULL,
  subject.var = "subject",
  time.var = "time",
  t0.level = "1",
  ts.levels = "2",
  group.var = "group",
  strata.var = "sex",
  base.size = 20,
  theme.choice = "bw",
  palette = NULL,
  pdf = TRUE,
  file.ann = NULL,
  pdf.wid = 11,
  pdf.hei = 8.5
)
```

<figure><img src="../.gitbook/assets/Screenshot 2023-10-11 at 14.37.51.png" alt=""><figcaption><p>This boxplot, crafted by <strong>generate_alpha_boxplot_long()</strong>, showcases the Simpson diversity index across multiple time points for each group. The connecting lines represent individual subjects over time, intuitively visualizing each subject's alpha diversity change.</p></figcaption></figure>

Note, however, that certain visualizations may not be as effective when applied to a paired samples design. For such cases, MicrobiomeStat provides the `generate_alpha_change_boxplot_pair()` function, which is specifically designed for paired samples designs. This function is particularly useful for visualizing shifts in alpha diversity.

Here's an example of its usage:

```r
generate_alpha_change_boxplot_pair(
   data.obj = peerj32.obj,
   alpha.obj = NULL,
   alpha.name = c("shannon","observed_species"),
   depth = NULL,
   subject.var = "subject",
   time.var = "time",
   group.var = "group",
   strata.var = "sex",
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

<figure><img src="../.gitbook/assets/Screenshot 2023-10-11 at 14.39.14.png" alt=""><figcaption><p>This boxplot, generated by <strong>generate_alpha_change_boxplot_pair()</strong>, illustrates the log 2 fold change in alpha diversity between groups. The connecting lines highlight the changes for individual subjects, providing an intuitive view of group differences.</p></figcaption></figure>
