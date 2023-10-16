---
description: >-
  This section provides an introduction to longitudinal studies in microbiome
  research and an overview of the example dataset used for the purpose.
---

## Longitudinal Studies in Microbiome Research: Introduction and Dataset Overview

Longitudinal studies are a prevalent methodology in microbiome research. They enable the observation of temporal changes and complexities that are not accessible through cross-sectional measurements. These studies facilitate the examination of the interactions between the microbiome and various influencing factors such as the environment, medical interventions, and disease progression over time.

MicrobiomeStat can handle the complexities of longitudinal data. It offers an approach to analyze temporal dynamics, including changes in alpha diversity, shifts in beta diversity relative to baseline, and variations in specific taxa over time. It also aids in visualizing taxa composition trends, demonstrating changes in the abundance and prevalence of taxa over time.

In this chapter, we will utilize two datasets to explore longitudinal microbiome research.

The first dataset is derived from the "Early Childhood Antibiotics and the Microbiome" (ECAM) study. This project observed the microbiome composition and development of 43 infants in the United States from birth through their second year. The study explored the impact of antibiotic exposure, delivery mode, and diet on early microbiome maturation.

> Bokulich, Nicholas A., et al. "Antibiotics, birth mode, and diet shape microbiome maturation during early life." Science translational medicine 8.343 (2016): 343ra82-343ra82.

The second dataset originates from the "T2D16S" study, which tracked a cohort of approximately 100 individuals at risk of diabetes. This study utilized omics analysis of longitudinal patient samples and snapshots of their microbiomes, with the objective of understanding the microbiome-host interactions during viral infections and throughout the onset and progression of diabetes. Due to the extensive nature of this dataset, we have created a subset for our analysis, focusing only on the 16S rRNA sequencing data from the first six time points.

> Stansfield J, Smirnova E, Zhao N, Fettweis J, Waldron L, Dozmorov M (2023). _HMP2Data: 16s rRNA sequencing data from the Human Microbiome Project 2_. R package version 1.14.0, [https://github.com/jstansfield0/HMP2Data](https://github.com/jstansfield0/HMP2Data).

These datasets offer insights into how early-life experiences and health conditions like diabetes can influence the microbiome. Using our analytical tool, we aim to analyze these datasets to explore the longitudinal dynamics of the microbiome, contributing to a better understanding of the temporal intricacies of microbiome development and its susceptibilities.

For users wishing to apply our tutorial to their own datasets, please see the following guide. 

{% content-ref url="../setting-up-microbiomestat-installation-and-data-preparation/laying-the-foundation-creating-the-microbiomestat-data-object/" %}
[laying-the-foundation-creating-the-microbiomestat-data-object](../setting-up-microbiomestat-installation-and-data-preparation/laying-the-foundation-creating-the-microbiomestat-data-object/)
{% endcontent-ref %}

This section explains how to convert your data into the `mStat` format, enabling you to use MicrobiomeStat for your research.

In the following sections, we will delve into the analysis of these longitudinal datasets using advanced analytical tools. Stay tuned as we explore the complexities of the microbiome over time.
