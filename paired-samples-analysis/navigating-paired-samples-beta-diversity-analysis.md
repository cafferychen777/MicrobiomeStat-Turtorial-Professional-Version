---
description: >-
  A deep-dive into beta diversity metrics and ordination methods for paired
  samples, elucidating differences in microbial community structure.
---

# Navigating Paired Samples: Beta Diversity Analysis

Welcome to **Navigating Paired Samples: Beta Diversity Analysis**. Here, we're **exploring the labyrinth of beta diversity**, specifically with an eye on **paired samples**. Prepare to dive deep into the variations in the **microbial community structure** across different groups and within individual subjects over time. Get ready to uncover **dynamic changes**, decipher patterns, and extract **meaningful insights** from your microbiome data. So strap in and get ready for a captivating expedition across the intricate microbial terrain!

During our journey, **MicrobiomeStat's commitment to efficiency** will be our guiding light. All beta-related functions share a similar streamlined approach. If `dist.obj` or `pc.obj` aren't supplied, these functions will intuitively invoke `mStat_calculate_beta_diversity()` and `mStat_calculate_PC()`, respectively, **ensuring a seamless and efficient analysis experience**.

So, fasten your seatbelts as we prepare to **uncover dynamic changes**, **decipher intricate patterns**, and **extract meaningful insights from your microbiome data**. Get ready for an exciting journey across the diverse microbial terrain!

Before visualizing, it's essential to **statistically test** beta diversity differences.

`generate_beta_change_test_pair()` fits **linear models** relating beta diversity measures to grouping and covariate variables.

```r
generate_beta_change_test_pair(
  data.obj = peerj32.obj,
  dist.obj = NULL,
  time.var = "time",
  subject.var = "subject",
  group.var = "group",
  adj.vars = c("sex"),
  change.base = "1",
  dist.name = c('BC', 'Jaccard') 
)
```

It returns **coefficient tables** with **p-values** for assessing variable associations with each beta diversity metric.

| Distance | Term         | Estimate | Std.Error | Statistic | P.Value  |
| -------- | ------------ | -------- | --------- | --------- | -------- |
| BC       | (Intercept)  | 0.175    | 0.0186    | 9.42      | 8.24e-12 |
| BC       | sexmale      | 0.0671   | 0.0217    | 3.09      | 3.58e-3  |
| BC       | groupPlacebo | -0.0157  | 0.0210    | -0.746    | 4.60e-1  |

| Distance | Term         | Estimate | Std.Error | Statistic | P.Value  |
| -------- | ------------ | -------- | --------- | --------- | -------- |
| Jaccard  | (Intercept)  | 0.294    | 0.0246    | 12.0      | 6.04e-15 |
| Jaccard  | sexmale      | 0.0853   | 0.0288    | 2.96      | 5.06e-3  |
| Jaccard  | groupPlacebo | -0.0207  | 0.0279    | -0.744    | 4.61e-1  |

Alternatively, `generate_beta_test_pair()` uses **PERMANOVA** to test beta diversity differences.

```r
generate_beta_test_pair(
  data.obj = peerj32.obj,
  dist.obj = NULL,
  time.var = "time",
  subject.var = "subject",
  group.var = "group",
  adj.vars = c("sex"),
  dist.name = c('BC', 'Jaccard')
)
```

It provides **test statistics** and **p-values** from PERMANOVA models.

| Term  | D1.p.value | D2.p.value | omni.p.value |
| ----- | ---------- | ---------- | ------------ |
| sex   | 0.427      | 0.45       | 0.45         |
| group | 0.427      | 0.45       | 0.45         |
| time  | 0.427      | 0.45       | 0.45         |

| Variable  | DF | Sum\_Sq | Mean\_Sq | F\_Statistic | R\_Squared | P\_Value | Distance |
| --------- | -- | ------- | -------- | ------------ | ---------- | -------- | -------- |
| sex       | 1  | 0.123   | 0.123    | 1.991        | 0.046      | 0.427    | BC       |
| group     | 1  | 0.068   | 0.068    | 1.091        | 0.025      | 0.427    | BC       |
| time      | 1  | 0.019   | 0.019    | 0.311        | 0.007      | 0.427    | BC       |
| Residuals | 40 | 2.48    | 0.062    | NA           | 0.922      | NA       | BC       |
| Total     | 43 | 2.69    | NA       | NA           | 1          | NA       | BC       |
| sex       | 1  | 0.228   | 0.228    | 1.737        | 0.04       | 0.45     | Jaccard  |
| group     | 1  | 0.151   | 0.151    | 1.151        | 0.027      | 0.45     | Jaccard  |
| time      | 1  | 0.05    | 0.05     | 0.38         | 0.009      | 0.45     | Jaccard  |
| Residuals | 40 | 5.26    | 0.131    | NA           | 0.924      | NA       | Jaccard  |
| Total     | 43 | 5.69    | NA       | NA           | 1          | NA       | Jaccard  |

Together these enable rigorous hypothesis testing before visualizing beta diversity changes in paired designs.

