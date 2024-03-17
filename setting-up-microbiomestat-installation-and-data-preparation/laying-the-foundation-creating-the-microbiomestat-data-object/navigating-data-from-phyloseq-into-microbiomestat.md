---
description: >-
  Learn the process for converting Phyloseq data structures for compatibility
  with MicrobiomeStat.
---

# Converting Data from Phyloseq into MicrobiomeStat

This guide is tailored for users who already possess a Phyloseq object. If you haven't yet successfully constructed a Phyloseq object, we recommend reviewing this tutorial for guidance.&#x20;

{% content-ref url="building-microbiomestat-from-matrix-and-data.frame.md" %}
[building-microbiomestat-from-matrix-and-data.frame.md](building-microbiomestat-from-matrix-and-data.frame.md)
{% endcontent-ref %}

The current guide illustrates the procedure to convert a Phyloseq object into a MicrobiomeStat data object.

### Requirement: Taxa as Rows in Phyloseq

Before converting your Phyloseq object to a MicrobiomeStat data object, it is crucial to ensure that the taxa are represented as rows in your Phyloseq object. This is a requirement for the conversion process to work correctly.

To check if your Phyloseq object has taxa as rows, you can use the `phyloseq::taxa_are_rows()` function:

```r
library(phyloseq)

data(GlobalPatterns)

# Check if taxa are rows in the Phyloseq object
phyloseq::taxa_are_rows(GlobalPatterns)
```

If the function returns `TRUE`, your Phyloseq object is ready for conversion. If it returns `FALSE`, you need to transpose your Phyloseq object to have taxa as rows before proceeding with the conversion.

First, let's take a look at the original structure of our Phyloseq data:

<figure><img src="../../.gitbook/assets/Screenshot 2023-10-10 at 10.51.53.png" alt=""><figcaption><p><strong>Figure 1</strong>: Structure of the original Phyloseq data object.</p></figcaption></figure>

Now, initiate the conversion:

```r
# Required package
library(microbiome)

# Load the dataset
data(peerj32)
peerj32.phy <- peerj32$phyloseq

# Convert the Phyloseq object to a MicrobiomeStat data object
data.obj <- mStat_convert_phyloseq_to_data_obj(peerj32.phy)
```

After the conversion, the Phyloseq object is transformed into a MicrobiomeStat data object. Let's examine the structure post-conversion:

<figure><img src="../../.gitbook/assets/Screenshot 2023-10-10 at 10.52.44.png" alt=""><figcaption><p><strong>Figure 2</strong>: Structure of the MicrobiomeStat data object after converting from Phyloseq.</p></figcaption></figure>

### Caution Regarding Factor Levels

During the conversion, there might be changes in the levels of factors in the metadata. This can potentially alter the order or even result in missing levels.

**Recommendation:** It's important to verify the factors after conversion to ensure data consistency. If there are any changes, the factor levels should be reset:

```r
# Example: Adjusting the 'sex' factor levels
data.obj$meta.dat$sex <- factor(as.factor(data.obj$meta.dat$sex), levels = c("male", "female"))
```

Reorder the factor levels of the `sex` column。

The function `mStat_convert_phyloseq_to_data_obj` returns a MicrobiomeStat data object, which includes:

* **feature.tab**: A matrix representing the feature table (OTU table). Rows with a sum of zero are excluded, retaining only the features present in the samples.
* **meta.dat**: A data frame containing the sample data.
* **feature.ann**: A matrix of the feature annotation table (taxonomy table). Only the rows corresponding to the feature table are included.
* **tree**: A rooted phylogenetic tree. If the tree is not rooted, it will be rooted using the midpoint method. Tips not present in the OTU table are pruned.

After converting the Phyloseq object to the MicrobiomeStat format, the data is now compatible with MicrobiomeStat's analysis tools.
