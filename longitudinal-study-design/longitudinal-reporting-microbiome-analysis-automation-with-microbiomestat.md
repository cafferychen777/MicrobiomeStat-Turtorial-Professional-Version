---
description: >-
  Automate integrated analysis reporting for longitudinal microbiome studies
  with MicrobiomeStat.
---

# Longitudinal Reporting: Microbiome Analysis Automation with MicrobiomeStat

Longitudinal microbiome studies reveal complex temporal dynamics. Manual synthesis is challenging. **MicrobiomeStat** offers an automated solution through integrated analysis reports.

The `mStat_generate_report_long()` function performs a comprehensive analysis encompassing:

* **Alpha diversity** analysis using `generate_alpha_boxplot_long()`, `generate_alpha_spaghettiplot_long()` and `generate_alpha_test_long()`
* **Beta diversity** analysis using `generate_beta_ordination_long()`, `generate_beta_pc_boxplot_long()`, `generate_beta_change_spaghettiplot_long()` and `generate_beta_test_long()`
* **Taxonomic composition** analysis using `generate_taxa_areaplot_long()`, `generate_taxa_heatmap_long()`, `generate_taxa_barplot_long()` , `generate_taxa_test_long()`, `generate_taxa_boxplot_long()` and `generate_taxa_indiv_boxplot_long()`

It then compiles the results into a publication-ready PDF report containing:

* Summary statistics
* Plots like boxplots, heatmaps
* Statistical test results in tables
* Interpretation of findings

We provide the longitudinal data and analysis parameters:

```r
mStat_generate_report_long(
  data.obj = long_data, 
  time.var = "week",
  output.file = "report.pdf"
) 
```

This automated workflow streamlines analysis and reporting for longitudinal studies. The integrated report synthesizes temporal dynamics across analytical dimensions.

MicrobiomeStat powers longitudinal discoveries through automated, standardized data reports.
