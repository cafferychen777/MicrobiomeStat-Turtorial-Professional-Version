---
description: >-
  Explore alpha diversity analysis in cross-sectional studies using
  MicrobiomeStat. Learn to interpret these biodiversity measures for valuable
  microbiome dataset insights.
---

# Cross-Sectional Snapshot: Alpha Diversity Analysis with MicrobiomeStat

With **MicrobiomeStat**, we have the capability to delve into the richness and evenness of our microbiome dataset via **alpha diversity indices**. These indices paint a picture of the species complexity within individual samples. For our `peerj32` dataset, we'll start with a broad view, ignoring time and strata variables by setting both `time.var` and `t.level` as `NULL`.

Initially, we create an alpha diversity boxplot encompassing all data, without any time point distinction:

```r
# ... previous code ...

# Generate alpha diversity boxplot (simpson index) for all data
plot_alpha_list <- generate_alpha_boxplot_single(
  data.obj = peerj32.obj,
  alpha.obj = NULL,
  alpha.name = c("simpson"),
  subject.var = "subject",
  time.var = NULL,
  t.level = NULL,
  group.var = "group",
  strata.var = NULL,
  base.size = 16,
  theme.choice = "classic",
  palette = NULL,
  pdf = TRUE,
  file.ann = NULL,
  pdf.wid = 11,
  pdf.hei = 8.5)
```

<figure><img src="../.gitbook/assets/Screenshot 2023-06-11 at 19.45.33.png" alt=""><figcaption><p>Boxplot of the Simpson alpha diversity index across all samples, disregarding the time and strata variables. The boxplot provides an overview of the species complexity within the samples from the two groups: Probiotic LGG and Placebo.</p></figcaption></figure>

Next, we focus our lens on a specific time point, `t.level = "2"`, to assess if there are significant alpha diversity differences:

```r
# Generate alpha diversity boxplot (simpson index) for specific time point
plot_alpha_list <- generate_alpha_boxplot_single(
  data.obj = peerj32.obj,
  alpha.obj = NULL,
  alpha.name = c("simpson"),
  subject.var = "subject",
  time.var = "time",
  t.level = "2",
  group.var = "group",
  strata.var = NULL,
  base.size = 16,
  theme.choice = "classic",
  palette = NULL,
  pdf = TRUE,
  file.ann = NULL,
  pdf.wid = 11,
  pdf.hei = 8.5)
```

<figure><img src="../.gitbook/assets/Screenshot 2023-06-11 at 19.46.41.png" alt=""><figcaption><p>Boxplot of the Simpson alpha diversity index for samples at the specific time point '2'. The plot shows the species complexity for each group at this particular time point, offering insights into the potential impact of the probiotic intervention.</p></figcaption></figure>

Finally, we incorporate the strata variable `sex` into our analysis, adding another layer of complexity:

```r
# Generate alpha diversity boxplot (simpson index) considering strata
plot_alpha_list <- generate_alpha_boxplot_single(
  data.obj = peerj32.obj,
  alpha.obj = NULL,
  alpha.name = c("simpson"),
  subject.var = "subject",
  time.var = "time",
  t.level = "2",
  group.var = "group",
  strata.var = "sex",
  base.size = 16,
  theme.choice = "classic",
  palette = NULL,
  pdf = TRUE,
  file.ann = NULL,
  pdf.wid = 11,
  pdf.hei = 8.5)
```

<figure><img src="../.gitbook/assets/Screenshot 2023-06-11 at 19.47.28.png" alt=""><figcaption><p>Boxplot of the Simpson alpha diversity index for samples at time point '2', considering the strata variable 'sex'. The plot provides an added layer of understanding by showcasing the species complexity stratified by sex and group. This helps to identify any sex-specific effects of the intervention on the microbiome's species complexity.</p></figcaption></figure>

The alpha diversity index in our examples is the Simpson index, but users have the flexibility to input a vector of indices to generate a **list of plots**. The supported indices include **"shannon"**, **"simpson"**, **"observed\_species"**, **"chao1"**, **"ace"**, and **"pielou"**. Each of these measures offers unique insights into the richness and evenness of species in your samples.
