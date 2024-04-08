# Introduction

In clinical microbiome research, paired design is a common study design that are frequently used to study the treatment effects. By using self as a control, the effects of many variables can be efficiently controled, increasing the analysis power as well as reducing potential confounding.   By sampling the microbiome twice, it is also possible to study the microbiome dynamics/evolution and its association with some treatment outcome. Depending on how the paired samples are collected, there are mainly two setting involving paired samples. In the first setting, the two samples from the same subject are under two different condtions.  For example, one sample could be before treatment and the other sample could be after treatment. The main interest is to study the difference between the microbiomes before and after treatment, and to correlate the microbiome change with some treatment outcome.   In the second setting, the two samples are collected under the same condition. For example, the two samples are both obtained in the treatment course, so both samples are under the same treatment condition. The main purpose is to understand how the microbiome evolves in the treatment course, and how the evolution is related to the treatment outcome.  In both settings, we will label the two samples as "t1" and "t2", repsectively.  Although the questions to be asked may be different from the two settings, the analysis tasks share much commonality:

* Task 1 (paired comparison): Compare the t1 vs. t2 to see if there are community-level and feature-level microbiome differences.
* Task 2 (per time point analysis): Given a subject-level outcome, invesitigate the association of the t1 and t2 microbiome with the outcome, separately, to see how the association changes.
* Task 3 (analysis of changes): If there are changes between t1 and t2, investigate whether and how the changes are associated with an outcome.
* Task 4 (joint analysis)：Jointly analyze t1 and t2 and test the global hypothesis that none of the two time points is associated with an outcome. This analysis is informative when we are not sure when the association takes place, and the timing may be different among subjects.

 This guide will navigate you through a comprehensive analysis, from calculating alpha diversity and beta diversity to performing taxa analysis. The visualizations produced by MicrobiomeStat can provide patterns and trends in your data, facilitating a more straightforward interpretation and understanding of your results.

The dataset utilized in this guide is the peerj32 dataset, derived from a real-world study that investigates the interaction between human intestinal bacteria and host lipid metabolism under the influence of a probiotic intervention. The primary variable in this study is the 'LGG Probiotic/Placebo' treatment, with 'sex' serving not only as a stratifying variable but also as a covariate in the tests. The dataset includes data at the Phylum, Family, and Genus taxonomic levels, offering a granular perspective of the microbial communities in the samples. However, it's noteworthy that this dataset does not include a phylogenetic tree.

For an in-depth introduction and generation process of the peerj32 dataset, please refer to our previous cross-sectional study design tutorial.

{% content-ref url="../cross-sectional-study-design/unraveling-cross-sectional-studies-with-microbiomestat.md" %}
[unraveling-cross-sectional-studies-with-microbiomestat.md](../cross-sectional-study-design/unraveling-cross-sectional-studies-with-microbiomestat.md)
{% endcontent-ref %}

Due to the constraints of development progress, our current group.var, strata.var, adj.vars are all assumed to have temporal consistency. That is, for the same subject, these states do not change over time. If there are functions applicable to time-varying properties in the tutorial, we will specifically mark them out.

For users wishing to apply our tutorial to their own datasets, please see the following guide.

{% content-ref url="../setting-up-microbiomestat-installation-and-data-preparation/laying-the-foundation-creating-the-microbiomestat-data-object/" %}
[laying-the-foundation-creating-the-microbiomestat-data-object](../setting-up-microbiomestat-installation-and-data-preparation/laying-the-foundation-creating-the-microbiomestat-data-object/)
{% endcontent-ref %}

This section explains how to convert your data into the `mStat` format, enabling you to use MicrobiomeStat for your research.

This guide is designed to provide a thorough introduction to paired samples analysis in microbiome research using MicrobiomeStat. By following the steps outlined here, you can gain a deeper understanding of the dynamics of microbial communities in your research.
