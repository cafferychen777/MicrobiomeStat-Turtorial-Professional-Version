---
description: >-
  A look at techniques for differential abundance analysis, illuminating
  significant changes in microbial abundance between paired conditions.
---

# Inspecting Paired Samples: Taxa Analysis

Welcome to **Inspecting Paired Samples: Taxa Analysis**. Here, we delve into the minutiae of **microbial ecosystems**, with our sights set on **taxa variations**. **MicrobiomeStat** presents us with powerful tools for **differential abundance analysis**, letting us illuminate significant **changes in microbial abundance** between paired conditions.

It's all about deciphering patterns in **taxa composition, prevalence, abundance, and changes**, and identifying samples with similar trends of taxa variation. Prepare to **uncover the hidden narratives** within your microbial data and reveal the complexity of these fascinating **microbial communities**!

Before visual exploration, we can perform statistical testing to identify differentially abundant taxa using `generate_taxa_test_pair()`:

```r
generate_taxa_test_pair(
  data.obj = peerj32.obj,
  subject.var = "subject",
  time.var = "time",
  group.var = "group",
  adj.vars = c("sex"),
  feature.level = "Family",
  prev.filter = 0,
  abund.filter = 0,
  feature.dat.type = "count"
)
```

This fits linear mixed models to assess taxa abundance changes over time across groups. The output contains data frames with log fold changes, p-values and other statistics for each taxonomic level.

#### Differential abundance results at Family level

| Variable       | Group   | Base.Mean    | Log2.Fold.Change | LFC.SE    | Stat        | P.Value    | Adjusted.P.Value | Mean.Abundance | Mean.Prevalence | Output.Element |
| -------------- | ------- | ------------ | ---------------- | --------- | ----------- | ---------- | ---------------- | -------------- | --------------- | -------------- |
| Actinobacteria | Placebo | 26787.28072  | 0.588211903      | 0.3408940 | 1.72549818  | 0.10066563 | 0.5536610        | 4.216254e-02   | 1               | sexmale        |
| Actinobacteria | LGG     | 26787.28072  | 0.588211903      | 0.3408940 | 1.72549818  | 0.10066563 | 0.5536610        | 3.456194e-02   | 1               | sexmale        |
| Asteroleplasma | Placebo | 26.72426     | -0.008993952     | 0.1867924 | -0.04814946 | 0.96209966 | 0.9731425        | 3.085257e-05   | 1               | sexmale        |
| Asteroleplasma | LGG     | 26.72426     | -0.008993952     | 0.1867924 | -0.04814946 | 0.96209966 | 0.9731425        | 2.355158e-05   | 1               | sexmale        |
| Bacilli        | Placebo | 42586.38242  | -0.164408489     | 0.3289424 | -0.49980943 | 0.62294818 | 0.9160217        | 3.842440e-02   | 1               | sexmale        |
| Bacilli        | LGG     | 42586.38242  | -0.164408489     | 0.3289424 | -0.49980943 | 0.62294818 | 0.9160217        | 4.550907e-02   | 1               | sexmale        |
| Bacteroidetes  | Placebo | 202133.22205 | -0.140524866     | 0.2804474 | -0.50107380 | 0.61906560 | 0.9160217        | 1.946338e-01   | 1               | sexmale        |
| Bacteroidetes  | LGG     | 202133.22205 | -0.140524866     | 0.2804474 | -0.50107380 | 0.61906560 | 0.9160217        | 1.751649e-01   | 1               | sexmale        |

Alternatively, we can assess taxa changes between timepoints using `generate_taxa_change_test_pair()`:

```r
generate_taxa_change_test_pair(
  data.obj = peerj32.obj,
  subject.var = "subject",
  time.var = "time",
  group.var = "group",
  adj.vars = c("sex"),
  change.base = "1",
  feature.change.func = "lfc",
  feature.level = "original",
  prev.filter = 0.01,
  abund.filter = 0.01,
  feature.dat.type = "count"
)
```

This computes fold changes in taxa abundance between timepoints. It models the changes using linear mixed models to test for group effects.

