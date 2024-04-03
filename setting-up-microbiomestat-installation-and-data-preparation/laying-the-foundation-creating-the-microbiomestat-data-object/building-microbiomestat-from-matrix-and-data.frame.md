---
description: >-
  Discover how to craft the MicrobiomeStat data object using matrices and data
  frames from your R environment or by importing data from external CSV files.
---

# Building MicrobiomeStat Object from Matrix and Data.frame

The basic MicrobiomeStat data object consists of the following components:

* **feature.tab (matrix)**: A matrix recording the measurements of detected features (like OTUs, ASVs, genes) with features as rows and samples as columns. Ensure row names are the names of the features, and column names match the samples.
* **meta.dat (data.frame)**: A data frame of the sample meta data, where rows correspond to samples, and columns denote annotations.  Ensure the row names match the feature.tab columns. Tibbles are not permitted; use a standard R data frame.
* **feature.ann (matrix)**: An annotation matrix with features as rows and annotations as columns. Annotations are usually hierarchical such as the taxonomic annotations. Ensure row names match those in feature.tab.
* **tree (phylo, optional)**: Captures the evolutionary relationships among feautres. It's essential for specific beta-diversity calculations but isn't required for the majority of MicrobiomeStat analyses.
* **feature.agg.list (list, optional)**: Generated using the `mStat_aggregate_by_taxonomy` function, this list contains matrices of aggregated data based on the annotations specified in ”feature.ann", with each matrix's columns representing samples and rows representing the annotation level (e.g., Phylum, Genus).

Here is how to construct the MicrobiomeStat data object directly from matrix and data.frame:

### 1. Using Existing Data in R Environment

#### Starting from matrix and data.frame

Assuming we already have the count matrix and sample metadata:

```r
# Step 1: Create the sample names.
# We want 20 sample names, each prefixed with "S". We use the `paste0` function to concatenate "S" with the numbers 1 through 20.
samplenames <- paste0("S",1:20)

# Step 2: Create the feature names.
# We want 100 feature names, each prefixed with "OTU". We use the `paste0` function to concatenate "OTU" with the numbers 1 through 100.
featurenames <- paste0("OTU",1:100)

# Step 3: Create the Feature.tab matrix.
# We want a matrix with 100 rows and 20 columns, filled with random counts.
# We then set the row and column names of this matrix to the feature and sample names we created earlier.
Feature.tab <- matrix(sample(1:1000, 100*20, repl = TRUE),nrow=100,ncol=20)
rownames(Feature.tab) <- featurenames
colnames(Feature.tab) <- samplenames

# Step 4: Create the Meta.dat data frame.
# We want a data frame with two columns: "group" and "weight".
# The "group" column should have 10 "case"s followed by 10 "control"s.
# The "weight" column should have 20 random numbers from a normal distribution with mean 70 and standard deviation 5.
# We then set the row names of this data frame to the sample names we created earlier.
# Create the Meta.dat dataframe
Meta.dat <- data.frame(
  group = rep(c("control","case"), each=10),
  weight = rnorm(20,70,5)
)

# Convert the "group" column to a factor and set "control" as the reference level
Meta.dat$group <- relevel(as.factor(Meta.dat$group), ref="control")

rownames(Meta.dat) <- samplenames
```

#### Add Feature Annotation

To enrich the analysis, it's beneficial to add feature annotations. For this example, we add both taxonomic and functional classifications:

```r
# Step 1: Define the values for the "Kingdom" column. 
# We want 80 "Bacteria" and 20 "Archaea", so we use the `rep` function to repeat these values the desired number of times.
bacteria <- rep("Bacteria", 80)
archaea <- rep("Archaea", 20)
kingdom_values <- c(bacteria, archaea)

# Step 2: Define the values for the "Function" column. 
# We want a random selection from "Metabolism", "Signaling", and "Regulation", and we want 100 values in total.
# We use the `sample` function to randomly select these values, allowing for repeats (replace=TRUE).
function_values <- sample(c("Metabolism","Signaling","Regulation"), 100, replace=TRUE)

# Step 3: Combine the "Kingdom" and "Function" values into a single vector.
# We use the `c` function to concatenate these two vectors into one.
combined_values <- c(kingdom_values, function_values)

# Step 4: Get the number of rows from the `Feature.tab` data frame.
# We use the `nrow` function to get this number.
num_rows <- nrow(Feature.tab)

# Step 5: Get the row names from the `Feature.tab` data frame.
# We use the `rownames` function to get these names.
row_names <- rownames(Feature.tab)

# Step 6: Create the matrix.
# We use the `matrix` function to create the matrix, specifying the combined values, the number of rows, and the column and row names.
Feature.ann <- matrix(combined_values, nrow=num_rows, dimnames=list(row_names, c("Kingdom", "Function")))
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

#### Add Phylogenetic Tree

If you have a phylogenetic tree that you would like to use, you can upload it:

```r
# Load the ape package
library(ape)

# Read the tree file
# Replace 'path_to_your_tree_file' with the actual path to your tree file.
# The tree file should be in Newick format.
tree <- read.tree('path_to_your_tree_file')

MicrobiomeData <- list(
  feature.tab = as.matrix(Feature.tab),
  meta.dat = Meta.dat,
  feature.ann = as.matrix(Feature.ann),
  tree = tree
)
```

With either the provided sample data or your own structured data, you now have a complete MicrobiomeStat data object ready for subsequent analysis and visualization.
