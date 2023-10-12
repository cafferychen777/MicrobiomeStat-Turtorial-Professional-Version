---
description: >-
  Discover how to craft the MicrobiomeStat data object using matrices and data
  frames from your R environment or by importing data from external CSV files.
---

# Building MicrobiomeStat from Matrix and Data.frame

The MicrobiomeStat data object consists of the following components:

* **feature.tab (Matrix)**: A matrix displaying the connection between research entities (like OTUs, ASVs, genes) as rows and samples as columns. Ensure row names are the names of the research entities, and column names match the samples.
* **meta.dat (Dataframe)**: Holds metadata for samples, where rows correlate with samples, and columns denote annotations. Examples of annotations include disease severity (mild, moderate, severe), treatment status (treated, untreated), and study phases (phase 1, phase 2, etc). Ensure the row names match the feature.tab columns. Tibbles are not permitted; use a standard R data frame.
* **feature.ann (Matrix)**: An annotation matrix with research entities as rows. Columns might provide taxonomic details for microbiome data, cell types for single-cell data, or pathway levels for KEGG data. Ensure row names match those in feature.tab.
* **tree (optional)**: Depicts evolutionary relationships among research entities. It's essential for specific beta-diversity calculations but isn't required for the majority (\~99%) of MicrobiomeStat analyses.
* **feature.agg.list (List, optional)**: Generated using the `mStat_aggregate_by_taxonomy()` function, this list contains matrices of aggregated data based on taxonomy or cell type, with each matrix's columns representing sample names and rows representing the aggregation level (e.g., Phylum, CellType).

Here is how to construct the MicrobiomeStat data object directly from matrix and dataframe:

### 1. Using Existing Data in R Environment

#### Starting from Matrix and Dataframe

Assuming we already have the expression matrix and sample metadata:

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

#### Add Feature Annotation

To enrich the analysis, it's beneficial to add a feature annotation. For this example, we add taxonomic classifications and functional categories:

```r
Feature.ann <- matrix(c(rep(c("Bacteria","Archaea"),c(80,20)),
                        sample(c("Metabolism","Signaling","Regulation"), 100, replace=TRUE)),
                      nrow=nrow(Feature.tab), 
                      dimnames=list(rownames(Feature.tab),
                                   c("Kingdom", "Function")))
```

#### Add Phylogenetic Tree (Optional)

If a phylogenetic tree is available, directly assign:

```r
tree <- ape::rtree(100) # generate random tree
```

#### Combine into MicrobiomeStat Object

Now, combine the components into a MicrobiomeStat object:

```r
MicrobiomeData <- list(
  feature.tab = Feature.tab,
  meta.dat = Meta.dat,
  feature.ann = Feature.ann,
  tree = tree
)
```

<figure><img src="../../.gitbook/assets/Screenshot 2023-10-10 at 10.18.21.png" alt=""><figcaption><p>The image displays the structured breakdown of the <code>MicrobiomeStat</code> data object using R's <code>str</code> function. Additionally, the successful validation of the object with <code>mStat_validate_data(MicrobiomeData)</code> confirms its readiness for subsequent analyses.</p></figcaption></figure>

### 2. Using External CSV Data

To facilitate your understanding and to ease the process of setting up your data, we have provided sample CSV files that can be used as templates. By analyzing these sample files, you can structure your data similarly and ensure it's compatible with our tool.

For this demonstration, we'll import data from three external CSV files: `feature_tab.csv`, `meta_dat.csv`, and `feature_ann.csv`.

#### Download Sample Data Files

You can download these files to understand the expected format:

{% file src="../../.gitbook/assets/feature_tab.csv" %}

{% file src="../../.gitbook/assets/meta_dat.csv" %}

{% file src="../../.gitbook/assets/feature_ann.csv" %}

If you want to use your own data, download these example files, examine their structure, and modify your data accordingly to match this format.

#### Import Data from CSV

Once you have the desired CSV files, you can proceed to import the data:

```r
Feature.tab <- read.csv("feature_tab.csv", row.names = 1)
Meta.dat <- read.csv("meta_dat.csv", row.names = 1)
Feature.ann <- read.csv("feature_ann.csv", row.names = 1)
```

#### Combine into MicrobiomeStat Object

With the imported data, combine the components:

```r
MicrobiomeData <- list(
  feature.tab = as.matrix(Feature.tab),
  meta.dat = Meta.dat,
  feature.ann = as.matrix(Feature.ann)
)
```

<figure><img src="../../.gitbook/assets/Screenshot 2023-10-10 at 10.28.16.png" alt=""><figcaption><p>This visualization presents the structure of the imported <code>MicrobiomeStat</code> data object using R's <code>str</code> function, illustrating the organization of the components sourced from external CSV files.</p></figcaption></figure>

With either the provided sample data or your own structured data, you now have a complete MicrobiomeStat data object ready for subsequent analysis and visualization.
