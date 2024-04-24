# Introduction

In clinical microbiome research, the paired samples design is a common study design that are frequently used to study the treatment effects. By using self as a control, the effects of many variables can be efficiently controled, increasing the analysis power as well as reducing potential confounding. By sampling the microbiome longitudinally, it is also possible to study the microbiome dynamics/evolution and its association with some treatment outcome. Depending on how the paired samples are collected, there are mainly two setting involving paired samples. In the first setting, the two samples from the same subject are under two different condtions. For example, one sample could be before treatment and the other sample could be after treatment. The main interest is to study the difference between the microbiomes before and after treatment, and to correlate the microbiome change with some treatment outcome. In the second setting, the two samples are collected under the same condition. For example, the two samples are both obtained in the treatment course, so both samples are under the same treatment condition. The main purpose is to understand how the microbiome evolves in the treatment course, and how the evolution is related to the treatment outcome. In both settings, we will label the two samples as "t1" and "t2", respectively. Although the questions to be asked may be different from the two settings, the analysis tasks share much commonality:

* Task 1 (paired comparison): Compare the t1 vs. t2 to see if there are community-level and feature-level microbiome differences.
* Task 2 (per time point analysis): Given a subject-level outcome, invesitigate the association of the t1 and t2 microbiome with the outcome, separately, to see how the association changes.
* Task 3 (analysis of changes): If there are changes between t1 and t2, investigate whether and how the changes are associated with an outcome.
* Task 4 (joint analysis)：Jointly analyzing t1 and t2 and test the global hypothesis that none of the two time points is associated with an outcome (omnibus test). This analysis will allow the identification of taxa whose abundance differs in some ways with respect to the outcome. The p-values for individual terms as well as the results from Task 2 and 3 will help understand the nature of the associations.

Task 2 can be achieved using the functions for single time point analysis described in last section, and other Tasks are specific for paired samples design. Although Task 1-3 in principle can be simulatenuously achieved by the joint modeling approach of Task 4, tools for analysis of independent data (Task 1-3) are usually more abundant, robust and flexible and require less assumptions than the joint approach. Task 3 is particularly useful if the difference at the t1 (baseline) is assumed not to exist, for example, due to randomization，or the baseline difference is a nusaince and should be controlled. Analysis of changes standardizes the large inter-subject variablity at the baseline and is usually more powerful than analysis of t2 only. This guide will navigate you through a comprehensive analysis of microbiome data from a paired samples desgin. The visualizations produced by MicrobiomeStat can reveal patterns and trends in your data, facilitating a more straightforward interpretation and understanding of your results.

The dataset utilized in this guide is the peerj32 dataset, derived from a real-world study that investigates the interaction between human intestinal bacteria and host lipid metabolism under the influence of a probiotic intervention. The primary variable in this study is the 'LGG Probiotic/Placebo' treatment, with 'sex' serving as a stratifying variable in visualization and a covariate to be adjusted in the tests. The dataset includes taxonomic abundance data at the Phylum, Family, and Genus levels.

For an in-depth introduction and generation process of the peerj32 dataset, please refer to our previous cross-sectional study design tutorial.

{% content-ref url="../single-point-analysis/unraveling-cross-sectional-studies-with-microbiomestat.md" %}
[unraveling-cross-sectional-studies-with-microbiomestat.md](../single-point-analysis/unraveling-cross-sectional-studies-with-microbiomestat.md)
{% endcontent-ref %}

For users wishing to apply our tutorial to their own datasets, please see the following guide.

{% content-ref url="../setting-up-microbiomestat-installation-and-data-preparation/laying-the-foundation-creating-the-microbiomestat-data-object/" %}
[laying-the-foundation-creating-the-microbiomestat-data-object](../setting-up-microbiomestat-installation-and-data-preparation/laying-the-foundation-creating-the-microbiomestat-data-object/)
{% endcontent-ref %}

This section explains how to convert your data into the `mStat` format, enabling you to use MicrobiomeStat for your research.

Note that, in our current implementation, the `group.var` and `strata.var` should be non-time-varying, i.e., they should stay constant across time points. `adj.vars` on the other hand could be time-varying in most cases. If only non-time-varying variables are allowed, we will state it explicitly.
