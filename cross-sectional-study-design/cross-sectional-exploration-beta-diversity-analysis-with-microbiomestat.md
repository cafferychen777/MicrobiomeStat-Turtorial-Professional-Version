---
description: >-
  MicrobiomeStat facilitates the exploration of beta diversity in cross-sectional microbiome studies by offering computational tools for distance calculation and dissimilarity visualization.
---

# Beta Diversity Analysis in Cross-Sectional Studies with MicrobiomeStat

In cross-sectional studies, beta diversity acts as an instrumental measure to gauge differences in microbial community composition between samples. The `MicrobiomeStat` toolkit aids researchers in navigating these complex terrains.

When venturing into the dataset with a **Simple Cross-Sectional Design**, it's not obligatory to have pre-computed `dist.obj` and `pc.obj`. The suite, being intuitive, defaults to computing these based on the `dist.name` provided.

Before embarking on a visual interpretation of beta diversity through `generate_beta_ordination_single`, a quantitative assessment is essential. The `generate_beta_test_single` function fills this role by deploying PERMANOVA on the chosen distance matrices. The aim is to discern significant variances between groups while factoring in supplementary covariates.

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

It's noteworthy that in practical scenarios, the nature of microbial changes isn't inherently known. Distinct distance measures excel at detecting specific scenarios. Leveraging multiple distance matrices and conducting separate tests for each can lead to a reduction in analytical power due to the necessity of multiple testing corrections. An integrated approach that combines the matrices in a singular test can enhance this power. PermanovaG exemplifies this by amalgamating multiple distance matrices, primarily by considering the minimum of the P values from individual matrices and evaluating significance through permutation. Hence, this table not only aggregates the p-values from the PERMANOVA tests across distinct distance matrices but also offers an omnibus test result, furnishing a more comprehensive understanding of the microbial community structures across the designated groups.

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

The table above elucidates the detailed outcomes of the PERMANOVA analysis, encompassing metrics like the sum of squares, mean squares, and corresponding p-values. This granularity enables a more profound comprehension of the microbial community structures across the designated groups.

Transitioning from the quantitative to the qualitative, the `generate_beta_ordination_single` function provides the necessary visualization tools:

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

<figure><img src="../.gitbook/assets/Screenshot 2023-06-11 at 20.20.28.png" alt=""><figcaption><p>Illustration of Beta Diversity Ordination for the entire dataset, offering a broad perspective on microbial community structures without temporal or strata distinctions.</p></figcaption></figure>

Shifting our gaze to a specific temporal frame by setting `t.level` to "2", we probe into the beta diversity nuances of this time juncture:

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

<figure><img src="../.gitbook/assets/Screenshot 2023-06-11 at 20.21.25.png" alt=""><figcaption><p>Beta An in-depth visual representation of the Beta Diversity Ordination at Time Point '2'. This delineation unveils the microbial community structures' intricacies at this particular instance.</p></figcaption></figure>

Incorporating a stratifying dimension, such as `sex`, augments the depth of the analysis:

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

<figure><img src="../.gitbook/assets/Screenshot 2023-06-11 at 20.22.28.png" alt=""><figcaption><p>Delving deeper, this illustration elucidates the Beta Diversity Ordination at Time Point '2', with a stratification based on gender. This overlay permits a more detailed inspection of microbial community variations across both time and gender spectra.</p></figcaption></figure>

The robustness of MicrobiomeStat is evident in its adaptability, allowing researchers to orchestrate analyses that align with their study's granularity and requirements.&#x20;
