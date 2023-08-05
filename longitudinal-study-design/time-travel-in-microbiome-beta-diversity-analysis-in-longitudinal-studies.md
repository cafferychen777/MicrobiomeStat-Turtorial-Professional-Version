---
description: >-
  Focusing on beta diversity analysis, this page explains how to visualize and
  quantify changes in microbial community composition over time across different
  samples.
---

# Time Travel in Microbiome: Beta Diversity Analysis in Longitudinal Studies

Emerging from the temporal mists, we arrive at our next destination - **illuminating shifts in beta diversity over time using `generate_beta_test_long()`**. This powerful function performs pairwise PERMANOVA tests, comparing each time point to the baseline visit. The returned p-values **quantify significant differences**, acting as guideposts to changing microbial landscapes.

Let's traverse this rugged terrain:

```{r
# Perform pairwise beta diversity tests using PERMANOVA
beta_test_long_results <- generate_beta_test_long(
  data.obj = subset_T2D.obj,
  dist.obj = NULL, 
  time.var = "visit_number",
  t0.level = sort(unique(subset_T2D.obj$meta.dat$visit_number))[1],
  ts.levels = sort(unique(subset_T2D.obj$meta.dat$visit_number))[2:6],
  subject.var = "subject_id",
  group.var = "subject_race",
  adj.vars = "subject_gender",
  dist.name = c('BC', 'Jaccard')
)
```



The returned p-values are signposts guiding us to diverging microbial landscapes across time. **Low p-values indicate significant differences** compared to baseline, intimating dynamic change.

Now let's **visualize these temporal journeys using `generate_beta_pc_boxplot_long()`**. This function traces individual trajectories across ordination Axes, connecting the dots from one timepoint to the next.

```{r
generate_beta_pc_boxplot_long(
  data.obj = ecam.obj,
  dist.obj = NULL,
  pc.obj = NULL,
  subject.var = "studyid",
  time.var = "month",
  t0.level = "0",
  ts.levels = as.character(sort(as.numeric(unique(ecam.obj$meta.dat$month))))[2],
  group.var = "diet", 
  strata.var = "delivery",
  dist.name = c('BC'),
  base.size = 20,
  theme.choice = "bw",
  custom.theme = NULL,
  palette = NULL,
  pdf = TRUE,
  file.ann = "test",
  pdf.wid = 11,
  pdf.hei = 8.5
)
```

The resulting plot maps each subject's unique path across the ordination landscape, **telling the story of their microbiome's evolution**. By connecting the dots from one timepoint to the next, we trace out individual trajectories of change.

Now let's glimpse the big picture using `generate_beta_ordination_long()`. This paints a landscape where samples cluster based on similarity, **illuminating global patterns**.

```{r
generate_beta_ordination_long(
  data.obj = subset_T2D.obj,
  dist.obj = NULL,  
  pc.obj = NULL,
  subject.var = "subject_id",
  time.var = "visit_number",
  t0.level = sort(unique(subset_T2D.obj$meta.dat$visit_number))[1],
  ts.levels = sort(unique(subset_T2D.obj$meta.dat$visit_number))[2:6],
  group.var = "subject_race",
  strata.var = "subject_gender",
  dist.name = c("BC"),
  base.size = 16,
  theme.choice = "bw",
  custom.theme = NULL,
  palette = NULL,
  pdf = TRUE,
  file.ann = NULL,
  pdf.wid = 11,
  pdf.hei = 8.5  
)
```

The resulting ordination plot visualizes the relationships between samples, with **proximity indicating similarity**. We observe intricate patterns as samples shift across the terrain over time.

Finally, `generate_beta_change_spaghettiplot_long()` **connects the dots for each subject**, tracing individual trajectories of microbial change.

```{r
generate_beta_change_spaghettiplot_long(
  data.obj = ecam.obj,
  dist.obj = NULL,
  subject.var = "studyid",
  time.var = "month", 
  t0.level = NULL,
  ts.levels = NULL,
  group.var = "diet",
  strata.var = NULL,
  dist.name = c("BC"),
  base.size = 20,
  theme.choice = "bw",
  palette = NULL,
  pdf = TRUE,
  file.ann = "test",
  pdf.wid = 11, 
  pdf.hei = 8.5
)
```

The resulting spaghetti plot **visualizes each subject's journey through time**, elucidating the unique twists and turns of their microbial narrative.

And with that, we conclude our expedition through the dimensional shifts of beta diversity over time. From statistical tests to ordination plots, MicrobiomeStat furnished an illuminating travelogue of this intricate terrain. What an adventure!