| Variable                         | Group   | R.Squared   | F.Statistic | Estimate            | P.Value | Adjusted.P.Value | Mean.Abundance\_Change | Mean.Prevalence\_Change | SD.Abundance\_Change | SD.Prevalence\_Change |
| -------------------------------- | ------- | ----------- | ----------- | ------------------- | ------- | ---------------- | ---------------------- | ----------------------- | -------------------- | --------------------- |
| Akkermansia                      | Placebo | 0.224813850 | 5.75929207  | -0.786560304739555  | 0.05    | 0.3198653        | -0.37419286            | 0.3571429               | 0.9291046            | 0.4879500             |
| Akkermansia                      | LGG     | 0.224813850 | 5.75929207  | -0.786560304739555  | 0.05    | 0.3198653        | 0.43559488             | 0.7500000               | 0.5547334            | 0.4472136             |
| Anaerostipes caccae et rel.      | Placebo | 0.013836924 | 0.30074853  | 0.156445859566378   | 0.64    | 0.8195592        | 0.01211655             | 0.2857143               | 0.5373659            | 0.4600437             |
| Anaerostipes caccae et rel.      | LGG     | 0.013836924 | 0.30074853  | 0.156445859566378   | 0.64    | 0.8195592        | -0.16805018            | 0.2500000               | 0.5034656            | 0.4472136             |
| Bacteroides intestinalis et rel. | Placebo | 0.001066535 | 0.02111832  | -0.0630644885178442 | 0.88    | 0.9198885        | 0.01827511             | 0.5714286               | 1.1320547            | 0.5039526             |
| Bacteroides intestinalis et rel. | LGG     | 0.001066535 | 0.02111832  | -0.0630644885178442 | 0.88    | 0.9198885        | 0.10736426             | 0.6250000               | 1.0647242            | 0.5000000             |
| Bacteroides uniformis et rel.    | Placebo | 0.003758428 | 0.07334084  | -0.114486918852422  | 0.74    | 0.8538961        | -0.23956082            | 0.5000000               | 1.0205461            | 0.5091751             |
| Bacteroides uniformis et rel.    | LGG     | 0.003758428 | 0.07334084  | -0.114486918852422  | 0.74    | 0.8538961        | -0.11328446            | 0.5000000               | 1.0481861            | 0.5163978             |

After diving into the Differential Abundance Analysis, let's shed some light on two other powerful functions in our analytical toolbox: `generate_taxa_indiv_boxplot_long()` and `generate_taxa_boxplot_long()`. These functions provide an in-depth visualization analysis for individual taxa.

Let's start with `generate_taxa_indiv_boxplot_long()`:

```r
generate_taxa_indiv_boxplot_long(
   data.obj = peerj32.obj,
   subject.var = "subject",
   time.var = "time",
   t0.level = "1",
   ts.levels = "2",
   group.var = "group",
   strata.var = NULL,
   feature.level = c("Family"),
   features.plot = NULL,
   feature.dat.type = "other",
   top.k.plot = NULL,
   top.k.func = NULL,
   Transform = "log",
   prev.filter = 0.1,
   abund.filter = 0.1,
   base.size = 20,
   theme.choice = "classic",
   custom.theme = NULL,
   palette = NULL,
   pdf = TRUE,
   file.ann = NULL,
   pdf.wid = 11,
   pdf.hei = 8.5
)
```

<figure><img src="../.gitbook/assets/Screenshot 2023-06-13 at 10.29.38.png" alt=""><figcaption><p><strong>Longitudinal Boxplot of Individual Taxa in Paired Samples</strong>: Using the <code>generate_taxa_indiv_boxplot_long()</code> function, we delve into each taxon's variation in abundance between paired samples over time. Each page of the multi-page PDF showcases one taxon, giving a deep, focused insight into the dynamic changes in paired conditions.</p></figcaption></figure>

This function creates a series of boxplots, one for each taxon, and outputs them into a multi-page PDF. Each page gives you a focused look at a single taxon, helping you digest complex data in bite-sized pieces.

Next, we have `generate_taxa_boxplot_long()`:

```r
generate_taxa_boxplot_long(
   data.obj = peerj32.obj,
   subject.var = "subject",
   time.var = "time",
   t0.level = "1",
   ts.levels = "2",
   group.var = "group",
   strata.var = NULL,
   feature.level = c("Family"),
   feature.dat.type = "count",
   features.plot = NULL,
   top.k.plot = NULL,
   top.k.func = NULL,
   Transform = "log",
   prev.filter = 0.1,
   abund.filter = 0.1,
   base.size = 16,
   theme.choice = "classic",
   custom.theme = NULL,
   palette = NULL,
   pdf = TRUE,
   file.ann = NULL,
   pdf.wid = 11,
   pdf.hei = 8.5
)
```

<figure><img src="../.gitbook/assets/Screenshot 2023-06-13 at 10.32.08.png" alt=""><figcaption><p><strong>Comprehensive Boxplot of Individual Taxa in Paired Samples</strong>: The <code>generate_taxa_boxplot_long()</code> function creates a combined boxplot of all taxa on one page, allowing for an immediate comparison of abundance variations across paired samples over time. This view is especially beneficial for highlighting taxa that show significant differential abundance, presenting a clear snapshot of the microbiome dynamics in paired samples.</p></figcaption></figure>

