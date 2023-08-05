---
description: >-
  Diving into alpha diversity analysis for longitudinal studies. How does
  within-sample diversity change over time? This page provides the tools to
  answer this question.
---

# Chronological Journey: Alpha Diversity Analysis in Longitudinal Studies

Our expedition into **Chronological Journey: Alpha Diversity Analysis in Longitudinal Studies** launches with the indispensable task of loading our _critical dataset_ and meticulously **filtering out samples with low sequencing depth**. The star of this journey is none other than **alpha diversity analysis**, a potent method celebrated for unraveling the captivating complexities of **species diversity within each sample**. However, beware! This analytical star is highly **sensitive to sequencing depth**, its notorious adversary. In particular, samples with **extremely low sequencing depth** (for instance, those deceitful ones below 10) can throw our understanding of **microbial diversity** into disarray, weaving distorted narratives and infusing our analysis with treacherous bias. To fend off these destructive forces, we utilize our filtering arsenal with utmost precision, ensuring they meet their well-deserved exit.&#x20;

```r
data(subset_T2D.obj)
```

Once we've prepared our dataset, we move on to calculate the alpha diversity, using Shannon diversity as our metric:

```r
# Calculate alpha diversity
T2D.alpha.obj <- mStat_calculate_alpha_diversity(subset_T2D.obj$feature.tab, "shannon")
```

As we embark on our Chronological Journey: Alpha Diversity Analysis in Longitudinal Studies, it is prudent to first grasp how to test for differences in alpha diversity across timepoints. MicrobiomeStat provides the `generate_alpha_test_long()` function, which utilizes a linear mixed model approach to detect changes in alpha diversity over time.

The linear mixed model incorporates a temporal component, accounting for the correlation between repeated measurements on the same subject. The function also accepts additional adjustment variables for greater modeling flexibility.

The output is a list of coefficient tables, one for each alpha diversity index. Each table includes the term, estimate, standard error, t-value, and p-value for each fixed effect in the model.

Here is an example usage:

```r
data("subset_T2D.obj") 
alpha_test_results <- generate_alpha_test_long(
  data.obj = subset_T2D.obj,
  alpha.obj = NULL,
  time.var = "visit_number",
  t0.level = sort(unique(subset_T2D.obj$meta.dat$visit_number))[1],
  ts.levels = sort(unique(subset_T2D.obj$meta.dat$visit_number))[2:6],
  alpha.name = c("shannon", "simpson"),
  subject.var = "subject_id",
  group.var = "subject_race",
  adj.vars = "subject_gender"
)
```

| Term                              | Estimate  | Std.Error | Statistic | P.Value  |
| --------------------------------- | --------- | --------- | --------- | -------- |
| (Intercept)                       | 3.03      | 0.215     | 14.1      | 3.86e-22 |
| visit\_number 2                   | -0.122    | 0.125     | -0.971    | 3.32e-1  |
| visit\_number 3                   | -0.150    | 0.129     | -1.17     | 2.44e-1  |
| visit\_number 4                   | -0.215    | 0.126     | -1.71     | 8.83e-2  |
| visit\_number 5                   | -0.0412   | 0.138     | -0.298    | 7.66e-1  |
| visit\_number 6                   | -0.0611   | 0.142     | -0.430    | 6.67e-1  |
| subject\_gendermale               | 0.219     | 0.100     | 2.19      | 3.22e-2  |
| subject\_genderunknown            | 0.625     | 0.414     | 1.51      | 1.33e-1  |
| subject\_raceasian                | -0.481    | 0.229     | -2.10     | 4.02e-2  |
| subject\_racecaucasian            | -0.306    | 0.208     | -1.47     | 1.47e-1  |
| subject\_raceethnic\_other        | -1.08     | 0.349     | -3.11     | 2.51e-3  |
| subject\_racehispanic\_or\_latino | -0.000394 | 0.290     | -0.00136  | 9.99e-1  |

| Term                              | Estimate | Std.Error | Statistic | P.Value  |
| --------------------------------- | -------- | --------- | --------- | -------- |
| (Intercept)                       | 0.840    | 0.0382    | 22.0      | 8.28e-30 |
| visit\_number 2                   | -0.0221  | 0.0177    | -1.25     | 2.13e-1  |
| visit\_number 3                   | -0.0243  | 0.0183    | -1.33     | 1.84e-1  |
| visit\_number 4                   | -0.0377  | 0.0179    | -2.11     | 3.55e-2  |
| visit\_number 5                   | -0.0178  | 0.0197    | -0.906    | 3.65e-1  |
| visit\_number 6                   | -0.0154  | 0.0202    | -0.763    | 4.46e-1  |
| subject\_gendermale               | 0.0394   | 0.0180    | 2.19      | 3.30e-2  |
| subject\_genderunknown            | 0.108    | 0.0676    | 1.60      | 1.12e-1  |
| subject\_raceasian                | -0.0566  | 0.0417    | -1.36     | 1.81e-1  |
| subject\_racecaucasian            | -0.0403  | 0.0381    | -1.06     | 2.94e-1  |
| subject\_raceethnic\_other        | -0.198   | 0.0600    | -3.30     | 1.45e-3  |
| subject\_racehispanic\_or\_latino | 0.0316   | 0.0539    | 0.587     | 5.60e-1  |

