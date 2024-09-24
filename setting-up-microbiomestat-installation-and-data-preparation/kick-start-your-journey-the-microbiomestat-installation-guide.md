---
description: >-
---

# Installation Guide

This document will guide you through the installation process of the MicrobiomeStat package.

## Installation Options

### Option 1: Installation via CRAN

MicrobiomeStat is available on the CRAN repository. To install it, run the following command in your R console:

```R
install.packages("MicrobiomeStat")
```

{% hint style="info" %}
**Note:** The current version of MicrobiomeStat on CRAN supports only the `linda` and `linda.plot` functions. For a complete set of functionalities, especially for analyzing longitudinal data, consider installing the development version from GitHub.
{% endhint %}

### Option 2: Installation via GitHub (Development Version)

The development version of MicrobiomeStat is hosted on GitHub. To install it, you will first need the 'devtools' package. If you haven't installed it yet, you can do so using the command:

```R
install.packages("devtools")
```

After installing 'devtools', you can proceed to install MicrobiomeStat:

```R
devtools::install_github("cafferychen777/MicrobiomeStat")
```

### Checking the Installed Version

After installation, you can check the installed version of MicrobiomeStat to ensure that you have the latest version:

```R
packageVersion("MicrobiomeStat")
```

This should display the version number of MicrobiomeStat. For the development version installed from GitHub, the current version is 1.1.3. For the CRAN version, the current version is 1.1. Ensure that the displayed version number matches the expected version for the source you installed from (GitHub or CRAN).

## Dependencies

To ensure the proper functioning of MicrobiomeStat, ensure that you have the following packages installed:

### Required CRAN Packages

```R
# List of packages to install
packages_to_install <- c(
  "rlang",
  "tibble",
  "ggplot2",
  "matrixStats",
  "lmerTest",
  "foreach",
  "modeest",
  "dplyr",
  "pheatmap",
  "tidyr",
  "ggh4x",
  "GUniFrac",
  "stringr",
  "rmarkdown",
  "knitr",
  "pander",
  "tinytex",
  "vegan",
  "scales",
   "ape",          # Used only in mStat_convert_phyloseq_to_data_obj and mStat_import_mothur_as_data_obj
  "ggrepel",       # Used only in the linda.plot function
  "parallel",      # Utilized only when setting parallel in linda
  "ggprism",       # Active only when theme.choice is set to “prism”
  "aplot",         # Operates exclusively in generate_beta_ordination_single
  "yaml",          # Employed solely in mStat_import_qiime2_as_data_obj
  "biomformat",    # Used in mStat_import_biom_as_data_obj
  "Biostrings"     # Required for mStat_import_qiime2_as_data_obj and mStat_import_dada2_as_data_obj
)

# Installing packages
install.packages(packages_to_install)

```

{% hint style="info" %}
**Note:** Due to CRAN's restrictions, we couldn't list all the packages that `MicrobiomeStat` depends on as dependencies in our package documentation. However, for a smooth analysis process, it's imperative to have all these packages installed. Please ensure that you've installed all the packages listed above to prevent any disruptions during your analysis using `MicrobiomeStat`.
{% endhint %}

## Troubleshooting

Ensure that you are using the latest versions of R and Bioconductor. If you encounter any issues, it might be because a package requires a specific version of R or Bioconductor. Consult the documentation of the problematic package for guidance.

Thank you for choosing MicrobiomeStat.