{% hint style="info" %}
🔍 **Hint:** When it comes to controlling the features you wish to visualize, the `features.plot` parameter is your best friend. If it's not NULL, **`abund.filter` and `prev.filter` will be disregarded during the visualization process**, although they will still be considered during computation. This means `features.plot` allows you to **filter the taxa after calculations and just before visualization**, displaying only the taxa listed in `features.plot`. This feature is particularly useful when you want to **visualize only those taxa that show significant changes in a Differential Abundance Analysis (DAA)**. It’s your bridge to effectively linking taxa analysis and DAA!&#x20;
{% endhint %}

This function places all taxa onto a single page, providing an overview of all your taxa at once. This can be especially useful for visualizing features with significant differential abundance (<0.05).

Moving forward, we introduce a couple of essential tools for our paired taxa analysis: `generate_taxa_indiv_change_boxplot_pair()` and `generate_taxa_change_boxplot_pair()`. This pair of functions offer a deep dive into changes in the taxonomic composition at the family level.

The first function, `generate_taxa_indiv_change_boxplot_pair()`, is applied as follows:

```R
generate_taxa_indiv_change_boxplot_pair(
   data.obj = peerj32.obj,
   subject.var = "subject",
   time.var = "time",
   group.var = "group",
   strata.var = NULL,
   change.base = "1",
   feature.change.func = "lfc",
   feature.level = c("Family"),
   features.plot = NULL,
   feature.dat.type = "count",
   top.k.plot = NULL,
   top.k.func = NULL,
   prev.filter = 0.1,
   abund.filter = 0.01,
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

<figure><img src="../.gitbook/assets/Screenshot 2023-06-13 at 10.54.42.png" alt=""><figcaption><p>Individual Taxa Change Boxplot Pair: Crafted by the <code>generate_taxa_indiv_change_boxplot_pair()</code> function, this plot visualizes the changes in taxonomic composition at the family level. The log-fold-change metric gives a clear view of the shifts in abundance over time, offering deeper insight into the dynamics between different conditions.</p></figcaption></figure>

Next, we move onto the second function `generate_taxa_change_boxplot_pair()`:

```R
generate_taxa_change_boxplot_pair(
   data.obj = peerj32.obj,
   subject.var = "subject",
   time.var = "time",
   group.var = "group",
   strata.var = NULL,
   change.base = "1",
   feature.change.func = "lfc",
   feature.level = c("Family"),
   feature.dat.type = "count",
   features.plot = NULL,
   top.k.plot = 6,
   top.k.func = "sd",
   prev.filter = 0.1,
   abund.filter = 0.01,
   base.size = 20,
   theme.choice = "bw",
   custom.theme = NULL,
   palette = NULL,
   pdf = TRUE,
   file.ann = "test",
   pdf.wid = 11,
   pdf.hei = 8.5
)
```

<figure><img src="../.gitbook/assets/Screenshot 2023-06-13 at 10.56.58.png" alt=""><figcaption><p>Individual Taxa Change Boxplot Pair with Standard Deviation: Created by the <code>generate_taxa_change_boxplot_pair()</code> function, this plot also illustrates changes in abundance within paired samples. However, the focus is shifted to the top six taxa based on their standard deviation, casting a new light on the microbial dynamics.</p></figcaption></figure>

Our next destination in this analytical journey is `generate_taxa_barplot_pair()`. This function unveils two insightful stacked bar plots, painting a vivid picture of taxonomic composition from both individual and group perspectives.

```r
generate_taxa_barplot_pair(
  data.obj = peerj32.obj,
  subject.var = "subject",
  time.var = "time",
  group.var = "group",
  strata.var = NULL,
  feature.level = "Family",
  feature.dat.type = c("count"),
  feature.number = 8,
  base.size = 10,
  theme.choice = "bw",
  custom.theme = NULL,
  palette = NULL,
  pdf = TRUE,
  file.ann = NULL,
  pdf.wid = 11,
  pdf.hei = 8.5
)
```

<figure><img src="../.gitbook/assets/Screenshot 2023-06-12 at 18.41.04.png" alt=""><figcaption><p>Individual Stacked Bar Plot of Taxa Composition: This visualization was created by the <code>generate_taxa_barplot_pair()</code> function, and it presents the taxonomic composition changes within each subject over time. Connecting lines across the stacked bars highlight the individual longitudinal microbiome journey.</p></figcaption></figure>

<figure><img src="../.gitbook/assets/Screenshot 2023-06-12 at 18.41.49.png" alt=""><figcaption><p>Average Group Stacked Bar Plot of Taxa Composition: Also crafted by the <code>generate_taxa_barplot_pair()</code> function, this plot displays the average taxonomic composition changes within groups over time. This representation sheds light on the broader microbial dynamics across different subject groups.</p></figcaption></figure>

{% hint style="info" %}
**Hint**: The `feature.number` parameter is a common feature in all **taxa\_barplot** functions within MicrobiomeStat. It sets a limit on the **number of taxa** displayed in the bar plot. If you set it to a specific number, let's say 9, then only the **9 most abundant taxa** are distinctly represented. All other taxa are collectively grouped into an '**Other**' category. This feature ensures clarity by focusing on the **most abundant taxa**, while still accounting for the collective presence of other, less abundant taxa.
{% endhint %}

{% hint style="info" %}
**Hint** : The `feature.level` parameter is highly flexible and plays a key role in refining your taxa analysis. It can be set to any column name in `feature.ann`. Moreover, if you want to perform analysis directly on **OTUs, ASVs, KEGG, Genes**, etc., you can simply set `feature.level` to '**original**'. This flexibility allows MicrobiomeStat to cater to your specific analytical needs, offering personalized insights into your microbiome data.
{% endhint %}

With its vibrant visuals, this function illuminates the shift in **taxa composition** for each subject across different time points, while simultaneously showcasing the average taxa composition change within groups. These **graphical representations provide a holistic understanding of the dynamic microbial landscape**, capturing the unique microbiome journey of each subject and revealing overarching patterns across groups. A true feast for the eyes and the mind!&#x20;

Delving further into the microbiome landscape, we turn our attention to the `generate_taxa_dotplot_pair()` function. This powerful tool offers a visual way to assess the disparities in **mean abundance** and **mean prevalence** across different groups.

When exploring paired samples, `generate_taxa_dotplot_pair()` displays dotplots of each feature's (like taxa at the Family level) mean abundance and mean prevalence. It provides a rich visual overview that aids in deciphering differences between microbial communities.

```r
generate_taxa_dotplot_pair(
  data.obj = peerj32.obj,
  subject.var = "subject",
  time.var = "time",
  group.var = "group",
  strata.var = NULL,
  feature.level = c("Family"),
  feature.dat.type = "count",
  features.plot = NULL,
  top.k.plot = NULL,
  top.k.func = NULL,
  prev.filter = 0.01,
  abund.filter = 0.01,
  base.size = 16,
  theme.choice = "bw",
  custom.theme = NULL,
  palette = NULL,
  pdf = TRUE,
  file.ann = NULL,
  pdf.wid = 45,
  pdf.hei = 8.5
)
```

<figure><img src="../.gitbook/assets/Screenshot 2023-06-12 at 20.34.39.png" alt=""><figcaption><p>Dotplot of Mean Abundance and Prevalence Between Paired Groups. Generated using the <code>generate_taxa_dotplot_pair()</code> function, this comprehensive dotplot showcases the variation in mean abundance (depicted by the size of the dots) and prevalence (represented by the color intensity) of each taxon at the Family level across different paired groups. This visualization allows for a nuanced understanding of significant changes in taxa abundance and prevalence across conditions, highlighting the intricate dynamics within our microbiome.</p></figcaption></figure>

In this journey, you have the ability to filter out low-abundance and low-prevalence features by adjusting the `prev.filter` and `abund.filter`. This enables a more focused and meaningful exploration of your microbiome data.

Moving on to `generate_taxa_change_dotplot_pair()`, we find a function perfectly tailored to **visualize microbiome fluctuations** within your dataset. By leveraging this function, you can track the **dynamic changes** in taxa abundance across different time points.

Let's see an example of how to use this function:

```r
generate_taxa_change_dotplot_pair(
  data.obj = peerj32.obj,
  subject.var = "subject",
  time.var = "time",
  group.var = "group",
  strata.var = NULL,
  change.base = "2",
  feature.change.func = "lfc",
  feature.level = "Family",
  feature.dat.type = "count",
  features.plot = NULL,
  top.k.plot = NULL,
  top.k.func = NULL,
  prev.filter = 0.001,
  abund.filter = 0.01,
  base.size = 16,
  theme.choice = "bw",
  custom.theme = NULL,
  pdf = TRUE,
  file.ann = NULL,
  pdf.wid = 11,
  pdf.hei = 8.5
)
```

<figure><img src="../.gitbook/assets/Screenshot 2023-06-12 at 21.06.46.png" alt=""><figcaption><p>Dotplot of Taxa Change Between Paired Groups: Crafted by the <code>generate_taxa_change_dotplot_pair()</code> function, this visualization underscores the dramatic shifts in taxa abundance at the Family level across different time points. The size of the dots corresponds to the change magnitude, dictated by log 2 fold change, while the color intensity signals the prevalence of each taxon. This insightful depiction accentuates the differing microbial dynamics between groups, inviting a deep dive into the intricate ballet within your microbiome.</p></figcaption></figure>

With `feature.change.func` set to "lfc" (log 2 fold change), you can **visually articulate** these microbiome shifts, unveiling the mysterious dance of your microbial communities over time.

However, the true power of this function isn't in tracking individual taxa changes, but in its capacity to **contrast these changes between different groups**. This capability allows you to dive deeper, uncovering the significant differences that propel your research forward towards more comprehensive conclusions.

Indeed, `generate_taxa_change_dotplot_pair()` serves as a potent tool in your MicrobiomeStat toolbox, focusing your analysis by filtering out the noise and shining a light on the most impactful shifts within your microbiome data.

Next on our **MicrobiomeStat** journey is the `generate_taxa_heatmap_pair()` function. This powerful tool allows us to craft an **illuminating heatmap** for an in-depth visualization of taxonomic shifts over time. Here's how we call it:

```r
generate_taxa_heatmap_pair(
   data.obj = peerj32.obj,
   subject.var = "subject",
   time.var = "time",
   group.var = "group",
   strata.var = NULL,
   feature.level = c("Family"),
   feature.dat.type = "count",
   features.plot = NULL,
   top.k.plot = NULL,
   top.k.func = NULL,
   prev.filter = 0.1,
   abund.filter = 0.1,
   base.size = 10,
   custom.theme = NULL,
   palette = NULL,
   pdf = TRUE,
   file.ann = NULL,
   pdf.wid = 11,
   pdf.hei = 8.5
)
```

<figure><img src="../.gitbook/assets/Screenshot 2023-06-12 at 21.20.20.png" alt=""><figcaption><p>Individual Taxa Heatmap: This detailed heatmap was created using the <code>generate_taxa_heatmap_pair()</code> function, offering a vibrant visual display of taxa changes at the Family level within individual samples over time. The color intensity indicates the relative abundance of each taxon, revealing shared abundance patterns among different taxa and providing insights into the intricate ecology of the microbiome.</p></figcaption></figure>

This function crafts a heatmap that not only lets us observe individual samples' taxa shifts at the **Family** level, but also helps us identify **taxa that share similar abundance patterns** across different conditions. Such insights can be instrumental in uncovering potential biological interactions and ecological behaviors within your microbiome data!

Diving deep into **patterns of change** is crucial for insightful microbiome analysis, and the **`generate_taxa_change_heatmap_pair()`** function from MicrobiomeStat is perfectly suited to assist in this quest.&#x20;

The function demonstrated here:

```
generate_taxa_change_heatmap_pair(
   data.obj = peerj32.obj,
   subject.var = "subject",
   time.var = "time",
   group.var = "group",
   strata.var = NULL,
   change.base = "1",
   feature.change.func = "relative difference",
   feature.level = "Family",
   feature.dat.type = "count",
   features.plot = NULL,
   top.k.plot = NULL,
   top.k.func = NULL,
   prev.filter = 0.1,
   abund.filter = 0.1,
   base.size = 10,
   palette = NULL,
   cluster.rows = NULL,
   cluster.cols = FALSE,
   pdf = TRUE,
   file.ann = NULL,
   pdf.wid = 11,
   pdf.hei = 8.5
)
```

<figure><img src="../.gitbook/assets/Screenshot 2023-06-12 at 21.33.57.png" alt=""><figcaption><p>Taxa Change Heatmap: Generated with the <code>generate_taxa_change_heatmap_pair()</code> function, this heatmap reveals the relative changes in taxa abundance at the Family level. The color intensity signifies the magnitude of change, providing an in-depth look at how microbial populations shift over time. Clustering reveals groups of taxa with similar abundance change patterns, shedding light on potential functional or ecological relationships.</p></figcaption></figure>

Generates a **meticulously detailed heatmap** that spotlights patterns of change in taxa abundance at the Family level. You are equipped to identify similar patterns among both **taxa and samples** based on relative changes in abundance.

In the function, 'relative change' is calculated using the formula `(change.after - change.before) / (change.before + change.after)`. This means the **intensity of color** in the heatmap is directly proportional to the **magnitude of change**. This **intuitive and visually engaging** method aids in understanding shifts in the microbial community over time.

By using this, you will journey into the depths of your data and **uncover the rich and intricate details** that shape your microbiome.
