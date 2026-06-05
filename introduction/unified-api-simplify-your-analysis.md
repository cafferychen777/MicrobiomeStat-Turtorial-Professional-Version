---
description: >-
  Use 6 unified functions to perform all analyses, with automatic study design
  detection.
---

# Unified API: Simplify Your Analysis

Starting from version 1.5.0, MicrobiomeStat provides a **unified API** with 6 core functions that automatically detect your study design (cross-sectional, paired, or longitudinal) and route to the appropriate underlying functions. This significantly simplifies the user experience — instead of memorizing dozens of function-specific names, you can use a consistent interface for all analyses.

## Overview

| Function | Purpose | Replaces |
|----------|---------|----------|
| `plot_taxa()` | Taxonomic visualization | `generate_taxa_barplot_single()`, `generate_taxa_boxplot_long()`, `generate_taxa_heatmap_pair()`, etc. |
| `plot_alpha()` | Alpha diversity visualization | `generate_alpha_boxplot_single()`, `generate_alpha_spaghettiplot_long()`, etc. |
| `plot_beta()` | Beta diversity visualization | `generate_beta_ordination_single()`, `generate_beta_pc_boxplot_long()`, etc. |
| `test_taxa()` | Feature-level statistical testing | `generate_taxa_test_single()`, `generate_taxa_trend_test_long()`, etc. |
| `test_alpha()` | Alpha diversity statistical testing | `generate_alpha_test_single()`, `generate_alpha_trend_test_long()`, etc. |
| `test_beta()` | Beta diversity statistical testing | `generate_beta_test_single()`, `generate_beta_trend_test_long()`, etc. |

## How Study Design Detection Works

The unified functions automatically determine your study design based on the parameters you provide:

* **Cross-sectional (single time point)**: When `time.var = NULL` or a single time point is specified
* **Paired samples**: When `time.points` contains exactly 2 values (baseline + follow-up)
* **Longitudinal**: When `time.points` contains more than 2 values, or multiple time points are present in the data

## Visualization Functions

### plot\_taxa()

One function for all taxonomic visualizations.

```r
data(peerj32.obj)

# Cross-sectional barplot
plot_taxa(peerj32.obj, "barplot",
          group.var = "group",
          feature.level = "Phylum",
          feature.dat.type = "count")

# Paired heatmap
plot_taxa(peerj32.obj, "heatmap",
          subject.var = "subject",
          time.var = "time",
          time.points = c("1", "2"),
          group.var = "group",
          feature.level = "Genus",
          feature.dat.type = "count")

# Longitudinal boxplot with relative change
plot_taxa(peerj32.obj, "boxplot",
          subject.var = "subject",
          time.var = "time",
          group.var = "group",
          feature.level = "Family",
          feature.dat.type = "count",
          change.type = "relative")
```

**Supported plot types**: `"barplot"`, `"boxplot"`, `"heatmap"`, `"dotplot"`, `"areaplot"` (longitudinal only), `"spaghettiplot"` (longitudinal only), `"cladogram"` (single time point only).

### plot\_alpha()

One function for all alpha diversity visualizations.

```r
# Cross-sectional alpha boxplot
plot_alpha(peerj32.obj, "boxplot",
           alpha.name = c("shannon", "observed_species"),
           group.var = "group")

# Longitudinal spaghettiplot
plot_alpha(peerj32.obj, "spaghettiplot",
           subject.var = "subject",
           time.var = "time",
           alpha.name = "shannon",
           group.var = "group")
```

**Supported plot types**: `"boxplot"`, `"spaghettiplot"` (longitudinal), `"dotplot"` (longitudinal).

### plot\_beta()

One function for all beta diversity visualizations.

```r
# Cross-sectional ordination
plot_beta(peerj32.obj, "ordination",
          dist.name = c("BC", "Jaccard"),
          group.var = "group")

# Longitudinal ordination
plot_beta(peerj32.obj, "ordination",
          subject.var = "subject",
          time.var = "time",
          dist.name = "BC",
          group.var = "group")
```

**Supported plot types**: `"ordination"`, `"boxplot"`, `"spaghettiplot"`, `"dotplot"`.

## Statistical Testing Functions

### test\_taxa()

One function for all feature-level statistical tests.

```r
# Cross-sectional group comparison
test_taxa(peerj32.obj, "difference",
          group.var = "group",
          feature.level = "Genus",
          feature.dat.type = "count")

# Longitudinal trend test
test_taxa(peerj32.obj, "trend",
          subject.var = "subject",
          time.var = "time",
          group.var = "group",
          feature.level = "Genus",
          feature.dat.type = "count")
```

**Supported test types**: `"difference"` (group comparisons), `"trend"` (temporal trends, longitudinal), `"volatility"` (temporal instability, longitudinal), `"association"` (continuous variable associations), `"per_time"` (test at each time point separately).

### test\_alpha()

One function for all alpha diversity statistical tests.

```r
# Cross-sectional test
test_alpha(peerj32.obj, "difference",
           alpha.name = c("shannon", "simpson"),
           group.var = "group")

# Longitudinal trend test
test_alpha(peerj32.obj, "trend",
           alpha.name = "shannon",
           subject.var = "subject",
           time.var = "time",
           group.var = "group")
```

**Supported test types**: `"difference"`, `"trend"`, `"volatility"`, `"per_time"`.

### test\_beta()

One function for all beta diversity statistical tests (PERMANOVA, etc.).

```r
# Cross-sectional PERMANOVA
test_beta(peerj32.obj, "difference",
          dist.name = c("BC", "Jaccard"),
          group.var = "group")
```

**Supported test types**: `"difference"`, `"trend"`, `"volatility"`.

## Key Parameters

Common parameters shared across the unified functions:

* `data.obj`: MicrobiomeStat data object
* `subject.var`: Subject/sample ID variable (required for paired/longitudinal)
* `time.var`: Time variable (NULL for cross-sectional)
* `group.var`: Grouping variable (e.g., treatment, condition)
* `strata.var`: Stratification variable for faceting
* `time.points`: Time point specification — controls design detection:
  * `NULL`: use all available time points (auto-detect)
  * Single value: cross-sectional at that time point
  * Vector of 2: paired design (baseline, follow-up)
  * Vector of >2: longitudinal with specific time points
* `change.type`: For paired/longitudinal comparisons — `"none"` (raw values), `"relative"`, `"log_fold"`, or `"absolute"`
* `feature.dat.type`: Data type — `"count"`, `"proportion"`, or `"other"`
* `theme`: Plot theme — `"bw"`, `"classic"`, `"gray"`, or `"prism"`

## When to Use Unified vs. Specific Functions

The unified API is recommended for:
* **Quick exploratory analysis** — fewer function names to remember
* **Switching between study designs** — same function, just change parameters
* **Teaching and tutorials** — simpler mental model

The specific functions (e.g., `generate_taxa_barplot_single()`) are still available and recommended when:
* You need fine-grained control over function-specific parameters
* You are building automated pipelines that call specific functions
* You want to be explicit about which function is invoked

Both approaches produce identical results — the unified functions simply route to the specific functions internally.
