---
description: >-
  Metadata provides crucial sample information for microbiome analysis.
  Effective metadata management enables robust comparative analysis.
---

# Metadata Management

## Metadata Management

Metadata provides key sample information for integrated microbiome analysis. MicrobiomeStat offers functions to update and synchronize metadata.

### Update Metadata

The `mStat_update_meta_data()` function updates metadata from a file or dataframe:

```r
mStat_update_meta_data(
  data.obj = obj,
  map.file = "new_meta.csv",
  meta.sep = "\t"  # Use "," for CSV, "\t" for TSV (default)
)
```

**Parameters:**

* `data.obj`: The MicrobiomeStat data object to update.
* `map.file`: Path to the metadata file, or a data.frame.
* `meta.sep`: Column separator for the metadata file. Default is `"\t"` (tab-separated). Use `","` for CSV files.
* `quote`: Quote character used in the file. Default is `"\""`.
* `comment`: Comment character. Default is `""` (none).

It can handle CSVs, TSVs, etc.

**Steps include**:

* Load metadata file or dataframe
* Replace existing `meta.dat`
* Subset feature table by intersecting samples
* Return updated data object

This allows incorporating new metadata to enable more powerful integrated analyses.

### Update Sample Names

The `mStat_update_sample_name()` function updates sample names across data components:

```r
mStat_update_sample_name(
  data.obj = obj,
  new.name = c("s1", "s2", "s3")
)
```

It synchronizes sample names in `meta.dat`, `feature.tab`, and `feature.agg.list`.

**Key steps**:

* Check new names are valid
* Update `meta.dat` rownames
* Update `feature.agg.list` colnames
* Update `feature.tab` colnames

This maintains data consistency when updating sample identifiers.

Proper metadata management ensures high-quality integrated analyses. MicrobiomeStat empowers seamless metadata updates to enable robust microbiome discoveries.
