---
description: >-
  Automate analysis reporting for cross-sectional studies with MicrobiomeStat.
  Generate integrated reports encompassing alpha diversity, beta diversity and
  taxonomic compositions.
---

# Cross-Sectional Reporting: Microbial Analysis Reports with MicrobiomeStat

Microbiome studies produce **multidimensional datasets** with intricacies along various analytical dimensions including:

- Alpha diversity
- Beta diversity 
- Differential abundance
- Functional potential
- Correlation networks

Manual interpretation across dimensions is challenging. 

**MicrobiomeStat** provides a robust automation solution through **integrated analysis reports** for cross-sectional studies. The reports contain intuitive visualizations and statistical summaries of key results across dimensions to support biological interpretation and accelerate discoveries. 

By automating complex analytical workflows, MicrobiomeStat enables rapid, reproducible, and comprehensive reporting to boost productivity in microbiome research.

The `mStat_generate_report_single()` function performs a comprehensive analysis encompassing:

- **Alpha diversity** indices using `generate_alpha_boxplot_single()` and `generate_alpha_test_single()`
- **Beta diversity** ordination and PERMANOVA testing using `generate_beta_ordination_single()` and `generate_beta_test_single()` 
- **Taxonomic composition** visualization and differential abundance testing using `generate_taxa_barplot_single()`, `generate_taxa_heatmap_single()` and `generate_taxa_test_single()`

It then automatically compiles an integrated PDF report containing:

- Summary statistics from mStat_summarize_data_obj() 

- Alpha diversity plots from generate_alpha_boxplot_single()

- Beta diversity plots from generate_beta_ordination_single()

- Taxa composition plots from generate_taxa_barplot_single(), generate_taxa_dotplot_single() and generate_taxa_heatmap_single()

- Statistical test results tables for alpha, beta and taxonomic data

- Text interpretations of the statistical findings

We simply provide parameters like input data, variables of interest, and analysis settings:

```r
mStat_generate_report_single(
  data.obj = peerj32.obj, 
  dist.obj = NULL,   
  group.var = "group", 
  adj.vars = c("sex"),
  subject.var = "subject",   
  time.var = "time",
  alpha.name = c("shannon","simpson"),
  dist.name = c("BC",'Jaccard'),   
  t.level = "1",   
  transform = "log",
  strata.var = "sex",   
  feature.level = c("Phylum","Family"),
  feature.dat.type = "count",   
  theme.choice = "bw",
  base.size = 12,
  output.file = "path/report.pdf" 
)
```

{% file src="../.gitbook/assets/mStat_generate_report_single_example.pdf" %}

The automated report relieves the burden of **manual analysis** and promotes consistency. By **integrating results** from diverse analytical dimensions, it strengthens evidence and tells a cohesive story.

MicrobiomeStat powers researchers with **automated workflows** to swiftly synthesize and report discoveries from cross-sectional studies. The standardized reports integrate multidimensional perspectives including:

- **Alpha diversity** 
- **Beta diversity**
- **Differential abundance analysis**
- **Functional potential analysis**
- **Correlation networks**

for comprehensive insights. 

The reports contain intuitive visualizations and statistical summaries to support biological interpretation and facilitate result dissemination. By automating time-consuming, manual analytical tasks, MicrobiomeStat enables rapid, reproducible, and robust reporting to accelerate microbiome research.
