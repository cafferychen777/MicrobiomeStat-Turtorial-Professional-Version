---
description: >-
  MicrobiomeStat offers tools for analyzing beta diversity in cross-sectional microbiome studies, including computing distances and visualizing dissimilarities.
---

# Cross-Sectional Exploration: Beta Diversity Analysis with MicrobiomeStat

**Simple Cross-Sectional Design**: With MicrobiomeStat, we can launch our exploration without any pre-calculations of `dist.obj` and `pc.obj`. They will be automatically computed based on `dist.name` in action. 

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

#### Omnibus test results

| Term  | BC    | Jaccard | Omnibus |
| ----- | ----- | ------- | ------- |
| sex   | 0.791 | 0.819   | 0.808   |
| group | 0.794 | 0.859   | 0.817   |

This table summarizes the p-values from PERMANOVA tests for each distance matrix and the omnibus test across all distances.

#### PERMANOVA results

| Distance | Variable   | DF | Sum.Sq | Mean.Sq | F.Statistic | R.Squared | P.Value |
|----------|------------|----|--------|---------|-------------|-----------|---------|
| BC       | sex        | 1  | 0.043  | 0.043   | 0.644       | 0.032     | 0.786   |
| BC       | group      | 1  | 0.042  | 0.042   | 0.621       | 0.031     | 0.782   |
| BC       | Residuals  | 19 | 1.28   | 0.067   | NA          | 0.938     | NA      |
| BC       | Total      | 21 | 1.36   | NA      | NA          | 1         | NA      |
| Jaccard  | sex        | 1  | 0.099  | 0.099   | 0.707       | 0.035     | 0.827   |
| Jaccard  | group      | 1  | 0.094  | 0.094   | 0.668       | 0.033     | 0.827   |
| Jaccard  | Residuals  | 19 | 2.67   | 0.14    | NA          | 0.933     | NA      |
| Jaccard  | Total      | 21 | 2.86   | NA      | NA          | 1         | NA      |


The above table outlines the PERMANOVA analysis outcomes for various beta diversity distances, offering details such as degrees of freedom (DF), sum of squares, mean squares, F-statistics, R-squared values, and corresponding p-values. The results enable an assessment of significant differences in microbial community structures across specified groups.

Next, we visualize the beta diversity using `generate_beta_ordination_single`:

```r
generate_beta_ordination_single(
  data.obj = peerj32.obj,
  dist.obj = NULL,
  pc.obj = NULL,
  subject.var = "subject",
  time.var = NULL, # Variable representing time points
  t.level = NULL, # Specific time level for subset (if subset desired)
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

```r
 generate_beta_ordination_single(
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
  custom.theme = NULL,
  palette = NULL,
  pdf = TRUE,
  file.ann = NULL,
  pdf.wid = 11,
  pdf.hei = 8.5
)
```

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
