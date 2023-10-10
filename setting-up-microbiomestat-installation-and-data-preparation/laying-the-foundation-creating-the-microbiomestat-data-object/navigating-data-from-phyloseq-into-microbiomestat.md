---
description: Learn the process for converting Phyloseq data structures for compatibility with MicrobiomeStat.
---

# Converting Phyloseq Data for Use with MicrobiomeStat

This guide illustrates the procedure to convert a Phyloseq object into a MicrobiomeStat data object.

First, let's take a look at the original structure of our Phyloseq data:

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

### Caution Regarding Factor Levels

During the conversion, there might be changes in the levels of factors in the metadata. This can potentially alter the order or even result in missing levels.

**Recommendation:** It's important to verify the factors after conversion to ensure data consistency. If there are any changes, the factor levels should be reset:

```r
# Example: Adjusting the 'sex' factor levels
data.obj$meta.dat$sex <- factor(as.factor(data.obj$meta.dat$sex), levels = c("male", "female"))
```

Replace `'sex'` with the appropriate metadata column and set the factor levels as required.

The function `mStat_convert_phyloseq_to_data_obj` returns a MicrobiomeStat data object, which includes:

* **feature.tab**: A matrix representing the feature table (OTU table). Rows with a sum of zero are excluded, retaining only the features present in the samples.
* **meta.dat**: A data frame containing the sample data.
* **feature.ann**: A matrix of the feature annotation table (taxonomy table). Only the rows corresponding to the feature table are included.
* **tree**: A rooted phylogenetic tree. If the tree is not rooted, it will be rooted using the midpoint method. Tips not present in the OTU table are pruned.

After converting the Phyloseq object to the MicrobiomeStat format, the data is now compatible with MicrobiomeStat's analysis tools.

With the data object set up, you can now proceed with MicrobiomeStat's microbiome data analysis functions.