---
description: >-
  Data aggregation summarizes microbiome data at broader taxonomic levels. This
  reveals intuitive compositional patterns and trends.
---

# Data Aggregation

Data aggregation plays a vital role in simplifying and structuring microbiome data analysis. By summarizing data at broader taxonomic levels or by subject variables, users can gain clearer insights into compositional patterns within their datasets. MicrobiomeStat offers several functions for this purpose: `mStat_aggregate_data()`, `mStat_aggregate_by_taxonomy()`, and `mStat_aggregate_by_taxonomy2()`.

## 1. mStat\_aggregate\_data()

### Function Overview

The `mStat_aggregate_data()` function is designed to aggregate a microbiome dataset based on a specified subject variable. Additionally, users have the option to further stratify the aggregation through an additional variable.

```r
 # Prepare data for the function
data(peerj32.obj)

 # Call the function
aggregated_data <- mStat_aggregate_data(
  data.obj = peerj32.obj,
  subject.var = "subject",
  strata.var = NULL
)
```

Where:

* `data.obj`: Refers to the data object within MicrobiomeStat that needs to be aggregated.
* `subject.var`: Represents the primary variable for aggregation, such as a patient ID.
* `strata.var`: Acts as an optional stratification variable, allowing for more nuanced aggregation based on groups or categories.

In this example, we've prepared the data for our function using the `peerj32.obj` dataset. Next, we call the `mStat_aggregate_data()` function to aggregate the data based on the "subject" variable. We haven't provided any stratification variable (`strata.var`), so the function will aggregate solely based on the "subject" variable.

<figure><img src="../.gitbook/assets/Screenshot 2023-10-09 at 19.22.06.png" alt=""><figcaption><p><strong>Figure 1:</strong> Snapshot of the <code>feature.tab</code> component of the <code>peerj32.obj</code> dataset before aggregation. This table showcases the microbial feature abundances across multiple samples, illustrating the raw distribution and counts of different microbiome features in the original dataset.</p></figcaption></figure>

<figure><img src="../.gitbook/assets/Screenshot 2023-10-09 at 19.22.53.png" alt=""><figcaption><p><strong>Figure 2:</strong> Overview of the <code>meta.dat</code> component of the <code>peerj32.obj</code> dataset prior to aggregation. This table provides metadata information for each sample, including various attributes and characteristics that offer context and additional information about the samples in the dataset.</p></figcaption></figure>

After the aggregation, the data will be restructured and summarized based on the specified "subject" variable.

<figure><img src="../.gitbook/assets/Screenshot 2023-10-09 at 19.23.36.png" alt=""><figcaption><p><strong>Figure 3:</strong> Snapshot of the <code>feature.tab</code> component after applying the <code>mStat_aggregate_data()</code> function. The table now reflects aggregated microbial feature abundances based on the "subject" variable, summarizing the microbiome features for each unique subject in a consolidated manner.</p></figcaption></figure>

<figure><img src="../.gitbook/assets/Screenshot 2023-10-09 at 19.24.06.png" alt=""><figcaption><p><strong>Figure 4:</strong> Overview of the <code>meta.dat</code> component post-aggregation. The metadata table has been restructured and condensed to correspond with the aggregated samples in <code>feature.tab</code>, offering a streamlined view of sample attributes in the aggregated dataset.</p></figcaption></figure>

## Detailed Breakdown

The `mStat_aggregate_data()` function performs the following significant steps:

1. **Initialization and Message Communication:** The function starts by conveying to the user its intent to aggregate the data based on the provided criteria.
2. **Aggregation of `feature.agg.list`:** Before diving into the primary feature table aggregation, the function examines whether the `data.obj` contains a `feature.agg.list`. If present, this list contains feature abundance tables that need to be aggregated in a similar manner to the primary feature table.
3. **Main Feature Table Aggregation (`feature.tab`):** The microbiome feature abundances from `feature.tab` are grouped and summarized based on the provided `subject.var` and, if available, `strata.var`. This forms the crux of the aggregation process, effectively collapsing the dataset based on the selected variables.
4. **Metadata Aggregation (`meta.dat`):** The metadata accompanying the samples is equally crucial. The function groups and summarizes this data analogously, ensuring that the metadata remains consistent with the aggregated feature table.
5. **Other Data Elements:** The function retains other essential components of the `data.obj`, like the `tree` and `feature.ann`, ensuring the output remains comprehensive.
6. **Output Formation:** The function culminates by packaging all processed components into a new MicrobiomeStat data object, ready for further exploration and analysis.

## Practical Applications

The `mStat_aggregate_data()` function is indispensable for various microbiome data analysis ventures:

* **Simplifying Complex Data:** By conglomerating data based on subjects or other criteria, it renders dense microbiome tables more interpretable and manageable.
* **Harnessing Detailed Metadata:** The function's ability to integrate and process associated metadata means users can make better-informed decisions based on contextual information alongside microbial abundances.
* **Stratification and Comparative Analysis:** Should users provide a `strata.var`, the function creates a stratified view of the microbiome landscape, vital for dissecting differences among distinct groups or conditions.
* **Streamlined Longitudinal and Cohort Studies:** The aggregated view of the data, especially when based on subjects or time-points, is invaluable for long-term studies, ensuring patterns across time or multiple visits are discernible.

