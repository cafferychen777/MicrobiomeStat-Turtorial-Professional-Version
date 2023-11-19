---
description: >-
  MicrobiomeStat facilitates the exploration of beta diversity in
  cross-sectional microbiome studies by offering computational tools for
  distance calculation and dissimilarity visualization.
---

# Cross-Sectional Exploration: Beta Diversity Analysis with MicrobiomeStat

Beta diversity analysis is instrumental in revealing the difference in microbial community composition across samples.&#x20;

Before starting with pre-computed data objects, it's beneficial to grasp the utility of `dist.obj` and `pc.obj` in the context of this toolkit. By design, if these aren't provided, `MicrobiomeStat` auto-generates them. The `dist.obj` is computed using the `mStat_calculate_beta_diversity` function. When covariates (`adj.vars`) are identified, the `mStat_calculate_adjusted_distance` function refines the microbial community dissimilarities using a method based on linear models and multidimensional scaling. This process ensures the extracted microbial patterns are not confounded by the specified covariates, thus presenting a more accurate representation of the microbial community structures.

`pc.obj`, in its essence, is derived from the `mStat_calculate_PC` function. While users are free to select between "mds" and "nmds" as their ordination method, in scenarios where `pc.obj` isn't provided beforehand, the toolkit defaults to the "mds" method. Researchers favoring advanced ordination techniques like t-SNE or UMAP can employ external tools to compute results. Subsequently, these results can be reformatted to align with the `pc.obj` structure, facilitating effortless integration with `MicrobiomeStat`.

Further, it's worth noting that the range of distance measures (`dist.name`) supported includes "BC" (Bray-Curtis), "Jaccard", "UniFrac" (unweighted UniFrac), "GUniFrac" (generalized UniFrac), "WUniFrac" (weighted UniFrac), and "JS" (Jensen-Shannon divergence). It's pivotal to understand that some of these methods, especially those like "UniFrac", necessitate the presence of a phylogenetic tree. Thus, prior to leveraging these metrics, ensure that the `tree` component exists within the `data.obj`.

To rigorously test the association between the compositional variation and a covariate of interest while adjusting for other covariates, we can use `generate_beta_test_single`, which is based PERMANOVA on beta diversity measures (distance matrices).

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

In this example, we test the compositional association with the `group` variable, adjusting for the `sex` variable, at the time point `"2"`. We investigate both the "BC" and "Jaccard" beta diversity measures. The function will output the R2 (percent of compositional variation explained) and association p-value for each beta diversity measure. To be noted, the association statistics for `sex` in the table is not adjusted for the `group` effect.

#### Omnibus test results

| Term  | BC    | Jaccard | Omnibus |
| ----- | ----- | ------- | ------- |
| sex   | 0.791 | 0.819   | 0.808   |
| group | 0.794 | 0.859   | 0.817   |

It's also noteworthy that in practical scenarios, the nature of microbial changes isn't inherently known. Distinct distance measures excel at detecting specific scenarios. Leveraging multiple distance matrices and conducting separate tests for each can lead to a reduction in analytical power due to the necessity of multiple testing corrections. An integrated approach that combines the matrices in a singular test can enhance this power. PermanovaG achieves this by amalgamating multiple distance matrices, by taking the minimum of the P values from individual matrices as a test statistic and evaluating significance through permutation. Hence, this table also offers the omnibus test result, combining the association evidence provided by different beta diversity measures.

> Chen J, Bittinger K, Charlson ES, Hoffmann C, Lewis J, Wu GD, Collman RG, Bushman FD, Li H. Associating microbiome composition with environmental covariates using generalized UniFrac distances. Bioinformatics. 2012 Aug 15;28(16):2106-13. doi: 10.1093/bioinformatics/bts342. Epub 2012 Jun 17. PMID: 22711789; PMCID: PMC3413390.

#### PERMANOVA results

