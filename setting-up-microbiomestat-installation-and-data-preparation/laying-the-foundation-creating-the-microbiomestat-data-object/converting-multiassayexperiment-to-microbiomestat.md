# Converting MultiAssayExperiment Data into MicrobiomeStat

If your data is stored in a `MultiAssayExperiment` object (commonly used in multi-omics studies), the `mStat_convert_MultiAssayExperiment_to_data_obj()` function converts a specified experiment to the MicrobiomeStat data format.

## Function Overview

```r
mStat_convert_MultiAssayExperiment_to_data_obj(mae.obj, experiment_name = NULL)
```

**Parameters:**

* `mae.obj`: A `MultiAssayExperiment` object to be converted.
* `experiment_name`: A string specifying which experiment in the `MultiAssayExperiment` to convert. Default is the first experiment.

**Returns:** A MicrobiomeStat data object (a list) containing:

* `feature.tab`: A matrix of assay data (features as rows, samples as columns)
* `meta.dat`: A data frame of sample metadata (from colData)

## Example Usage

```r
library(MultiAssayExperiment)

# Assuming 'mae' is your MultiAssayExperiment object
# Convert the "16S" experiment to MicrobiomeStat format
data.obj <- mStat_convert_MultiAssayExperiment_to_data_obj(mae, "16S")

# Or convert the first experiment (default)
data.obj <- mStat_convert_MultiAssayExperiment_to_data_obj(mae)

# Verify the conversion
print(dim(data.obj$feature.tab))
print(head(data.obj$meta.dat))
```

## Notes

* Only features with a sum > 0 across all samples are retained during conversion.
* The function requires the `MultiAssayExperiment` package to be installed.
* After conversion, validate the data object with `mStat_validate_data(data.obj)`.
