---
description: >-
  Automate integrated analysis reporting for paired microbiome studies with
  MicrobiomeStat.
---

# Paired Studies Reporting: Microbiome Analysis Automation with MicrobiomeStat

Paired microbiome studies enable tracking temporal variations within subjects. But synthesis of multidimensional dynamics is challenging. **MicrobiomeStat** offers two automated solutions:

- `mStat_generate_report_pair()`: Provides integrated analysis and generates a comprehensive report including alpha diversity, beta diversity, taxonomic composition, summary statistics, plots and results interpretation. It takes inputs like paired microbiome data, time variables, sample metadata etc.

- Modular functions like `generate_alpha_boxplot_long()`, `generate_taxa_heatmap_pair()` etc: Allow flexible and customized analysis by calling individual functions for specific tasks like visualization or hypothesis testing.

### Paired Sample Analysis Report

The `mStat_generate_report_pair()` provides integrated analysis:

- **Alpha diversity** indices using `generate_alpha_boxplot_long()` to visualize the alpha diversity distribution across subjects and timepoints, and `generate_alpha_test_pair()` to perform statistical testing for group differences in alpha diversity over time based on linear mixed effects models.

- **Beta diversity** ordination using `generate_beta_ordination_pair()` to visualize sample clustering and trajectories on a PCoA plot, as well as `generate_beta_test_pair()` for PERMANOVA hypothesis testing to detect group differences in beta diversity changes over time.

- **Taxonomic composition** visualization using `generate_taxa_dotplot_pair()` to show average proportions of top taxa across groups and timepoints, `generate_taxa_heatmap_pair()` for an overview of taxonomic profiles, and `generate_taxa_test_pair()` to identify differentially abundant taxa across groups over time using the LinDA framework.

It compiles results into a publication-ready report:

- Summary statistics like sample sizes, alpha diversity values, beta diversity distances etc. 

- Visualization plots such as boxplots for alpha diversity distribution, PCoA plots for beta diversity ordination, heatmaps and dotplots for taxonomic composition.

- Statistical test result tables from linear mixed effects models, ANOVA, PERMANOVA etc. showing p-values and significance.

- Interpretation of key findings in the form of text to summarize the observed patterns and statistical results. 

We provide paired data, time variables and change settings:

```r
 data(peerj32.obj)
 mStat_generate_report_pair(
   data.obj = peerj32.obj,
   group.var = "group",
   test.adj.vars = NULL,
   vis.adj.vars = NULL,
   strata.var = "sex",
   subject.var = "subject",
   time.var = "time",
   change.base = "1",
   alpha.obj = NULL,
   alpha.name = c("shannon", "observed_species"),
   dist.obj = NULL,
   dist.name = c("BC",'Jaccard'),
   feature.change.func = "relative change",
   vis.feature.level = c("Genus"),
   test.feature.level = c("Genus"),
   bar.area.feature.no = 30,
   heatmap.feature.no = 30,
   dotplot.feature.no = 20,
   feature.dat.type = "count",
   feature.mt.method = "none",
   feature.sig.level = 0.1,
   theme.choice = "bw",
   base.size = 18,
   output.file = "path/to/report.pdf"
 )
```

{% file src="../.gitbook/assets/mStat_generate_report_pair_example.pdf" %}
