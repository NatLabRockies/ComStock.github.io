---
layout: default
title: ComStock Hospital Modeling
parent: Resources
nav_order: 2
---

# ComStock Hospital Modeling

## Summary
Hospital models in ComStock are unevenly distributed across the United States (see figure, below). For example, some densely populated counties, such as those containing Chicago and New Orleans, have no hospital models at all, while several low-population states like Wyoming have a disproportionately high number. This stems from gaps in the input data used to determine where hospitals are located and how models are sampled from those locations. This limitation affects all ComStock dataset releases.

{:refdef: style="text-align: center;"}
![](../../../assets/images/hospital_model_distribution.svg)
{:refdef}

## Source of the Limitation
This issue originates in the input data used to determine where hospitals are located and how models are sampled from those locations. The data source has gaps that result in hospitals being over-represented in some regions and absent in others.

## Analysis Recommendations
When conducting state- or county-level analysis, users should independently verify the expected number and size of hospitals in the area of interest and manually rescale energy modeling results to match. See the [Sample Size Considerations]({{  site.baseurl  }}{% link docs/resources/explanations/sample_size_considerations.md %}) explanation for guidance.

This step is especially critical for analyses focused specifically on hospitals, where the distributional error will have a large effect on results.

## Future Work
We are actively working to resolve this limitation and expect a fix to be included in the next standard dataset release. The planned fix involves incorporating a more representative input dataset to improve hospital representation across all regions of ComStock.
