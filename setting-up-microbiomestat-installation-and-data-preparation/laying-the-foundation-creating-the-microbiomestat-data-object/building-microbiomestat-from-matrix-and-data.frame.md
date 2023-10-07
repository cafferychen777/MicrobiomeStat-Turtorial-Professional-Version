---
description: >-
  Follow the guide to construct MicrobiomeStat's data object directly from
  matrix and data.frame structures.
---

# Building MicrobiomeStat from Matrix and Data.frame

The MicrobiomeStat data object consists of the following components:

* **feature.tab (Matrix)**: Stores microbiome feature abundance data, with **rows representing features** (OTUs, ASVs, genes, etc) and **columns representing samples**.
* **meta.dat (Dataframe)**: Stores sample metadata, with **each row representing a sample** and **columns representing metadata variables**.
* **feature.ann (Matrix, optional)**: Stores feature annotation data, with **rows representing features** and **columns representing annotations** (taxonomic classifications, functional categories, etc).
* **tree (optional)**: Represents evolutionary relationships between features.
* **feature.agg.list (List, optional)**: Aggregated statistics based on Feature.tab and Feature.ann.

Here is how to construct the MicrobiomeStat data object directly from matrix and dataframe:

### Starting from Matrix and Dataframe

Let's assume we already have the expression matrix and sample metadata:

```r
# Set sample names
samplenames <- paste0("S",1:20)

# Set feature names 
featurenames <- paste0("OTU",1:100)

# Construct Feature.tab
Feature.tab <- matrix(rnorm(100*20),nrow=100,ncol=20)
rownames(Feature.tab) <- featurenames
colnames(Feature.tab) <- samplenames

# Construct Meta.dat
Meta.dat <- data.frame(
  group = rep(c("A","B"),each=10),
  weight = rnorm(20,70,5)  
)
rownames(Meta.dat) <- samplenames
```

### Add Feature Annotation (Optional)

If feature annotation data is available, construct Feature.ann. Here we add taxonomic classifications and functional categories:

```r
Feature.ann <- matrix(c(rep(c("Bacteria","Archaea"),c(80,20)),
                        sample(c("Metabolism","Signaling","Regulation"), 100, replace=TRUE)),
                      nrow=nrow(Feature.tab), 
                      dimnames=list(rownames(Feature.tab),
                                   c("Kingdom", "Function")))
```

### Add Phylogenetic Tree (Optional)

If phylogenetic tree is available, directly assign:

```r
phylo <- ape::rtree(100) # generate random tree
```

### Combine into MicrobiomeStat Object

Finally, we combine the components into a single MicrobiomeStat object:

```r
MicrobiomeData <- list(
  feature.tab = Feature.tab,
  meta.dat = Meta.dat,
  feature.ann = Feature.ann,
  phylo = phylo
)
```

Now we have constructed a complete MicrobiomeStat data object directly from matrix and dataframe components! This object can then be passed into various analysis and visualization functions.
