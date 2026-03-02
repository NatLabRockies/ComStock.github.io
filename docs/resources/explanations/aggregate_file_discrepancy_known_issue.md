---
layout: default
title: Metadata and Annual Results Aggregate File Discrepancy in 2024 Release 2
parent: Resources
nav_order: 9
---

# Issue Report: Metadata and Annual Results Aggregate File Discrepancy in 2024 Release 2

## Status
Impacts ComStock 2024 Release 2 only. We are actively working to address this issue in future dataset releases.

## Details
In ComStock 2024 Release 2, the "metadata_and_annual_results_aggregates" files hosted on the Open Energy Data Initiative (OEDI) data lake are impacted by a known consistency issue. Specifically, the sum of the aggregated metadata and results files at different geographic levels—such as county and state—to a national level may produce slightly different totals. For example, summing energy consumption results in the county-level aggregate files for all counties in the U.S. yields national totals that differ by up to &plusmn;5% from those in the state-level aggregate files.

This discrepancy stems from a minor error during the process in which we generate a sample weight table to appropriately weight and reallocate the ComStock samples based on the stock truth data (see the [New ComStock Sampling Method]({{  site.baseurl  }}{% link docs/resources/explanations/new_sampling_method.md %}) explanation and [ComStock reference documentation](https://natlabrockies.github.io/ComStock.github.io/assets/files/comstock_reference_documentation_2024_2.pdf) for details). In 2024 Release 2, this weight table was generated using a non-deterministic method, meaning that it can produce slightly different—but still valid—results when rerun. Some of the “metadata_and_annual_results_aggregates” files were unintentionally generated using a different version of this weight table, resulting in small inconsistencies between geographic aggregations.

The following groups of geographies in the “metadata_and_annual_results_aggregates” directory used the same weight table and therefore have identical results:

**Group A**
-	by_state_and_county
-	by_state_and_puma

**Group B**
-	by_state
-	national

The results in the aggregate files in Group A also match the raw, tract-level results in the “metadata_and_annual_results” directory. Neither group of files are considered more “correct” than the other. The difference in total, national energy consumption between the two groups is 2%. The total floor area between the two groups is identical because the weighting and reallocation process matches the total floor area in the stock truth data.

The issue will be addressed in the next dataset release.