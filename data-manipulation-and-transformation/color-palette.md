# Color Palette

## Custom Color Palette Retrieval

### mStat\_get\_palette Function

Choosing the right color palette is essential for creating visually appealing and informative data visualizations. The `mStat_get_palette()` function in MicrobiomeStat allows users to easily access and utilize a variety of color palettes, especially those inspired by and sourced from the scientific publication realm, through the `ggsci` package.

#### Overview

`mStat_get_palette()` is tailored for users who need predefined or custom color palettes for their data visualizations in R. It offers an extensive selection from the `ggsci` package, known for its scientific journal-inspired palettes, as well as the flexibility to specify custom color sets.

**Functionality:**

* Access predefined palettes like 'npg', 'aaas', 'nejm', 'lancet', 'jama', 'jco', and 'ucscgb' from the `ggsci` package.
* Provide a vector of custom color codes.
* Return a default palette in case of `NULL` input or unrecognized palette names.

#### Usage

```{r
# Default palette
mStat_get_palette()

# Predefined `ggsci` palette: 'nejm'
mStat_get_palette("nejm")

# Custom color vector
mStat_get_palette(c("#123456", "#654321"))
```

#### Parameters

* `palette`: A character string for predefined palettes or a vector of color codes. If `NULL` or unrecognized, a default palette is used.

#### Details

Here's a glimpse into the functioning of `mStat_get_palette()`:

* It first checks if the `palette` parameter is `NULL`, a character string, or a vector.
* For a character string, it verifies if the name matches one of the predefined palettes in the `ggsci` database.
* If a match is found, the corresponding palette is returned.
* For a vector input, the vector itself is returned as the palette.
* In case of `NULL` or an unrecognized input, a default set of colors is provided.

#### Applications

This function is highly beneficial for:

* Enhancing the visual appeal of data plots in scientific publications.
* Customizing color schemes to match specific publication or presentation themes.
* Ensuring consistency and accuracy of color representation in multiple visualizations.

#### Visual Representation

* npg

<figure><img src="../.gitbook/assets/Screenshot 2024-01-10 at 15.26.49.png" alt=""><figcaption></figcaption></figure>

* lancet

<figure><img src="../.gitbook/assets/Screenshot 2024-01-10 at 15.27.24.png" alt=""><figcaption></figcaption></figure>

* aaas

<figure><img src="../.gitbook/assets/Screenshot 2024-01-10 at 15.27.06.png" alt=""><figcaption></figcaption></figure>

* nejm

<figure><img src="../.gitbook/assets/Screenshot 2024-01-10 at 15.28.02.png" alt=""><figcaption></figcaption></figure>

* jama

<figure><img src="../.gitbook/assets/Screenshot 2024-01-10 at 15.28.41.png" alt=""><figcaption></figcaption></figure>

* jco

<figure><img src="../.gitbook/assets/Screenshot 2024-01-10 at 15.30.36.png" alt=""><figcaption></figcaption></figure>

* ucscgb

<figure><img src="../.gitbook/assets/Screenshot 2024-01-10 at 15.31.27.png" alt=""><figcaption></figcaption></figure>
