# Introduction
In clinical microbiome research, longtudinal studies haven been widely employed to study the microbiome's evolution over time and its relationship to an outcome of interest. By longitudinally sampling the microbiome, we have a more comprehensive and dynamic view of the microbiome, facilitating the identification of the critical temopral window when the microbiome is "in effect".  Longitudinal studies also contributes to the establishment of causal effects if the microbiome change precedes the manifestation of an clincial outcome.   Furthermore, by using self as a control, the effects of many variables can be efficiently controled, increasing the power as well as reducing potential confounding for certain association analyses. 

* Task 1 (time effect analysis): Anlyze all time points to see if the microbiome composition shows consistent changes with the time.
* Task 2 (global null analysis)：Jointly analyze all time points (categorical) and test the global hypothesis that none of the time points is associated with a subject-level outcome. This analysis will allow the identification of taxa whose abundance variation is associated with the outcome in certain ways. The global test pools all time points and  could be more powerful than testing individual time points followed by multiple testing correction.  Task 3 and 4 will then help understand the details of the association for  taxa that reject the global null hypothesis.
* Task 3 (per time point analysis): Test the association of the microbiome at different time points with the outcome. This will help identify the particular time window where the association occurs or peaks.
* Task 4 (change analysis): Test the association of the microbiome changes with the outcome.  By using changes, it controls the large inter-subject variability in microbiome composition, and amplifies the association strength.  It is likely that task 4 finds more associations than Task 4 with larger strength. 
* Task 5 (volatility analysis): It is possible that the average composition of the microbiome remains similar between conditions, but the variability differs by conditions.  Volatility analysis will help identify the taxa whose temporal variability ("volatility") is associated with the outcome.
* Task 6 (trend analysis): In the global null test (Task 2), we do not assume any temporal pattern such as linearly or monotonically increasing. If we have some prior knowledge about the temporal trend or the microbiome is more densely sampled longitudinally, we can model the temporal trend and test the association of the trend with the outcome.  This offers more power than Task 2 to identify the taxa associated with the outcome. 


This guide will navigate you through a comprehensive analysis of microbiome data from a longitudinal desgin. We will utilize two datasets to illustrate the analysis.

The first dataset is derived from the "Early Childhood Antibiotics and the Microbiome" (ECAM) study. This project observed the microbiome composition and development of 43 infants in the United States from birth through their second year. The study explored the impact of antibiotic exposure, delivery mode, and diet on early microbiome maturation.

> Bokulich, Nicholas A., et al. "Antibiotics, birth mode, and diet shape microbiome maturation during early life." Science translational medicine 8.343 (2016): 343ra82-343ra82.

The second dataset originates from the "T2D16S" study, which tracked a cohort of approximately 100 individuals at risk of diabetes. This study utilized omics analysis of longitudinal patient samples and snapshots of their microbiomes, with the objective of understanding the microbiome-host interactions during viral infections and throughout the onset and progression of diabetes. Due to the large size of this dataset, we have created a subset for our analysis, focusing only on the 16S rRNA sequencing data from the first six time points.

> Stansfield J, Smirnova E, Zhao N, Fettweis J, Waldron L, Dozmorov M (2023). _HMP2Data: 16s rRNA sequencing data from the Human Microbiome Project 2_. R package version 1.14.0, [https://github.com/jstansfield0/HMP2Data](https://github.com/jstansfield0/HMP2Data).

These datasets provide valuable insights into how early-life experiences and health conditions, such as diabetes, can influence the microbiome. Our goal is to leverage our analytical tool to explore the longitudinal dynamics of the microbiome presented in these datasets. This exploration will contribute to a more comprehensive understanding of the temporal intricacies of microbiome development and its susceptibilities.

It is important to note some constraints of our current development. Currently, `group.var` and `strata.var` are assumed to be non-time-varying (they stay the same for each subject), while `adj.var` could allow or not allow time-varying variables depending on the specific settings.  If a function can not adjust time-varying covariates, we will explictly state it. We will increase the flexibility in the future work. 

For users wishing to apply our tutorial to their own datasets, please see the following guide.

{% content-ref url="../setting-up-microbiomestat-installation-and-data-preparation/laying-the-foundation-creating-the-microbiomestat-data-object/" %}
[laying-the-foundation-creating-the-microbiomestat-data-object](../setting-up-microbiomestat-installation-and-data-preparation/laying-the-foundation-creating-the-microbiomestat-data-object/)
{% endcontent-ref %}

This section explains how to convert your data into the `mStat` format, enabling you to use MicrobiomeStat for your research.


