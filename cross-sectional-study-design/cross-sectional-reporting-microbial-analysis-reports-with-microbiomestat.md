---
description: >-
  MicrobiomeStat offers automated analysis reporting for cross-sectional studies in microbiome research. It generates comprehensive reports covering alpha diversity, beta diversity, and taxonomic compositions.
---

# Cross-Sectional Microbial Analysis Reports via MicrobiomeStat

Microbiome research yields complex multidimensional datasets. These encompass:

- Alpha diversity: Represents within-sample taxonomic diversity.
- Beta diversity: Captures between-sample taxonomic dissimilarity.
- Differential abundance: Pinpoints differentially abundant taxa across experimental conditions.

Manually analyzing these dimensions is not only tedious but prone to inconsistencies. 

MicrobiomeStat offers an efficient approach, generating integrated reports for cross-sectional studies. These reports encompass:

- Visualizations to illustrate key findings
- Statistical summaries highlighting the main outcomes

This ensures a thorough and consistent interpretation of data.

Central to this process is the `mStat_generate_report_single()` function. It conducts:

- **Alpha diversity** analyses: These utilize functions like `mStat_calculate_alpha_diversity()`, `generate_alpha_boxplot_single()`, and `generate_alpha_test_single()`.
- **Beta diversity** analyses: Leveraging functions such as `mStat_calculate_beta_diversity()`, `generate_beta_ordination_single()`, and `generate_beta_test_single()`.
- **Taxonomic composition** analyses: Employing `generate_taxa_barplot_single()`, `generate_taxa_dotplot_single()`, `generate_taxa_heatmap_single()`, and `generate_taxa_test_single()`.

The function then assembles these analyses into a cohesive PDF report, featuring:

- A data summary from `mStat_summarize_data_obj()`
- Visual representations like alpha diversity boxplots, beta diversity ordination plots, and taxa composition visualizations
- Tables detailing statistical test results
- Commentaries on key findings

Here's how the function can be implemented:

```r
 mStat_generate_report_single(
   data.obj = peerj32.obj,
   dist.obj = NULL,
   alpha.obj = NULL,
   group.var = "group",
   vis.adj.vars = c("sex"),
   test.adj.vars = c("sex"),
   subject.var = "subject",
   time.var = "time",
   alpha.name = c("shannon", "observed_species"),
   depth = NULL,
   dist.name = c("BC",'Jaccard'),
   t.level = "1",
   feature.box.axis.transform = "sqrt",
   strata.var = "sex",
   vis.feature.level = c("Phylum", "Family", "Genus"),
   test.feature.level = "Family",
   feature.dat.type = "count",
   theme.choice = "bw",
   base.size = 20,
   feature.mt.method = "none",
   feature.sig.level = 0.2,
   output.file = "path/to/mStat_generate_report_single_example.pdf"
 )
```

<figure><img src="../.gitbook/assets/mStat_generate_report_single_example_page-0001.jpg" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/mStat_generate_report_single_example_page-0002.jpg" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/mStat_generate_report_single_example_page-0003.jpg" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/mStat_generate_report_single_example_page-0004.jpg" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/mStat_generate_report_single_example_page-0005.jpg" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/mStat_generate_report_single_example_page-0006.jpg" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/mStat_generate_report_single_example_page-0007.jpg" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/mStat_generate_report_single_example_page-0008.jpg" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/mStat_generate_report_single_example_page-0009.jpg" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/mStat_generate_report_single_example_page-0010.jpg" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/mStat_generate_report_single_example_page-0011.jpg" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/mStat_generate_report_single_example_page-0012.jpg" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/mStat_generate_report_single_example_page-0013.jpg" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/mStat_generate_report_single_example_page-0014.jpg" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/mStat_generate_report_single_example_page-0015.jpg" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/mStat_generate_report_single_example_page-0016.jpg" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/mStat_generate_report_single_example_page-0017.jpg" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/mStat_generate_report_single_example_page-0018.jpg" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/mStat_generate_report_single_example_page-0019.jpg" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/mStat_generate_report_single_example_page-0020.jpg" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/mStat_generate_report_single_example_page-0021.jpg" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/mStat_generate_report_single_example_page-0022.jpg" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/mStat_generate_report_single_example_page-0023.jpg" alt=""><figcaption></figcaption></figure>


{% file src="../.gitbook/assets/mStat_generate_report_single_example.pdf" %}

The automated report relieves the burden of **manual analysis** and promotes consistency. By **integrating results** from diverse analytical dimensions, it strengthens evidence and tells a cohesive story.

MicrobiomeStat powers researchers with **automated workflows** to swiftly synthesize and report discoveries from cross-sectional studies. The standardized reports integrate multidimensional perspectives including:

- **Alpha diversity** 
- **Beta diversity**
- **Differential abundance analysis**

for comprehensive insights. 

The reports contain intuitive visualizations and statistical summaries to support biological interpretation and facilitate result dissemination. By automating time-consuming, manual analytical tasks, MicrobiomeStat enables rapid, reproducible, and robust reporting to accelerate microbiome research.