This allows us to test for differences in alpha diversity across timepoints and understand temporal trends. Next, we'll use a spaghetti plot to show how the alpha diversity changes over time for each individual in the study, grouped by race:

```r
# Generate a spaghetti plot of alpha diversity over time
generate_alpha_spaghettiplot_long(
  data.obj = subset_T2D.obj,
  alpha.obj = T2D.alpha.obj,
  alpha.name = c("shannon"),
  subject.var = "subject_id",
  time.var = "visit_number",
  t0.level = sort(unique(T2D.obj$meta.dat$visit_number))[1],
  ts.levels = sort(unique(T2D.obj$meta.dat$visit_number))[-1],
  group.var = "subject_race",
  strata.var = NULL,
  theme.choice = "bw",
  palette = ggsci::pal_npg()(9),
  pdf = TRUE,
  file.ann = NULL,
  pdf.wid = 11,
  pdf.hei = 8.5
)
```

<figure><img src="../.gitbook/assets/Screenshot 2023-06-13 at 21.18.16.png" alt=""><figcaption><p>Alpha Diversity Over Time Spaghetti Plot: Generated by the <code>generate_alpha_spaghettiplot_long()</code> function, this plot illustrates the temporal changes in the alpha diversity of each individual. It specifically displays Shannon diversity, a commonly used measure of species diversity, over time, with each line representing an individual in the study. Stratified by subject race, this plot provides insights into the impact of race on microbiome diversity trends over time, highlighting the complex interplay between demographic factors and microbiome dynamics.</p></figcaption></figure>

This plot, made up of lines and colors, represents the fascinating journey of our microbiome over time, influenced by various factors, including race. Stay tuned as we delve deeper into the complexities of microbiome analysis in the context of longitudinal studies!&#x20;

Embarking further on our **Chronological Journey: Alpha Diversity Analysis in Longitudinal Studies**, we now harness the power of boxplots to provide a distilled view of **alpha diversity over time**. This is achieved using the `generate_alpha_boxplot_long()` function. But, owing to the boxplot's structure, it isn't ideal for showcasing changes over a prolonged duration. Thus, to remedy this, we curate the time points exhibited via the `t0.level` and `ts.levels` parameters. Here's how:

```r
# Render a boxplot encapsulating alpha diversity across chosen time points
generate_alpha_boxplot_long(
  data.obj = subset_T2D.obj,
  alpha.obj = T2D.alpha.obj,
  alpha.name = c("shannon"),
  subject.var = "subject_id",
  time.var = "visit_number",
  t0.level = sort(unique(T2D.obj$meta.dat$visit_number))[1],
  ts.levels = sort(unique(T2D.obj$meta.dat$visit_number))[2:6],
  group.var = "subject_race",
  strata.var = NULL,
  base.size = 20,
  theme.choice = "bw",
  palette = ggsci::pal_npg()(9),
  pdf = TRUE,
  file.ann = NULL,
  pdf.wid = 20,
  pdf.hei = 8.5
)
```

<figure><img src="../.gitbook/assets/Screenshot 2023-06-13 at 21.45.06.png" alt=""><figcaption><p>Longitudinal Alpha Diversity Boxplot: Derived from the <code>generate_alpha_boxplot_long()</code> function, this plot efficiently compresses the temporal trends of Shannon diversity into a sequence of boxplots. Each box represents a distinct time point, and the spread of diversity within each time slot is succinctly portrayed. The median diversity is showcased by the line within each box, while the edges outline the first and third quartiles. This stratified depiction, segmented by subject race, enlightens us on the subtle, yet impactful influence of race on the shifts in microbiome diversity. The plot thus serves as a key that unlocks deeper understanding of the nuanced interplay between demographic variables and microbiome dynamics over time.</p></figcaption></figure>

This boxplot crystallizes the shifts in **Shannon diversity** over selected time points, displaying the **distribution of diversity** within each time point with precision. Every box signifies a time point, where the line within represents the **median diversity**, and the box's edges marking the first and third quartiles.

As we segment by **subject race**, this plot amplifies the impact of race on **microbiome diversity trends** over time. It's an exciting step in our journey, further unfurling the intriguing tapestry of the interplay between microbiome dynamics and race in longitudinal microbiome studies. Let's continue to decode these captivating complexities together!&#x20;
