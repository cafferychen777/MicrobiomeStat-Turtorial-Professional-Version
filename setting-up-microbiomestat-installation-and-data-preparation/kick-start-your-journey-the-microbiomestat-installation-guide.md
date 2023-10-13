---
description: >-
  Start your microbiome data analysis with this guide. It provides instructions
  for installing MicrobiomeStat, setting you up for your exploration journey.
---

# Begin Your Journey: The MicrobiomeStat Installation Guide

## Objective: Install MicrobiomeStat

This document will guide you through the installation process of the MicrobiomeStat package.

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

### Dependencies

To ensure the proper functioning of MicrobiomeStat, ensure that you have the following packages installed:

**Required CRAN Packages**:

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
  "vegan",
  "dplyr",
  "pheatmap",
  "tidyr",
  "ggh4x",
  "ape",
  "GUniFrac",
  "scales",
  "stringr",
  "rmarkdown",
  "knitr",
  "pander",
  "tinytex",
  "ggrepel",
  "parallel",
  "ggprism",
  "aplot",
  "philentropy",
  "forcats",
  "yaml",
  "biomformat",
  "Biostrings"
)

# Installing packages
install.packages(packages_to_install)
```

{% hint style="info" %}
**Note:** Due to CRAN's restrictions, we couldn't list all the packages that `MicrobiomeStat` depends on as dependencies in our package documentation. However, for a smooth analysis process, it's imperative to have all these packages installed. Please ensure that you've installed all the packages listed above to prevent any disruptions during your analysis using `MicrobiomeStat`.
{% endhint %}

Ensure that you are using the latest versions of R and Bioconductor. If you encounter any issues, it might be because a package requires a specific version of R or Bioconductor. Consult the documentation of the problematic package for guidance.

Thank you for choosing MicrobiomeStat.
