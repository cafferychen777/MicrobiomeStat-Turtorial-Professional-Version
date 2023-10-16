---
description: >-
  A guide to leveraging MicrobiomeStat for comprehensive analyses in
  cross-sectional microbiome studies.
---

# Unraveling Cross-Sectional Studies with MicrobiomeStat

Cross-sectional studies, which analyze data from a single point in time, are a fundamental component of microbiome research. MicrobiomeStat offers versatility in handling and analyzing such datasets, whether they capture standalone time points, specific moments from paired or longitudinal datasets, or treat paired and longitudinal datasets as cohesive units for comprehensive analysis.

One notable dataset that can be explored using this framework is the `peerj32` dataset. Originating from a study that probed the relationship between human intestinal bacteria and lipid metabolism in response to a probiotic intervention, this dataset offers rich insights.

**Exploring the PeerJ32 Dataset**

The `peerj32` dataset is sourced from the study on associations between the human intestinal microbiota and Lactobacillus rhamnosus GG and was originally in the `phyloseq` format.&#x20;

> Lahti L, Salonen A, Kekkonen RA, Salojärvi J, Jalanka-Tuovinen J, Palva A, Orešič M, de Vos WM. Associations between the human intestinal microbiota, Lactobacillus rhamnosus GG and serum lipids indicated by integrated analysis of high-throughput profiling data. PeerJ. 2013 Feb 26;1:e32. doi: 10.7717/peerj.32. PMID: 23638368; PMCID: PMC3628737.

We have converted it to the `mStat` format using the `mStat_convert_phyloseq_to_data_obj` function. For an in-depth look at the conversion process, please refer to this section in our Gitbook.

{% content-ref url="../setting-up-microbiomestat-installation-and-data-preparation/laying-the-foundation-creating-the-microbiomestat-data-object/navigating-data-from-phyloseq-into-microbiomestat.md" %}
[navigating-data-from-phyloseq-into-microbiomestat.md](../setting-up-microbiomestat-installation-and-data-preparation/laying-the-foundation-creating-the-microbiomestat-data-object/navigating-data-from-phyloseq-into-microbiomestat.md)
{% endcontent-ref %}

The principal experimental variable in this study was the treatment group: LGG Probiotic or Placebo. Additionally, the dataset includes other variables like the gender of participants, termed 'sex', which can serve both as a stratifying factor and as a covariate during statistical analysis.

<figure><img src="../.gitbook/assets/Screenshot 2023-10-10 at 18.31.52.png" alt=""><figcaption><p>Sample metadata overview from the <code>peerj32</code> dataset. This table provides details about each sample, including the subject ID, a consistent value, the time point of data collection, gender of the participant, and the treatment group (Placebo or LGG).</p></figcaption></figure>

The dataset presents taxonomic classifications at three tiers: Phylum, Family, and Genus. Notably, it does not include a phylogenetic tree, a detail that researchers should be mindful of when planning analyses.

<figure><img src="../.gitbook/assets/Screenshot 2023-10-10 at 18.32.30.png" alt=""><figcaption><p>Taxonomic annotations for the microbial features in the <code>peerj32</code> dataset. Each row represents a microbial feature, categorized at the Phylum, Family, and Genus levels.</p></figcaption></figure>

With MicrobiomeStat, users can adeptly manage, process, and analyze this dataset, drawing from its depth and breadth to produce scientifically sound insights.

As we proceed, we will further dissect the `peerj32` dataset, understanding its intricate details and harnessing its data for robust conclusions.

For users wishing to apply our tutorial to their own datasets, please see the following guide. 

{% content-ref url="../setting-up-microbiomestat-installation-and-data-preparation/laying-the-foundation-creating-the-microbiomestat-data-object/" %}
[laying-the-foundation-creating-the-microbiomestat-data-object](../setting-up-microbiomestat-installation-and-data-preparation/laying-the-foundation-creating-the-microbiomestat-data-object/)
{% endcontent-ref %}

This section explains how to convert your data into the `mStat` format, enabling you to use MicrobiomeStat for your research.

Harness the capabilities of MicrobiomeStat to derive meaningful conclusions from cross-sectional microbiome datasets and advance your research endeavors.
