---
description: >-
  High-quality analysis requires valid, well-formatted data. MicrobiomeStat
  provides data validation to check format compliance and consistency.
---

# Data Validation

## Data Validation

High-quality integrated analysis requires valid, well-formatted data. The `mStat_validate_data()` function checks and fixes MicrobiomeStat data objects.

```{r
mStat_validate_data(
  data.obj = obj
) 
```

It examines key components like `feature.tab`, `meta.dat`, `feature.ann` to ensure compliance with format rules.

**Key validation steps:**

* Check `data.obj` is a list
* Check `meta.dat` is a dataframe
* Match `feature.tab` and `feature.ann` row names
* Match `feature.tab` and `meta.dat` column names
* Additional checks if needed

If any rules fail, the function will attempt to fix the inconsistencies.

**Benefits include:**

* Ensure suitability for analysis functions
* Avoid cryptic errors from format issues
* Maintain data integrity across operations
* Validate new objects from various sources

**Robust microbiome discoveries require properly validated data.** `mStat_validate_data()` provides an automated way to catch and adjust format inconsistencies.