Next, we'll delve into the magic of the `generate_beta_ordination_pair()` function, a powerful tool that operates in the background to streamline your analysis.

```r
generate_beta_ordination_pair(
  data.obj = peerj32.obj,
  dist.obj = NULL,
  pc.obj = NULL,
  subject.var = "subject",
  time.var = "time",
  group.var = "group",
  strata.var = NULL,
  dist.name = c("BC"),
  base.size = 16,
  theme.choice = "bw",
  custom.theme = NULL,
  palette = NULL,
  pdf = TRUE,
  file.ann = NULL,
  pdf.wid = 11,
  pdf.hei = 8.5
)
```

<figure><img src="../.gitbook/assets/Screenshot 2023-06-12 at 15.32.21.png" alt=""><figcaption><p>Resulting from <code>generate_beta_ordination_pair()</code>, this beta diversity ordination plot maps out the intricate relationships between samples based on their microbial community structures. Look at how elegantly the arrows connect the same subject across different time points, presenting a dynamic journey of their microbiome composition over time.</p></figcaption></figure>

With the foundation established, let's transition to how `generate_beta_change_boxplot_pair()` augments our beta diversity analysis. This function quantifies the **beta diversity changes** within individual subjects over time, employing metrics like **Bray-Curtis dissimilarity** to illuminate temporal shifts within each microbiome.

Here's a glimpse of how we'd utilize this function in our code:

```r
generate_beta_change_boxplot_pair(
  data.obj = peerj32.obj,
  dist.obj = NULL,
  subject.var = "subject",
  time.var = "time",
  group.var = "group",
  strata.var = NULL,
  change.base = "1",
  dist.name = c('BC'),
  base.size = 16,
  theme.choice = "bw",
  custom.theme = NULL,
  palette = NULL,
  pdf = TRUE,
  file.ann = NULL,
  pdf.wid = 11,
  pdf.hei = 8.5
)
```

<figure><img src="../.gitbook/assets/Screenshot 2023-06-12 at 15.53.46.png" alt=""><figcaption><p>Courtesy of <code>generate_beta_change_boxplot_pair()</code>, this insightful plot unravels the intricate shifts in beta diversity within subjects over time. The neatly arranged boxplots symbolize different time points, while the horizontal lines represent the beta diversity changes of the same subject. Together, they illustrate a captivating narrative of microbiome dynamics.</p></figcaption></figure>

{% hint style="info" %}
**Hint**: The Bray-Curtis (BC) distance we're using is a non-metric measure commonly used in ecological studies to evaluate community resemblance between two samples. The score ranges from 0 to 1. A score of 0 implies identical communities, while 1 represents entirely distinct ones. Keep in mind, the interpretation of "high" or "low" similarity depends on your study's unique context and objectives!
{% endhint %}

Having explored the temporal shifts within each microbiome using `generate_beta_change_boxplot_pair()`, our next waypoint is `generate_beta_pc_change_boxplot_pair()`. This function further enhances our beta diversity analysis by elucidating the **differences in specific ordination Axes** between different groups. It's an additional lens that offers a more granular perspective of our microbial data.

```r
generate_beta_pc_change_boxplot_pair(
  data.obj = peerj32.obj,
  dist.obj = NULL,
  pc.obj = NULL,
  pc.ind = c(1, 2),
  subject.var = "subject",
  time.var = "time",
  group.var = "group",
  strata.var = NULL,
  change.base = "1",
  change.func = "difference",
  dist.name = c('BC'),
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

<figure><img src="../.gitbook/assets/Screenshot 2023-06-12 at 16.11.17.png" alt=""><figcaption><p>Produced by <code>generate_beta_pc_change_boxplot_pair()</code>, this boxplot unveils the differences in specific ordination Axes between different groups. It provides a granular look into the shifting sands of our microbial data, offering valuable insights into community shifts and their biological implications.</p></figcaption></figure>

With this robust tool in hand, we're all set to delve deeper and uncover the intricate shifts within our microbial communities!

Continuing our journey, let's spotlight `generate_beta_pc_boxplot_long()`. This function **connects the dots of our analysis**, tracing the trajectory of each subject's beta diversity across two time points on specific ordination axes.

```r
generate_beta_pc_boxplot_long(
  data.obj = peerj32.obj,
  dist.obj = NULL,
  pc.obj = NULL,
  subject.var = "subject",
  time.var = "time",
  t0.level = "1",
  ts.levels = "2",
  group.var = "group",
  strata.var = NULL,
  dist.name = c('BC'),
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

<figure><img src="../.gitbook/assets/Screenshot 2023-06-12 at 16.21.31.png" alt=""><figcaption><p>Emerging from <code>generate_beta_pc_boxplot_long()</code>, this compelling plot maps the dynamic journey of each subject's microbiome across two distinct time points. Each line elegantly depicts the evolution of beta diversity within an individual subject along specified ordination axes, providing a clear visualization of their microbial narrative.</p></figcaption></figure>

In essence, it's **unveiling the dynamic storyline** of each individual subject's microbiome over time. So let's immerse ourselves in the detailed narrative of these intriguing shifts!