In essence, the `mStat_aggregate_data()` function is a robust tool, simplifying and restructuring microbiome datasets for clearer interpretation and deeper analytical dives.

## 2. mStat\_aggregate\_by\_taxonomy()

### Function Overview

The `mStat_aggregate_by_taxonomy()` function facilitates the aggregation of microbiome data based on specified taxonomic levels, such as "Phylum" or "Family". This function is primarily designed to populate the `feature.agg.list` within the `data.obj`. Through this, microbiome datasets can be represented in terms that are more biologically interpretable. It's noteworthy that if `feature.agg.list` is not generated prior to using certain other functions within the package, those functions will automatically invoke `mStat_aggregate_by_taxonomy()` to ensure the necessary aggregation is performed.

### Usage

```r
 data(peerj32.obj)

 # Specify the taxonomy level
 feature.level <- c("Phylum", "Family")

 # Aggregate data object by taxonomy level
 peerj32.obj <- mStat_aggregate_by_taxonomy(peerj32.obj, feature.level)
```

<figure><img src="../.gitbook/assets/Screenshot 2023-10-09 at 19.42.36.png" alt=""><figcaption><p>An illustrative representation of the <code>feature.agg.list</code> created by the <code>mStat_aggregate_by_taxonomy()</code> function, showcasing the aggregation results.</p></figcaption></figure>

### Detailed Breakdown

The `mStat_aggregate_by_taxonomy()` function undertakes the following steps:

1. **Verification:** It checks if the `feature.level` is provided.
2. **Data Joining:** Loads the OTU table (`feature.tab`) and taxonomy table (`feature.ann`) from the data object. It then joins these two tables based on row names, ensuring a unified dataset.
3. **Consistency Check:** Validates that the row names between the OTU and taxonomy tables are consistent. It provides messages if there are discrepancies.
4. **Taxonomic Aggregation:** Using the chosen taxonomic level, the function aggregates the data, transforming OTUs to a higher taxonomy level representation.
5. **List Update:** It updates or creates the `feature.agg.list` within the data object to include the aggregated tables for the specified taxonomic levels.

### Practical Applications

The `mStat_aggregate_by_taxonomy()` function proves beneficial in scenarios like:

* **Biological Interpretation:** The function aids in presenting data in a manner that’s biologically insightful by focusing on higher taxonomic ranks like Phylum or Family.
* **Data Simplification:** The microbiome OTU tables can be extremely detailed. By aggregating data based on taxonomy, the table's complexity is reduced, making it more manageable and comprehensible.
* **Consistent Data Representation:** By aligning the OTU and taxonomy tables, the function ensures the dataset's integrity and consistency. This is particularly crucial as mismatches can lead to erroneous interpretations or analytical challenges.

Overall, the `mStat_aggregate_by_taxonomy()` function serves as an indispensable tool for microbiome researchers, enabling them to transmute granular OTU-level data into a higher, more interpretable taxonomic hierarchy.

## 3. mStat\_aggregate\_by\_taxonomy2()

### Function Overview

Unlike its predecessor, the `mStat_aggregate_by_taxonomy2` function allows aggregation based on a single taxonomic level at a time. It focuses on merging the OTU table and the taxonomy table based on the provided taxonomy level.

### Usage

```r
feature.tab <- matrix(c(5, 10, 0, 7, 0, 20), nrow=3, byrow=TRUE)
rownames(feature.tab) <- c("OTU1", "OTU2", "OTU3")
colnames(feature.tab) <- c("Sample1", "Sample2")

feature.ann <- data.frame(Phylum = c("Bacteroidetes", "Firmicutes", "Bacteroidetes"))
rownames(feature.ann) <- c("OTU1", "OTU2", "OTU3")

result <- mStat_aggregate_by_taxonomy2(feature.tab, feature.ann, feature.level="Phylum")
```

### Detailed Breakdown

1. **Input Verification:** The function first ensures that a `feature.level` is provided and that it specifies a single taxonomy level.
2. **Consistency Checks:** Before proceeding with aggregation, it verifies that the row names of the OTU (`feature.tab`) and taxonomy (`feature.ann`) tables match.
3. **Data Merging:** It performs an inner join on the OTU and taxonomy tables. This step ensures only rows with consistent names across both tables are considered for aggregation.
4. **Aggregation:** Data is then aggregated based on the provided taxonomy level, with any missing taxonomy labels being replaced with the term "Unclassified".
5. **Final Cleanup:** Finally, rows in the aggregated table with all zeros (i.e., no counts) are removed.

### Practical Applications

* **Single-level Focus:** This function is perfect for users who wish to focus on a specific taxonomic level without any distractions.
* **Consistency Checks:** It ensures data integrity by comparing row names between the OTU and taxonomy tables, alerting users to discrepancies.
* **Data Reduction:** This function simplifies datasets by aggregating at the specified taxonomy level, making it more digestible for subsequent analyses.
