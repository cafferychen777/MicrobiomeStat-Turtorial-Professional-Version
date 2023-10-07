---
description: Master the steps to import data from Phyloseq into MicrobiomeStat.
---

# Navigating Data from Phyloseq into MicrobiomeStat

Let's dive in! Your pathway from Phyloseq to MicrobiomeStat is as smooth as silk:

```r
# Example code
# Don't forget to have the 'microbiome' package installed first!
library(microbiome)

# Load the dataset
data(peerj32)
peerj32.phy <- peerj32$phyloseq

# Convert the Phyloseq object to a MicrobiomeStat data object
data.obj <- mStat_convert_phyloseq_to_data_obj(peerj32.phy)
```

Just like that, your Phyloseq object transmutes into a **MicrobiomeStat data object**.

### ⚠️ Heads up on Factor Loss!

While the conversion process is streamlined, there's a possibility of factor loss in your metadata when importing from a Phyloseq object into MicrobiomeStat. This can disrupt the order of levels or even drop levels entirely.

**Solution:** To ensure the integrity of your data, always double-check your factors after conversion. If you notice any discrepancies, reset the factor levels:

```r
# Example: Setting the 'sex' factor in the desired order
data.obj$meta.dat$sex <- factor(as.factor(data.obj$meta.dat$sex), levels = c("male", "female"))
```

Replace `'sex'` with the relevant metadata column and adjust the factor levels accordingly.

With `mStat_convert_phyloseq_to_data_obj`, you acquire a **MicrobiomeStat data object** (a list) brimming with:

* **feature.tab**: A matrix of the feature table (OTU table). Superfluous rows with a sum of zero are swept away, leaving only the features present in the samples.
* **meta.dat**: A data frame of the sample data. This serves as a comprehensive ledger, housing the metadata for each of your samples.
* **feature.ann**: A matrix of the feature annotation table (taxonomy table). Only the rows that have made their mark in the feature table are included.
* **tree**: A rooted phylogenetic tree. If not already rooted, the tree is given firm grounding by midpointing. Tips not present in the OTU table are pruned.

By **weaving your Phyloseq object into the fabric of MicrobiomeStat format**, you awaken the boundless potential of MicrobiomeStat's analytical capabilities and set the stage for your data to perform in the grand theatre of microbiome statistical analysis.

With your data object prepped and primed, you're ready to embark on the captivating voyage of microbiome data analysis with MicrobiomeStat. Let the adventure begin!
