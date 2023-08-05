---
description: >-
  Start your microbiome data analysis with this guide. It provides instructions
  for installing MicrobiomeStat, setting you up for your exploration journey.
---

# Kick-start Your Journey: The MicrobiomeStat Installation Guide

**Target: Install MicrobiomeStat**&#x20;

You're only a **few steps away** from having **MicrobiomeStat** up and running on your system!

**Option 1: CRAN Installation** Our package has comfortably nestled in the **CRAN repository**. Just input the command below in your R console and it will land safely onto your system:

```R
install.packages("MicrobiomeStat")
```

**Option 2: GitHub Installation (Development version)** Fancy using the **freshest, just-out-of-the-oven version**? We've got you covered! Our development version is sitting on GitHub. First, you'll need the '**devtools**' package. If it's not already part of your toolkit, get it with this command:

```R
install.packages("devtools")
```

Once 'devtools' is ready, summon the **MicrobiomeStat** from GitHub with this incantation:

```R
devtools::install_github("cafferychen777/MicrobiomeStat")
```

**Checklist: Dependencies** To ensure a **hiccup-free journey**, check that you've got these companions along:

**CRAN Companions**:

```R
install.packages(c("ggplot2", "matrixStats", "Matrix", "parallel", "ggrepel", "lmerTest", "foreach", "modeest", "vegan", "dplyr", "ggprism", "pheatmap", "tidyr", "ggh4x", "rlang", "tibble", "ape", "GUniFrac", "philentropy", "broom", "rmarkdown", "knitr", "aplot", "scales", "forcats", "pander", "tinytex"))
```

**Bioconductor Buddies**: First, bring aboard the '**BiocManager**' from CRAN:

```R
if (!requireNamespace("BiocManager", quietly = TRUE))
    install.packages("BiocManager")
```

Then, call in '**phyloseq**' from Bioconductor:

```R
BiocManager::install(c("phyloseq"))
```

With these commands, you're signing up all the **required pals** from CRAN and Bioconductor. Just make sure you're operating the latest versions of R and Bioconductor for a smooth sail. If the winds seem stormy, it could be due to a package asking for a specific R or Bioconductor version. In that case, refer to the individual package's map (documentation) for the right direction.

**Happy journey with MicrobiomeStat!**&#x20;
