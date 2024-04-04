# Introduction

Cross-sectional microbiome studies, which randomly sample the microbiome from a study population at a single time point, have been widely used in microbiome research due to its simplicity and easy sample collection. Besides cross-sectional design, case-control study designs are also prevalent in microbiome research.  Both designs will generate microbiome data that are dependent of each other and can be analyzed in a similar way by treating the microbiome measruement as the outcome. MicrobiomeStat provides full support of analyzing the data from cross-sectional and case-control studies. We illustrate the use of MicrobiomeStat in analyzing cross-sectional/case-control data using the `peerj32` dataset, which was originated from a study that probed the relationship between human intestinal bacteria and lipid metabolism in response to a probiotic intervention. 

> Lahti L, Salonen A, Kekkonen RA, Salojärvi J, Jalanka-Tuovinen J, Palva A, Orešič M, de Vos WM. Associations between the human intestinal microbiota, Lactobacillus rhamnosus GG and serum lipids indicated by integrated analysis of high-throughput profiling data. PeerJ. 2013 Feb 26;1:e32. doi: 10.7717/peerj.32. PMID: 23638368; PMCID: PMC3628737.

**Exploring the PeerJ32 Dataset**

The `peerj32`  was originally in the `phyloseq` format. We have converted it to our MicrobiomeStat format using the `mStat_convert_phyloseq_to_data_obj` function. For an in-depth look at the conversion process, please refer to this section in our Gitbook.

{% content-ref url="../setting-up-microbiomestat-installation-and-data-preparation/laying-the-foundation-creating-the-microbiomestat-data-object/navigating-data-from-phyloseq-into-microbiomestat.md" %}
[navigating-data-from-phyloseq-into-microbiomestat.md](../setting-up-microbiomestat-installation-and-data-preparation/laying-the-foundation-creating-the-microbiomestat-data-object/navigating-data-from-phyloseq-into-microbiomestat.md)
{% endcontent-ref %}

The major variable of interest in this study is the treatment group: LGG Probiotic or Placebo. Additionally, the dataset includes other variables such as the sex of participants, which can serve both as a stratifying factor in visualization and as a covariate in statistical testing.

<figure><img src="../.gitbook/assets/Screenshot 2023-10-10 at 18.31.52.png" alt=""><figcaption><p>Sample metadata overview from the <code>peerj32</code> dataset. This table provides details about each sample, including the subject ID, cons, time point, sex of the participant, and the treatment group assignment (Placebo or LGG).</p></figcaption></figure>

The dataset provides taxonomic classifications at three ranks: Phylum, Family, and Genus. The phylogenetic tree is not provided in this dataset.

<figure><img src="../.gitbook/assets/Screenshot 2023-10-10 at 18.32.30.png" alt=""><figcaption><p>Taxonomic annotations for the microbial features in the <code>peerj32</code> dataset. Each row represents a microbial feature, classified at the Phylum, Family, and Genus levels.</p></figcaption></figure>

We will use `peerj32` dataset to showcase the utility of MicrobiomeStats in analyzing cross-sectional/case-control data.

For users wishing to apply our tutorial to their own datasets, please see the following guide:

{% content-ref url="../setting-up-microbiomestat-installation-and-data-preparation/laying-the-foundation-creating-the-microbiomestat-data-object/" %}
[laying-the-foundation-creating-the-microbiomestat-data-object](../setting-up-microbiomestat-installation-and-data-preparation/laying-the-foundation-creating-the-microbiomestat-data-object/)
{% endcontent-ref %}

This section explains how to convert your data into the MicrobiomeStat format, enabling you to use the MicrobiomeStat toolkit for your research.

