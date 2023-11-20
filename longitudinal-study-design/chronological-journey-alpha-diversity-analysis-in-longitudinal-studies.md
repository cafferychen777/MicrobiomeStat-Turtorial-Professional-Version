# Alpha Diversity Analysis

This section introduces alpha diversity analysis in longitudinal studies, focusing on how within-sample diversity changes over time. The first step is to filter out samples with low sequencing depth, as alpha diversity analysis is sensitive to this factor.

MicrobiomeStat includes "shannon", "simpson", "observed\_species", "chao1", "ace", and "pielou". Each of these metrics furnishes distinct insights into the species richness and evenness inherent in the microbiome samples.

For those functions performing alpha diversity analysis, they all include an alpha.obj parameter, which is a list output from calling `mStat_calculate_alpha_diversity` If alpha.obj parameter is NULL, `mStat_calculate_alpha_diversity` will be called autonomously. To speed up computation, we recommend calling `mStat_calculate_alpha_diversity` once and store the alpha diversity indices in the alpha.obj, which can be used later repeatedly.

To note, `mStat_calculate_alpha_diversity` by deafult, undertakes data rarefaction. This process ensures the datasets are rendered more comparable by equalizing the sequencing depth across samples. Such standardization is vital for comparative analyses. You can set the desired rarefaction depth in `mStat_calculate_alpha_diversity` or if not specified, the minimum sequencing depth will be used. If you aim to work with non-rarefied data when determining alpha diversity, You can pre-calculate your own alpha diversity indices and pass them to the alpha.obj parameter in these alpha diversity analysis functions.

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

<table><thead><tr><th>Term</th><th width="114">Estimate</th><th width="123">Std.Error</th><th width="118">Statistic</th><th>P.Value</th></tr></thead><tbody><tr><td>(Intercept)</td><td>2.59</td><td>0.204</td><td>12.7</td><td>5.13e-27</td></tr><tr><td>subject_racecaucasian</td><td>0.214</td><td>0.231</td><td>0.928</td><td>3.55e-1</td></tr><tr><td>subject_racehispanic_or_latino</td><td>0.197</td><td>0.441</td><td>0.446</td><td>6.56e-1</td></tr><tr><td>visit_number_num</td><td>-0.00852</td><td>0.0547</td><td>-0.156</td><td>8.76e-1</td></tr><tr><td>subject_racecaucasian:visit_number_num</td><td>-0.0105</td><td>0.0617</td><td>-0.170</td><td>8.65e-1</td></tr><tr><td>subject_racehispanic_or_latino:visit_number_num</td><td>0.0515</td><td>0.116</td><td>0.443</td><td>6.58e-1</td></tr><tr><td>subject_race:visit_number_num</td><td>NA</td><td>NA</td><td>0.175</td><td>8.40e-1</td></tr></tbody></table>

**Observed Species Diversity**

<table><thead><tr><th>Term</th><th width="112">Estimate</th><th width="125">Std.Error</th><th width="131">Statistic</th><th>P.Value</th></tr></thead><tbody><tr><td>(Intercept)</td><td>118</td><td>16.1</td><td>7.36</td><td>3.22e-12</td></tr><tr><td>subject_racecaucasian</td><td>23.2</td><td>18.2</td><td>1.28</td><td>2.02e-1</td></tr><tr><td>subject_racehispanic_or_latino</td><td>20.1</td><td>34.6</td><td>0.580</td><td>5.62e-1</td></tr><tr><td>visit_number_num</td><td>-0.152</td><td>4.35</td><td>-0.0349</td><td>9.72e-1</td></tr><tr><td>subject_racecaucasian:visit_number_num</td><td>-0.792</td><td>4.90</td><td>-0.162</td><td>8.72e-1</td></tr><tr><td>subject_racehispanic_or_latino:visit_number_num</td><td>2.71</td><td>9.28</td><td>0.292</td><td>7.70e-1</td></tr><tr><td>subject_race:visit_number_num</td><td>NA</td><td>NA</td><td>0.0910</td><td>9.13e-1</td></tr></tbody></table>

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

<table><thead><tr><th align="center">Term</th><th width="127" align="center">Estimate</th><th width="112" align="center">Std.Error</th><th align="center">Statistic</th><th align="center">P.Value</th></tr></thead><tbody><tr><td align="center">(Intercept)</td><td align="center">0.544</td><td align="center">0.0959</td><td align="center">5.67</td><td align="center">0.000000423</td></tr><tr><td align="center">subject_racecaucasian</td><td align="center">0.0222</td><td align="center">0.108</td><td align="center">0.205</td><td align="center">0.838</td></tr><tr><td align="center">subject_racehispanic_or_latino</td><td align="center">-0.0705</td><td align="center">0.222</td><td align="center">-0.318</td><td align="center">0.752</td></tr><tr><td align="center">subject_race</td><td align="center">NA</td><td align="center">NA</td><td align="center">0.114</td><td align="center">0.893</td></tr><tr><td align="center">Residuals</td><td align="center">NA</td><td align="center">NA</td><td align="center">NA</td><td align="center">NA</td></tr></tbody></table>

**Observed Species**

<table><thead><tr><th width="148">Term</th><th width="141" align="center">Estimate</th><th width="109" align="center">Std.Error</th><th width="185" align="center">Statistic</th><th align="center">P.Value</th></tr></thead><tbody><tr><td>(Intercept)</td><td align="center">40.6</td><td align="center">5.82</td><td align="center">6.98</td><td align="center">2.51e-9</td></tr><tr><td>subject_racecaucasian</td><td align="center">-1.95</td><td align="center">6.56</td><td align="center">-0.297</td><td align="center">7.67e-1</td></tr><tr><td>subject_racehispanic_or_latino</td><td align="center">-7.04</td><td align="center">13.4</td><td align="center">-0.524</td><td align="center">6.02e-1</td></tr><tr><td>subject_race</td><td align="center">NA</td><td align="center">NA</td><td align="center">0.143</td><td align="center">8.67e-1</td></tr><tr><td>Residuals</td><td align="center">NA</td><td align="center">NA</td><td align="center">NA</td><td align="center">NA</td></tr></tbody></table>

Before we proceed with the visualization, it's crucial to understand the `time.var`, `t0.level`, and `ts.levels` parameters used in the functions `generate_alpha_spaghettiplot_long` and `generate_alpha_boxplot_long`.

The `time.var` parameter can take three forms:

* Numeric: When `time.var` is numeric, you don't need to set `t0.level` and `ts.levels`, as they will be automatically arranged in ascending order.
* Factor: If `time.var` is a factor, and if `t0.level` and `ts.levels` are not set, they will be automatically arranged according to the levels of the factor. However, if you have specific needs for the order of the levels, you can manually adjust `t0.level` and `ts.levels`.
* Character: If `time.var` is character, it's recommended to manually set `t0.level` and `ts.levels`, as the automatic arrangement might not reflect the correct order.

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
