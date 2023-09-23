---
description: >-
  Automate integrated analysis reporting for paired microbiome studies with
  MicrobiomeStat.
---

# Automated Reporting for Paired Studies: MicrobiomeStat's Integrated Analysis Reports

Paired microbiome studies enable tracking temporal variations within subjects. But synthesis of multidimensional dynamics is challenging. **MicrobiomeStat** offers two automated solutions:

### Paired Sample Analysis Report

The `mStat_generate_report_pair()` provides integrated analysis:

* **Alpha diversity** indices using `generate_alpha_boxplot_long()` and `generate_alpha_test_pair()`
* **Beta diversity** ordination and PERMANOVA testing using `generate_beta_ordination_pair()`, `generate_beta_test_pair()`
* **Taxonomic composition** visualization using `generate_taxa_dotplot_pair()`, `generate_taxa_heatmap_pair()` and `generate_taxa_test_pair()`

It compiles results into a publication-ready report:

* Summary statistics
* Plots like boxplots, heatmaps
* Statistical test results in tables
* Interpretation of findings

We provide paired data, time variables and change settings:

```r
mStat_generate_report_pair(
  data.obj = peerj32.obj,
  dist.obj = NULL,
  alpha.obj = NULL,
  group.var = "group",
  adj.vars = c("sex"),
  subject.var = "subject",
  time.var = "time",
  alpha.name = c("shannon","simpson"),
  dist.name = c("BC",'Jaccard'),
  change.base = "1",
  change.func = "relative change",
  strata.var = "sex",
  feature.level = c("Phylum","Family"),
  feature.dat.type = "count",
  theme.choice = "bw",
  base.size = 12,
  output.file = "path/report.pdf"
)
```

{% file src="../.gitbook/assets/mStat_generate_report_pair_example.pdf" %}

The two functions enable automated paired sample analysis and reporting with MicrobiomeStat. Researchers can swiftly synthesize multidimensional dynamics through integrated reports.
