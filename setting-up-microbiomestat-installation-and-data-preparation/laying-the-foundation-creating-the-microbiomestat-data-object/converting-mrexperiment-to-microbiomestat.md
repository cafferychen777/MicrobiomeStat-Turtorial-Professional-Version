# Converting MRExperiment Data into MicrobiomeStat

If your data is stored in a `MRExperiment` object from the **metagenomeSeq** package, the `mStat_convert_MRExperiment_to_data_obj()` function provides a direct conversion to the MicrobiomeStat data format.

## Function Overview

```r
mStat_convert_MRExperiment_to_data_obj(mr.obj)
```

**Parameters:**

* `mr.obj`: A `MRExperiment` object to be converted.

**Returns:** A MicrobiomeStat data object (a list) containing:

* `feature.tab`: A matrix of expression data (features as rows, samples as columns)
* `meta.dat`: A data frame of sample metadata
* `feature.ann`: A matrix of feature annotations (if available)

## Example Usage

```r
library(metagenomeSeq)

# Load example data from metagenomeSeq
data(mouseData)

# Convert to MicrobiomeStat format
data.obj <- mStat_convert_MRExperiment_to_data_obj(mouseData)

# Verify the conversion
print(dim(data.obj$feature.tab))
print(head(data.obj$meta.dat))
```

## Notes

* Only features with a sum > 0 across all samples are retained during conversion.
* The function requires the `metagenomeSeq` package to be installed.
* After conversion, validate the data object with `mStat_validate_data(data.obj)`.
