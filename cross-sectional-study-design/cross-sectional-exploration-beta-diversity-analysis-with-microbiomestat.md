---
description: >-
  Dive into MicrobiomeStat's tools for analyzing beta diversity in
  cross-sectional microbiome studies. Compute distances, visualize
  dissimilarities, and more!
---

# Cross-Sectional Exploration: Beta Diversity Analysis with MicrobiomeStat

**Simple Cross-Sectional Design**: With MicrobiomeStat, we can launch our exploration without any pre-calculations of `dist.obj` and `pc.obj`. They will be automatically computed based on `dist.name` in action. Upon generating the ordination plot, we can observe a significant difference in Axis 2 between the two groups:

Before visualizing beta diversity using `generate_beta_ordination_single`, we can quantitatively assess group differences using `generate_beta_test_single`. This function performs PERMANOVA on the specified distance matrices to test for significant differences between groups, adjusting for additional covariates.

```r
generate_beta_test_single(
  data.obj = peerj32.obj,
  dist.obj = NULL,
  time.var = "time",
  t.level = "2",
  group.var = "group", 
  adj.vars = "sex",
  dist.name = c('BC', 'Jaccard') 
)
```

#### PERMANOVA p-values

| Term  | BC    | Jaccard | Omnibus |
| ----- | ----- | ------- | ------- |
| sex   | 0.791 | 0.819   | 0.808   |
| group | 0.794 | 0.859   | 0.817   |

This table summarizes the p-values from PERMANOVA tests for each distance matrix and the omnibus test across all distances.

#### PERMANOVA ANOVA results

<table><thead><tr><th>Variable</th><th width="53">DF</th><th>Sum_Sq</th><th>Mean_Sq</th><th>F_Statistic</th><th>R_Squared</th><th>P_Value</th><th>Distance</th></tr></thead><tbody><tr><td>sex</td><td>1</td><td>0.043</td><td>0.043</td><td>0.644</td><td>0.032</td><td>0.791</td><td>BC</td></tr><tr><td>group</td><td>1</td><td>0.042</td><td>0.042</td><td>0.621</td><td>0.031</td><td>0.794</td><td>BC</td></tr><tr><td>Residuals</td><td>19</td><td>1.28</td><td>0.067</td><td>NA</td><td>0.938</td><td>NA</td><td>BC</td></tr><tr><td>Total</td><td>21</td><td>1.36</td><td>NA</td><td>NA</td><td>1</td><td>NA</td><td>BC</td></tr><tr><td>sex</td><td>1</td><td>0.099</td><td>0.099</td><td>0.707</td><td>0.035</td><td>0.819</td><td>Jaccard</td></tr><tr><td>group</td><td>1</td><td>0.094</td><td>0.094</td><td>0.668</td><td>0.033</td><td>0.859</td><td>Jaccard</td></tr><tr><td>Residuals</td><td>19</td><td>2.67</td><td>0.14</td><td>NA</td><td>0.933</td><td>NA</td><td>Jaccard</td></tr><tr><td>Total</td><td>21</td><td>2.86</td><td>NA</td><td>NA</td><td>1</td><td>NA</td><td>Jaccard</td></tr></tbody></table>

This table provides the detailed ANOVA results from PERMANOVA tests for each distance matrix, including degrees of freedom, sum of squares, F statistics, R squared, and p-values.

The PERMANOVA results contain p-value and ANOVA tables to evaluate if community structure differs significantly between groups.

Next, we visualize the beta diversity using `generate_beta_ordination_single`:

```r
generate_beta_ordination_single(
  data.obj = peerj32.obj,
  dist.obj = NULL,
  pc.obj = NULL,
  subject.var = "subject",
  time.var = NULL,
  t.level = NULL,
  group.var = "group",
  strata.var = NULL,
  dist.name = c("BC","Jaccard"),
  base.size = 20,
  theme.choice = "bw",
  custom.theme = NULL,
  palette = NULL,
  pdf = TRUE,
  file.ann = NULL,
  pdf.wid = 11,
  pdf.hei = 8.5
)
```

<figure><img src="../.gitbook/assets/Screenshot 2023-06-11 at 20.20.28.png" alt=""><figcaption><p>Beta Diversity Ordination (BC distance) for All Data: This graph provides an overview of the microbial community structure in our dataset, disregarding any time point distinction and stratification.</p></figcaption></figure>

Next, we'll shift our focus to a **specific time point**. By setting `t.level` to "2", we aim to uncover any significant beta diversity differences. Interestingly, at this time point, the difference in Axis 2 between the two groups becomes more pronounced:

<pre class="language-r"><code class="lang-r">generate_beta_ordination_single(
  data.obj = peerj32.obj,
  dist.obj = NULL,
  pc.obj = NULL,
  subject.var = "subject",
  time.var = "time",
  t.level = "2",
  group.var = "group",
  strata.var = NULL,
  dist.name = c("BC","Jaccard"),
  base.size = 20,
  theme.choice = "bw",
<strong>  custom.theme = NULL,
</strong>  palette = NULL,
  pdf = TRUE,
<strong>  file.ann = NULL,
</strong>  pdf.wid = 11,
  pdf.hei = 8.5
)
</code></pre>

<figure><img src="../.gitbook/assets/Screenshot 2023-06-11 at 20.21.25.png" alt=""><figcaption><p>Beta Diversity Ordination at Time Point '2' (BC distance): This visualization offers insights into how the microbial community structure varies at a specific time point.</p></figcaption></figure>

Finally, we'll introduce a **stratifying variable** `sex` into our analysis. This step lets us dissect the impact of gender on our microbiome data:

```r
generate_beta_ordination_single(
  data.obj = peerj32.obj,
  dist.obj = NULL,
  pc.obj = NULL,
  subject.var = "subject",
  time.var = "time",
  t.level = "2",
  group.var = "group",
  strata.var = "sex",
  dist.name = c("BC","Jaccard"),
  base.size = 20,
  theme.choice = "bw",
  custom.theme = NULL,
  palette = NULL,
  pdf = TRUE,
  file.ann = NULL,
  pdf.wid = 11,
  pdf.hei = 8.5
)
```

<figure><img src="../.gitbook/assets/Screenshot 2023-06-11 at 20.22.28.png" alt=""><figcaption><p>Beta Diversity Ordination at Time Point '2' with Gender Stratification (BC distance): This graph presents a detailed analysis of microbiome variability, taking into account both the specific time point '2' and the sex of subjects, thus allowing for a more nuanced understanding of our dataset.</p></figcaption></figure>

Remember, the beauty of MicrobiomeStat lies in its flexibility. Each of these steps represents a different level of complexity in your analysis, and you can customize them to meet your needs.&#x20;
