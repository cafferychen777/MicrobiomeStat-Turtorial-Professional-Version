# Beta Diversity Analysis

Beta diversity analysis is instrumental in revealing the difference in microbial community composition between samples, and attributing the differences to a specific covariate/covariates. For those beta diversity-based functions, they all have a `dist.obj` parameter, which accepts a list of  distance matrices (beta diversity is expressed in the form of pairwise distances). If the parameter is not specified, `mStat_calculate_beta_diversity` after rarefaction (`mStat_rarefy_data`) will be automatically called to calculate various distance matrices. MicrobiomeStats supports a variety of beta diversity measures, including "BC" (Bray-Curtis), "Jaccard", "UniFrac" (unweighted), "GUniFrac" (generalized), "WUniFrac" (weighted), and "JS" (Jensen-Shannon divergence). The users can also create a list of their own distance matrices of choice and pass it to `dist.obj`. We recommend the user to pre-calculate distance matrices once in the analysis, store it in a data object and then use it repeatedly.

In some applications (e.g. ordination), the users may want to remove the covariate effects from the distance matrices. This can be achieved by using `mStat_calculate_adjusted_distance` function based on the reference below.

> Shi, Y., Zhang, L., Do, K. A., Peterson, C. B., & Jenq, R. R. (2020). aPCoA: covariate adjusted principal coordinates analysis. Bioinformatics, 36(13), 4099-4101. PMID: 32339223 PMCID: PMC7332564.

This process ensures the extracted microbial patterns are not confounded by the specified covariates, thus presenting a more accurate representation of the microbial community structures.

Based on the distance matrices, we can also extract their first several principal coordinates (PCs), which capture the main variation in the data. PCs can be calculated by calling `mstat_calculate_PC` function. Users can select between "mds" and "nmds" as their ordination method (default is "mds" method). Users favoring advanced ordination techniques like t-SNE or UMAP can employ external tools to compute the PCs. For those PC-based functions, they all have a `pc.obj` parameter, which accepts a list of PC matrices based on a variety of beta diversity measures. If the parameter is not specified, `mStat_calculate_PC` will be called automatically.  We recommend the user to create the list once in the analysis and use it repeatedly.


We will first use beta diversity-based visualizations to explore the relationship between the microbial community and variables of interest. The `generate_beta_ordination_single` function performs this task.

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

<figure><img src="../.gitbook/assets/Screenshot 2023-10-10 at 19.54.22.png" alt=""><figcaption></figcaption></figure>

In the example above, we did not specifiy the specific time point, though the function has the flexibility to visualize a specific time point by using `time.var` and `t.level`.  After the visualization, we can conduct more rigorous statistical testing and calculate p-values. To rigorously test the association between the microbial community and a covariate of interest while adjusting for other covariates, we can use `generate_beta_test_single`, which is based on PERMANOVA using distance matrices.

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

In this example, we test the microbiome association with the "group" variable, adjusting for the "sex" variable, at the time point "2". We use both the "BC" and "Jaccard" distance measures. The function will output the R2 (percent  variation explained) and association p-values for each beta diversity measure. To be noted, the association statistics for "sex" in the table is not adjusted for the "group" effect.

#### PERMANOVA results

<table><thead><tr><th width="121">Distance</th><th>Variable</th><th width="60">DF</th><th>Sum.Sq</th><th width="82">Mean.Sq</th><th>F.Statistic</th><th>R.Squared</th><th>P.Value</th></tr></thead><tbody><tr><td>BC</td><td>sex</td><td>1</td><td>0.043</td><td>0.043</td><td>0.644</td><td>0.032</td><td>0.786</td></tr><tr><td>BC</td><td>group</td><td>1</td><td>0.042</td><td>0.042</td><td>0.621</td><td>0.031</td><td>0.782</td></tr><tr><td>BC</td><td>Residuals</td><td>19</td><td>1.28</td><td>0.067</td><td>NA</td><td>0.938</td><td>NA</td></tr><tr><td>BC</td><td>Total</td><td>21</td><td>1.36</td><td>NA</td><td>NA</td><td>1</td><td>NA</td></tr><tr><td>Jaccard</td><td>sex</td><td>1</td><td>0.099</td><td>0.099</td><td>0.707</td><td>0.035</td><td>0.827</td></tr><tr><td>Jaccard</td><td>group</td><td>1</td><td>0.094</td><td>0.094</td><td>0.668</td><td>0.033</td><td>0.827</td></tr><tr><td>Jaccard</td><td>Residuals</td><td>19</td><td>2.67</td><td>0.14</td><td>NA</td><td>0.933</td><td>NA</td></tr><tr><td>Jaccard</td><td>Total</td><td>21</td><td>2.86</td><td>NA</td><td>NA</td><td>1</td><td>NA</td></tr></tbody></table>

The table above provides the detailed outcome of the PERMANOVA analysis, including metrics such as sum of squares, mean squares, and p-values.

#### Omnibus test results

| Term  | BC    | Jaccard | Omnibus |
| ----- | ----- | ------- | ------- |
| sex   | 0.791 | 0.819   | 0.808   |
| group | 0.794 | 0.859   | 0.817   |

It is also noteworthy that in practical scenarios, the nature of microbiome change is usually unknown before the analysis. As each distance measure excels at detecting a specific type of change, combining several representative distance measures may improve statistical power.  Howver, conducting separate tests for each distance measure requires multiple testing correction and the simple Bonferroni correction may be too stringent due to the correlations among the distance measures. An omnibus test (reference below), which takes the minimum of the p-values as a new test statistic and uses permutation to assess significance, is usually more powerful than Bonferroni correction.  In the result, we thus also provide the omnibus test p-value by combining the association evidence provided by different distance measures.

> Chen J, Bittinger K, Charlson ES, Hoffmann C, Lewis J, Wu GD, Collman RG, Bushman FD, Li H. Associating microbiome composition with environmental covariates using generalized UniFrac distances. Bioinformatics. 2012 Aug 15;28(16):2106-13. doi: 10.1093/bioinformatics/bts342. Epub 2012 Jun 17. PMID: 22711789; PMCID: PMC3413390.

In the PCoA plot, we can also stratify on a specific variable, such as "sex", and add the PERMANOVA p-value to highlight the statistical significance:

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

<figure><img src="../.gitbook/assets/Screenshot 2023-10-16 at 18.48.19.png" alt=""><figcaption></figcaption></figure>
