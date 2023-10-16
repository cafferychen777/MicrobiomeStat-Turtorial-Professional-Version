---
description: >-
  This document provides a comprehensive guide to paired samples analysis in
  microbiome research using MicrobiomeStat, with a focus on alpha and beta
  diversity and taxa composition.
---

# Unveiling Paired Samples Analysis: A Comprehensive Guide

In microbiome research, paired samples analysis is a crucial tool for examining dynamic changes within microbial communities. The MicrobiomeStat package enables analyses of samples collected at distinct time points, labeled as '1' and '2', as well as accommodating traditional 'pre-' and 'post-treatment' study designs. This guide will navigate you through a comprehensive analysis, from calculating alpha diversity and beta diversity to performing taxa analysis. The visualizations produced by MicrobiomeStat can elucidate patterns and trends in your data, facilitating a more straightforward interpretation and understanding of your results.

The dataset utilized in this guide is the peerj32 dataset, derived from a real-world study that investigates the interaction between human intestinal bacteria and host lipid metabolism under the influence of a probiotic intervention. The primary variable in this study is the 'LGG Probiotic/Placebo' treatment, with 'sex' serving not only as a stratifying variable but also as a covariate in the tests. The dataset encompasses data at the Phylum, Family, and Genus taxonomic levels, offering a granular perspective of the microbial communities in the samples. However, it's noteworthy that this dataset does not include a phylogenetic tree.

For an in-depth introduction and generation process of the peerj32 dataset, please refer to our previous cross-sectional study design tutorial.

{% content-ref url="../cross-sectional-study-design/unraveling-cross-sectional-studies-with-microbiomestat.md" %}
[unraveling-cross-sectional-studies-with-microbiomestat.md](../cross-sectional-study-design/unraveling-cross-sectional-studies-with-microbiomestat.md)
{% endcontent-ref %}

For users wishing to apply our tutorial to their own datasets, please see the following guide. 

{% content-ref url="../setting-up-microbiomestat-installation-and-data-preparation/laying-the-foundation-creating-the-microbiomestat-data-object/" %}
[laying-the-foundation-creating-the-microbiomestat-data-object](../setting-up-microbiomestat-installation-and-data-preparation/laying-the-foundation-creating-the-microbiomestat-data-object/)
{% endcontent-ref %}

This section explains how to convert your data into the `mStat` format, enabling you to use MicrobiomeStat for your research.

This guide is designed to provide a thorough introduction to paired samples analysis in microbiome research using MicrobiomeStat. By following the steps outlined here, you can gain a deeper understanding of the dynamics of microbial communities in your research.
