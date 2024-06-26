# Alpha Diversity Analysis

Welcome to this tutorial on Alpha Diversity Analysis for Paired Samples. This analysis will involve studying variations in alpha diversity across different time points and groups. Our primary tools will be illustrative boxplots coupled with formal statistical tests, which will assist in identifying potential informative patterns in alpha diversity. One of our main objectives is to explore changes in alpha diversity within the same group and compare the changes between groups.

MicrobiomeStat supports "shannon", "simpson", "observed\_species", "chao1", "ace", and "pielou". Each of these metrics furnishes distinct insights into the species richness and evenness characteritics in the microbiome samples.

For those functions performing alpha diversity analysis, they all include an `alpha.obj` parameter, which accepts a matrix of  alpha diversity measures (row - samples, column - measures).  `mStat_calculate_alpha_diversity` could be used to generate the common alpha diversity measures.  If `alpha.obj` is NULL, `mStat_calculate_alpha_diversity` will be called autonomously. To speed up computation, we recommend calling `mStat_calculate_alpha_diversity` once and store the alpha diversity measures in an object, which can be used later repeatedly.

To note,  `mStat_calculate_alpha_diversity` does not perform rarefaction. Rarefaction is important in diversity analysis since this process ensures the datasets are rendered more comparable by equalizing the sequencing depth across samples. Such standardization is vital for comparative analyses. You can use `mStat_data_rarefy` to rarefy our data.  

The first step in this process involves understanding how to test for differences in alpha diversity across timepoints and/or groups. To this end, MicrobiomeStat provides the `generate_alpha_test_pair()` function. This function fits a linear mixed model to detect changes in alpha diversity between time points and the difference of the changes between groups. The model takes the time variable, group variable, and any additional adjustment variables as fixed effects, and the subject variable as a random effect. The function generates a list of coefficient tables, one for each alpha diversity index. These tables present the term, estimate, standard error, t-value, and p-value for each fixed effect in the model.

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
The output of this function is a list of summary tables for each alpha diversity measure tested. Each table contains columns for Term (variable name), Estimate (coefficient), Std.Error, Statistic (t or F), and P.Value.

Here is an example of what the output might look like:

<table><thead><tr><th>Term</th><th width="121">Estimate</th><th width="133">Std.Error</th><th width="144">Statistic</th><th>P.Value</th></tr></thead><tbody><tr><td>(Intercept)</td><td>3.53</td><td>0.0372</td><td>95.0</td><td>1.25e-43</td></tr><tr><td>groupPlacebo</td><td>0.0994</td><td>0.0436</td><td>2.28</td><td>2.87e-2</td></tr><tr><td>time2</td><td>0.0589</td><td>0.0433</td><td>1.36</td><td>1.88e-1</td></tr><tr><td>sexmale</td><td>-0.0548</td><td>0.0353</td><td>-1.55</td><td>1.37e-1</td></tr><tr><td>groupPlacebo:time2</td><td>-0.0548</td><td>0.0542</td><td>-1.01</td><td>3.25e-1</td></tr></tbody></table>

 In this example, we are particularly interested in testing whether there are time effects ("time2" term), i.e., whether the alpha diversity index changes over the two time points,  and wether the changes differ by groups ("groupPlacebo:time2" interaction term). An omnibus p-value is also provided to test the null hypothesis of no group effect at both time points (i.e., the coefficients for "groupPlacebo" and "groupPlacebo:time2" are both zeros).

If the major interest is to test whether the time changes differ by groups,  MicrobiomeStat provides the `generate_alpha_change_test_pair()` function to directly test the changes in relation to the group variable. One advantage of this function is the permission of flexible defintion of alpha diversity change while the linear mixed model implicitly uses the absolute change. The function `generate_alpha_change_test_pair` includes a parameter  `alpha.change.func` that allows you to specify how the change in alpha diversity is calculated. By default, the function calculates the 'absolute change', which represents the difference between 't2' and 't1'. You can also use 'log fold change'. Or you can define your own change  via  `alpha.change.func`. For instance, you might want to define the ratio of alpha diversity at the 't2' to 't1' as the change. Here's an example of how you can do this:

```r
# Define custom function
change_ratio <- function(time_2, time_1) {
  return(time_2 / time_1)
}
```

In this case, the function 'change_ratio' is defined to calculate the ratio of alpha diversity at `time_2` to `time_1`. Once defined, you can pass 'change_ratio' to the `alpha.change.func` parameter in the `generate_alpha_change_test_pair` function. To note, currently the function does not account for time-varying covariates.  If you need to control the effects of time-varying covariates, you may need to use the mixed model approach.

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
  alpha.change.func = "absolute change",
  change.base = "1"  
)
```

The output of this function is a list of summary tables for each alpha diversity measure tested. Each table contains columns for Term (variable name), Estimate (coefficient), Std.Error, Statistic (t or F), and P.Value.

Here is an example of what the output might look like:

<table><thead><tr><th>Term</th><th width="128">Estimate</th><th width="141">Std.Error</th><th>Statistic</th><th>P.Value</th></tr></thead><tbody><tr><td>(Intercept)</td><td>0.0174</td><td>0.0140</td><td>1.24</td><td>0.230</td></tr><tr><td>sexmale</td><td>-0.000390</td><td>0.0164</td><td>-0.0238</td><td>0.981</td></tr><tr><td>groupPlacebo</td><td>-0.0154</td><td>0.0158</td><td>-0.975</td><td>0.342</td></tr></tbody></table>

To visually explore alpha diversity changes, MicrobiomeStat facilites visualizing both time points via  `generate_alpha_boxplot_long` function or visualizing the changes via  `generate_alpha_change_boxplot_pair`. The `generate_alpha_boxplot_long` function is developed for general longitudinal data with multiple time points. As a special case, it can be applied to the paired samples case by specifying the start time point (`t0.level`) and the end time point (`ts.level`). The function generates an  alpha diversity boxplot, with lines connecting the dots representing the same subject. These lines provide a visual of how individual subjects' alpha diversity changes over time.

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

<figure><img src="../.gitbook/assets/Screenshot 2023-10-11 at 14.37.51.png" alt=""><figcaption></figcaption></figure>

In addition, MicrobiomeStat provides the `generate_alpha_change_boxplot_pair` function, which allows the exploration of the changes in relation to the group variable with or without stratification. The function is specifically designed for paired samples designs. 
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

<figure><img src="../.gitbook/assets/Screenshot 2023-10-11 at 14.39.14.png" alt=""><figcaption></figcaption></figure>
