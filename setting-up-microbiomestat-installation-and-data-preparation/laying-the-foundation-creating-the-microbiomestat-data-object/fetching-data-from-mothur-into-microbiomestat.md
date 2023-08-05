---
description: Acquire the knowledge to import data from Mothur into MicrobiomeStat.
---

# Fetching Data from Mothur into MicrobiomeStat

Here's how you can sail your Mothur data into MicrobiomeStat!

```r
# Example code (Replace with your real file paths!)
path_to_list_file <- "/path_to_your_files/your_list_file.list"
path_to_group_file <- "/path_to_your_files/your_group_file.groups"
path_to_tree_file <- "/path_to_your_files/your_tree_file.tree"
path_to_shared_file <- "/path_to_your_files/your_shared_file.shared"

# Convert Mothur data into a MicrobiomeStat data object
data_obj <- mStat_import_mothur_as_data_obj(mothur_list_file = path_to_list_file,
                                            mothur_group_file = path_to_group_file,
                                            mothur_tree_file = path_to_tree_file,
                                            mothur_shared_file = path_to_shared_file
                                            )
```

The `mStat_import_mothur_as_data_obj` function is your captain in navigating the sea of Mothur data and steering it towards the shores of MicrobiomeStat. Here's your navigational chart:

* **mothur\_list\_file** (Optional): Your list file from Mothur.
* **mothur\_group\_file** (Optional): Your group file from Mothur.
* **mothur\_tree\_file** (Optional): Your tree file from Mothur.
* **mothur\_shared\_file** (Optional): Your shared file from Mothur.
* **mothur\_constaxonomy\_file** (Optional): Your constaxonomy file from Mothur.
* **parseFunction** (Optional): Your custom function to parse the taxonomy. 'parse\_taxonomy\_default' can be a good starting point!

With your Mothur data on board, the `mStat_import_mothur_as_data_obj` function will chart a course and set sail for MicrobiomeStat! The resulting **MicrobiomeStat data object** is ready for the treasure trove of **MicrobiomeStat's statistical analysis tools**.

Embark on your exciting exploration now! Your journey through the fascinating world of microbiome data analysis is about to commence!&#x20;
