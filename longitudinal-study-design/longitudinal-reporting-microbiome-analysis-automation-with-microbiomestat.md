---
description: >-
  Automate integrated analysis reporting for longitudinal microbiome studies
  with MicrobiomeStat.
---

# Longitudinal Reporting: Microbiome Analysis Automation with MicrobiomeStat

The `mStat_generate_report_long()` function in the MicrobiomeStat package is a comprehensive tool designed for automated analysis and reporting of longitudinal microbiome studies. This wiki page dives deep into the function's features, parameters, and how they integrate to provide a streamlined workflow.

The `mStat_generate_report_long()` function performs a comprehensive analysis encompassing:

* **Alpha diversity** analysis using `generate_alpha_boxplot_long()`, `generate_alpha_spaghettiplot_long()` and `generate_alpha_test_long()`
* **Beta diversity** analysis using `generate_beta_ordination_long()`, `generate_beta_pc_boxplot_long()`, `generate_beta_change_spaghettiplot_long()` and `generate_beta_test_long()`
* **Taxonomic composition** analysis using `generate_taxa_areaplot_long()`, `generate_taxa_heatmap_long()`, `generate_taxa_barplot_long()` , `generate_taxa_test_long()`, `generate_taxa_boxplot_long()`,  `generate_taxa_indiv_boxplot_long()`,  `generate_taxa_spaghettiplot_long()` and `generate_taxa_indiv_spaghettiplot_long()`

It then compiles the results into a publication-ready PDF report containing:

* Summary statistics
* Plots like boxplots, heatmaps
* Statistical test results in tables
* Interpretation of findings

We provide the longitudinal data and analysis parameters:

```r
 mStat_generate_report_long(
  data.obj = subset_T2D.obj,
  dist.obj = NULL, 
  alpha.obj = NULL,
  group.var = "sample_body_site",
  adj_vars = c("subject_race"),
  subject.var = "subject_id",
  time.var = "visit_number",
  alpha.name = c("shannon","simpson"),
  dist.name = c("BC",'Jaccard'),
  t0.level = unique(sort(subset_T2D.obj$meta.dat$visit_number))[1],
  ts.levels = unique(sort(subset_T2D.obj$meta.dat$visit_number))[-1],
  strata.var = "subject_race",
  feature.level = c("Phylum"),
  feature.dat.type = "count",
  prev.filter = 0.0001,
  abund.filter = 0.0001, 
  Transform = "log",
  theme.choice = "bw", 
  base.size = 12,
  output.file = "path/report.pdf" 
)
```

{% file src="../.gitbook/assets/mStat_generate_report_long_example.pdf" %}

### Benefits of Using `mStat_generate_report_long`

* **Comprehensive**: Conducts a wide array of analyses and compiles them into a single, easy-to-read report.
* **Customizable**: Numerous parameters for customizing the analysis as per the study's needs.
* **Automated Workflow**: Reduces manual errors and saves time.
