---
description: >-
  Learn how to create MicrobiomeStat data objects, a crucial step in your
  microbiome analysis journey.
---

# Laying the Foundation: Creating the MicrobiomeStat Data Object

<figure><img src="../../.gitbook/assets/image (1).png" alt=""><figcaption><p>Figure 1: The Structure of a MicrobiomeStat Data Object. This diagram illustrates the interplay among the five components: 'Feature.tab', 'Meta.dat', 'Feature.ann', the optional 'Phylogenetic tree', and the optional 'Feature.agg.list'. It highlights how samples and features (OTU/ASV/KEGG/Gene etc.) align to connect these components.</p></figcaption></figure>

"**Objective: Constructing the MicrobiomeStat Data Object** You're just a few strides away from **mastering the inner workings** of the MicrobiomeStat data object. Let's unfold its **core components** and why each is a key pillar in your microbial analysis!

**Component 1: Feature.tab (Matrix)** At the heart of the data object lies our matrix - the **feature.tab**. Acting as a meeting point for your research objects (**OTU/ASV/KEGG/Gene**, etc.) and **samples**, this matrix makes sure your data is ready for the big analysis reveal. With **research objects forming the rows and samples the columns**, it lays out a comprehensive view of your data.

**Component 2: Meta.dat (Data frame)** Moving onto **meta.dat**, a meticulously arranged data frame. Here, the rows correspond to the samples, marrying up perfectly with the columns of feature.tab. Each column is an **annotation**, offering an in-depth descriptive layer to your samples.

**Component 3: Feature.ann (Matrix)** Next in line is the **feature.ann**, a matrix where research objects take the role of rows, aligning with those of feature.tab. Its columns serve as **annotations**, carrying classification information like **Kingdom, Phylum**, etc., or other helpful insights.

**Component 4: Phylogenetic tree (Optional)** Optional but enlightening, the **phylogenetic tree** offers an evolutionary perspective, illuminating the relationships among various research objects. It provides an **extra dimension** to your analysis, if needed.

**Component 5: Feature.agg.list (Optional)** Finally, we present **feature.agg.list**, an optional component delivering aggregated results based on the **feature.tab** and **feature.ann**. It's ready to be called into action when comprehensive outputs are required.

This ensemble makes up the DNA of our **MicrobiomeStat data object**, an all-rounder in prepping your data for an adventurous ride in microbiome analysis. Now, let's step into the fascinating world of each component."&#x20;