<table><thead><tr><th width="121">Distance</th><th>Variable</th><th width="60">DF</th><th>Sum.Sq</th><th width="82">Mean.Sq</th><th>F.Statistic</th><th>R.Squared</th><th>P.Value</th></tr></thead><tbody><tr><td>BC</td><td>sex</td><td>1</td><td>0.043</td><td>0.043</td><td>0.644</td><td>0.032</td><td>0.786</td></tr><tr><td>BC</td><td>group</td><td>1</td><td>0.042</td><td>0.042</td><td>0.621</td><td>0.031</td><td>0.782</td></tr><tr><td>BC</td><td>Residuals</td><td>19</td><td>1.28</td><td>0.067</td><td>NA</td><td>0.938</td><td>NA</td></tr><tr><td>BC</td><td>Total</td><td>21</td><td>1.36</td><td>NA</td><td>NA</td><td>1</td><td>NA</td></tr><tr><td>Jaccard</td><td>sex</td><td>1</td><td>0.099</td><td>0.099</td><td>0.707</td><td>0.035</td><td>0.827</td></tr><tr><td>Jaccard</td><td>group</td><td>1</td><td>0.094</td><td>0.094</td><td>0.668</td><td>0.033</td><td>0.827</td></tr><tr><td>Jaccard</td><td>Residuals</td><td>19</td><td>2.67</td><td>0.14</td><td>NA</td><td>0.933</td><td>NA</td></tr><tr><td>Jaccard</td><td>Total</td><td>21</td><td>2.86</td><td>NA</td><td>NA</td><td>1</td><td>NA</td></tr></tbody></table>

The table above provides the detailed outcomes of the PERMANOVA analysis, including metrics like the sum of squares, mean squares, and corresponding p-values.&#x20;

\[Please move before the statistical tests] We will first use beta diversity-based visualizations to study the relationship between the community-level composition and variables of interest. Later, we will perform more rigorous statistical tests. The `generate_beta_ordination_single` function provides the necessary visualizaiton tools.

```r
library(aplot)
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

<figure><img src="../.gitbook/assets/Screenshot 2023-10-10 at 19.54.22.png" alt=""><figcaption><p>Illustration of Beta Diversity Ordination for the entire dataset, offering a broad perspective on microbial community structures without temporal or strata distinctions.</p></figcaption></figure>

In the PCoA plot above, we plot all the observations regardless of time points and other stratifying variables. we can set `t.level` to visualize observations from a specific time point (`"2"` in this example).

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

<figure><img src="../.gitbook/assets/Screenshot 2023-10-10 at 19.55.26.png" alt=""><figcaption><p>Beta An in-depth visual representation of the Beta Diversity Ordination at Time Point '2'. </p></figcaption></figure>

We can also stratify on a specific variable, such as `sex`. And we also add a p-value annotation to the plot, derived from a PERMANOVA test, to highlight significant differences between groups:

```r
library(MicrobiomeStat)
data(peerj32.obj)
p <- generate_beta_ordination_single(
  data.obj = peerj32.obj,
  dist.obj = NULL,
  pc.obj = NULL,
  subject.var = "subject",
  time.var = "time",
  t.level = "2",
  group.var = "group",
  strata.var = "sex",
  adj.vars = "sex",
  dist.name = c("BC"),
  base.size = 20,
  theme.choice = "bw",
  custom.theme = NULL,
  palette = NULL,
  pdf = TRUE,
  file.ann = NULL,
  pdf.wid = 11,
  pdf.hei = 8.5
)

# Add a p-value annotation to the plot. The p-value is calculated from a PERMANOVA test.
p <- p + annotate("text", x = 0.3, y = 0.8, 
  label = paste("italic(p) == ", 
  format(pvalue, digits = 2)), 
  parse = TRUE, 
  size = 5)
```

<figure><img src="../.gitbook/assets/Screenshot 2023-10-16 at 18.48.19.png" alt=""><figcaption><p>Delving deeper, this illustration elucidates the Beta Diversity Ordination at Time Point '2', with a stratification based on gender. This overlay permits a more detailed inspection of microbial community variations across both time and gender spectra.</p></figcaption></figure>
