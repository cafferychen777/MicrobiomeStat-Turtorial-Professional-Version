---
description: >-
  Automate integrated analysis reporting for longitudinal microbiome studies
  with MicrobiomeStat.
---

# Longitudinal Reporting: Microbiome Analysis Automation with MicrobiomeStat

Longitudinal Reporting: Microbiome Analysis Automation with MicrobiomeStat

The `mStat_generate_report_long()` function in the MicrobiomeStat package is a comprehensive tool designed for automated analysis and reporting of longitudinal microbiome studies. This wiki page dives deep into the function's features, parameters, and how they integrate to provide a streamlined workflow.

## mStat\_generate\_report\_long()

The `mStat_generate_report_long()` function performs a comprehensive longitudinal microbiome analysis and generates a report PDF containing:

### Alpha diversity analysis

* Alpha diversity boxplots colored by time and groups
  * Using `generate_alpha_boxplot_long()`
* Alpha diversity spaghetti plots showing trajectories
  * Using `generate_alpha_spaghettiplot_long()`
* Linear mixed effects model results for alpha diversity trends
  * Using `generate_alpha_trend_test_long()`
* Linear model results for alpha diversity volatility
  * Using `generate_alpha_volatility_test_long()`

### Beta diversity analysis

* PCoA plots colored by time and groups
  * Using `generate_beta_ordination_long()`
* Boxplots of PCoA coordinates vs time
  * Using `generate_beta_pc_boxplot_long()`
* Distance from baseline spaghetti plots
  * Using `generate_beta_change_spaghettiplot_long()`
* Linear mixed effects models for beta diversity distance trends
  * Using `generate_beta_trend_test_long()`
* Linear models for beta diversity volatility
  * Using `generate_beta_volatility_test_long()`

### Taxonomic composition analysis

* Stacked area plots showing average composition
  * Using `generate_taxa_areaplot_long()`
* Heatmaps colored by relative abundance
  * Using `generate_taxa_heatmap_long()`
* Volcano plots highlighting significant taxa
  * Using `generate_taxa_trend_test_long()` and `generate_taxa_volatility_test_long()`
* Boxplots of significant taxa
  * Using `generate_taxa_boxplot_long()`
* Spaghetti plots for significant taxa
  * Using `generate_taxa_spaghettiplot_long()`

In summary, `mStat_generate_report_long()` provides extensive longitudinal microbiome analysis and visualization using core MicrobiomeStat functions.

It then compiles the results into a publication-ready PDF report containing:

* Summary statistics
  * Sample size, number of timepoints, covariates
* Plots like boxplots, heatmaps
  * Alpha diversity boxplots
  * Beta diversity PCoA plots
  * Taxa composition heatmaps
* Statistical test results in tables
  * Linear mixed effects model results
  * General linear model results
  * Tables with p-values
* Interpretation of findings
  * Description of significant results
  * Summary of important findings

In particular, the generated report includes:

* Alpha diversity analysis
  * Boxplots, spaghetti plots
  * Trend and volatility test tables
* Beta diversity analysis
  * PCoA plots, distance plots
  * Trend and volatility test tables
* Taxonomic composition analysis
  * Composition plots
  * Significant taxa tables
  * Volcano plots

So in summary, the report provides extensive visualization, statistical results, and interpretation to facilitate analysis and dissemination of longitudinal microbiome studies.

We provide the longitudinal data and analysis parameters:

```r

mStat_generate_report_long(
  data.obj = subset_T2D.obj, 
  dist.obj = NULL,
  alpha.obj = NULL,
  group.var = "sample_body_site", 
  adj.vars = c("subject_race"),
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
  transform = "log",
  theme.choice = "bw",
  base.size = 12, 
  output.file = "path/report.pdf"
)

```

The key parameters provided:

* data.obj - longitudinal microbiome data object
* group.var - grouping variable
* subject.var - subject IDs
* time.var - time variable
* alpha.name, dist.name - diversity indices to analyze
* t0.level, ts.levels - baseline and follow-up times
* feature.level - taxonomic level for analysis
* transform - data transformation

mStat\_generate\_report\_long() then performs extensive longitudinal analysis and generates a report PDF containing all results.

<figure><img src="../.gitbook/assets/Screenshot 2023-09-19 at 09.52.40.png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/Omics Analysis Report 69 subjects_page-0002 (1).jpg" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/Omics Analysis Report 69 subjects_page-0003 (1).jpg" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/Omics Analysis Report 69 subjects_page-0004 (2).jpg" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/Omics Analysis Report 69 subjects_page-0005 (1).jpg" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/Omics Analysis Report 69 subjects_page-0006 (1).jpg" alt=""><figcaption></figcaption></figure>

{% file src="../.gitbook/assets/Omics Analysis Report 69 subjects.pdf" %}

#### Benefits of Using `mStat_generate_report_long`

* **Comprehensive**: Conducts a wide array of analyses including alpha diversity, beta diversity, taxonomic composition, and compiles them into a single, easy-to-read report.
* **Customizable**: Numerous parameters for customizing the analysis as per the study's needs, such as specifying diversity indices, taxonomic levels, data transformations, color palettes, etc.
* **Automated Workflow**: Reduces manual errors and saves time compared to running each analysis individually. Streamlines the workflow from raw data to final report.
* **Simplified Interpretation**: Provides interpretations of key results, significantly easing understanding of the findings.
* **Publication-ready**: Generates a professionally formatted report that can be directly used for publications or presentations.

Overall, `mStat_generate_report_long()` automates the end-to-end longitudinal microbiome analysis process, making it easy to go from raw data to interpretable and shareable findings. This saves researchers time while also reducing errors and improving reproducibility.
