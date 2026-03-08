---
layout: default
title: Resources
nav_order: 3
has_children: false
has_toc: false
---

# Resources
{: .fw-500 }

This section provides tutorials, how-to guides, explanations and reference material designed to aid users in answering questions about the ComStock dataset.

[Tutorials](#tutorials){: .btn .btn-uo .white-text}
[How-to Guides](#how-to-guides){: .btn .btn-uo .white-text}
[Explanations](#general){: .btn .btn-uo .white-text}
[ComStock Limitations](#comstock-limitations){: .btn .btn-uo .white-text}
[Known Issues](#known-issues){: .btn .btn-uo .white-text}
[Training Videos](#training-videos){: .btn .btn-uo .white-text}
[Tableau Dashboards](#tableau-dashboards){: .btn .btn-uo .white-text}
[Reference Documenation](#references){: .btn .btn-uo .white-text}

## Tutorials
This section provides lessons for understanding certain capabilities and functions of ComStock, as well as for learning a specific analysis skill.

- [Joining data from an external dataset to ComStock using geospatial fields]({{  site.baseurl  }}{% link docs/resources/tutorials/join_geospatial_data.md %})
- [Perform an analysis by blending ComStock and local data]({{  site.baseurl  }}{% link docs/resources/tutorials/local_segmentation_study.md %})

## How-to Guides
This section provides a collection of step-by-step guides for using the ComStock dataset to answer a given question. 

- [Access the ComStock datasets programmatically]({{  site.baseurl  }}{% link docs/resources/how_to_guides/example_scripts.md %})<span class="label label-green">UPDATE</span>
- [Filter the building characteristics dashboard for a county]({{  site.baseurl  }}{% link docs/resources/how_to_guides/characteristics_dashboard.md %})
- [Perform a basic commercial building stock segmentation analysis in Excel]({{  site.baseurl  }}{% link docs/resources/how_to_guides/basic_stock_characterization_workbook.md %})
- [Perform a commercial building stock characterization and upgrades impact analysis in Excel]({{  site.baseurl  }}{% link docs/resources/how_to_guides/impact_workbook.md %})<span class="label label-blue">NEW</span>
- [Understand the annual energy use by building type for a city]({{  site.baseurl  }}{% link docs/resources/how_to_guides/puma_level_analysis.md %})

## Explanations
These documents provide explanations focusing on the *how* and *why* of various parts of the ComStock data sets. While this section does not provide explicit advice on how to achieve a specific outcome, the documentation here will help users understand specific and important aspects the data sets. If while using ComStock there are aspects of the data sets that seem counterintuitive and / or confusing, please [email us](mailto:ComStock@nlr.gov) to recommend an additional piece of explanation documentation.

### General
- [Building Type Crosswalks]({{  site.baseurl  }}{% link docs/resources/explanations/building_type_crosswalks.md %})
- [Considerations for ComStock Calibration, Validation, and Uncertainty]({{  site.baseurl  }}{% link docs/resources/explanations/comstock_calibration.md %})
- [Geographic Fields and Codes]({{  site.baseurl  }}{% link docs/resources/explanations/reference_geographic_codes.md %})
- [New ComStock Sampling Method]({{  site.baseurl  }}{% link docs/resources/explanations/new_sampling_method.md %})
- [Sampling and Weighting in ComStock]({{  site.baseurl  }}{% link docs/resources/explanations/sampling_and_weighting.md %})
- [Using ComStock to Analyze Cost]({{  site.baseurl  }}{% link docs/resources/explanations/costing_analysis.md %})
- [Why Individual ComStock Measure Results Should Not Be Combined]({{  site.baseurl  }}{% link docs/resources/explanations/combining_measure_results.md %})
- [Commercial Building Load Components Data and Analysis]({{  site.baseurl  }}{% link docs/resources/explanations/component_loads.md %})<span class="label label-blue">NEW</span>

### ComStock Limitations
- [Building Types Not Included in ComStock]({{  site.baseurl  }}{% link docs/resources/explanations/building_types_not_included.md %})<span class="label label-green">UPDATE</span>
- [Gas Consumption Underrepresented]({{  site.baseurl  }}{% link docs/resources/explanations/gas_consumption_underrepresented.md %})
- [Sample Size Considerations]({{  site.baseurl  }}{% link docs/resources/explanations/sample_size_considerations.md %})

### Known Issues
- [California Models Known Issues]({{    site.baseurl   }}{% link docs/resources/explanations/california_known_issues.md %})
- [Utility Bill and Emissions in 2024 Release 2]({{  site.baseurl  }}{% link docs/resources/explanations/utility_bills_emissions_known_issue.md %})
- [Metadata and Annual Results Aggregate File Discrepancy in 2024 Release 2]({{  site.baseurl  }}{% link docs/resources/explanations/aggregate_file_discrepancy_known_issue.md %})
- [Geothermal Heat Pump and Demand Flexibility Package Naming in 2025 Release 2]({{  site.baseurl  }}{% link docs/resources/explanations/2025_release_2_packages_known_issue.md %})<span class="label label-blue">NEW</span>
- [Hawaii AMY2012 Weather Files in ComStock 2025 Release 2]({{  site.baseurl  }}{% link docs/resources/explanations/2025_release_2_hi_known_issue.md %})<span class="label label-blue">NEW</span>
- [California Service Water Heating in ComStock 2025 Release 3]({{  site.baseurl  }}{% link docs/resources/explanations/2025_release_3_ca_swh_known_issue.md %})<span class="label label-blue">NEW</span>

## Training Videos
Webinars, presentations, and guidance on the ComStock and ResStock datasets—including training videos on accessing the datasets, using the Data Viewer, and more—are available on [NLR’s Building Stock Analysis YouTube channel](https://www.youtube.com/playlist?list=PLmIn8Hncs7bEYCZiHaoPSovoBrRGR-tRS). See below for a sample of available videos. For the full collection, visit the YouTube channel.
-   [End-Use Load Profiles Dataset Access Demonstration](https://www.youtube.com/watch?v=iS7KeVQ0Bvs)
-   [Loading End-Use Saving Shapes Data into AWS Athena](https://www.youtube.com/watch?v=qSR1MFpSiro&list=PLmIn8Hncs7bEYCZiHaoPSovoBrRGR-tRS&index=4&t=2s)

## Tableau Dashboards
The [ComStock Tableau Public page](https://public.tableau.com/app/profile/comstock.nrel/vizzes) offers interactive visualizations derived from the public ComStock datasets. These visualizations provide insights into building characteristics, energy consumption patterns, and the potential impacts of energy efficiency measures across various building types and geographic regions. See below for a sample of available dashboards. Additional dashboards will be posted to the ComStock Tableau Public page as they become available.

-   [ComStock Building Characteristics Dashboard](https://public.tableau.com/app/profile/comstock.nrel/viz/ComStockBuildingCharacteristicsDashboard/Introduction)
-   [Evaluating Geothermal Performance for the U.S. Building Stock](https://public.tableau.com/app/profile/comstock.nrel/viz/EvaluatingGeothermalPerformancefortheU_S_BuildingStock/Home)
-   [Building Stock Segmentation for Retrofit Planning](https://public.tableau.com/app/profile/comstock.nrel/viz/BuildingStockSegmentationforRetrofitPlanning/USMap)

For ResStock Tableau dashboards, please visit the [ResStock Tableau Public page](https://public.tableau.com/app/profile/nrel.buildingstock/vizzes).

## References
These documents describe various aspects of ComStock, including the baseline and upgrade model documentation, as well as geographic clustering methodology.

<details markdown="block" class="level1-collapse-section" open>
<summary><b>ComStock Reference Documentation</b></summary>
This document serves as a guide and resource for the methodology and assumptions behind ComStock. The reference documentation will be updated as major changes to the baseline models are incorporated.

| Reference Documentation Version               | Release Date | Corresponding ComStock Dataset Release(s)                            |
|-----------------------------------------------|--------------|----------------------------------------------------------------------|
| [ComStock Reference Documentation: 2025 Release 3]({{  site.baseurl  }}{% link assets/files/comstock_reference_documentation_2025_3.pdf %}) | Nov. 2025    | 2025/comstock_amy2018_release_3
| [ComStock Reference Documentation: 2025 Release 2]({{  site.baseurl  }}{% link assets/files/comstock_reference_documentation_2025_2.pdf %}) | August 2025  | 2025/comstock_amy2018_release_2<br>2025/comstock_amy2012_release_2
| [ComStock Reference Documentation: 2025 Release 1]({{  site.baseurl  }}{% link assets/files/comstock_reference_documentation_2025_1.pdf %}) | June 2025    | 2025/comstock_amy2018_release_1 
| [ComStock Reference Documentation: 2024 Release 2]({{  site.baseurl  }}{% link assets/files/comstock_reference_documentation_2024_2.pdf %}) | Feb. 2025    | 2024/comstock_amy2018_release_2
| [ComStock Reference Documentation: 2024 Release 1]({{  site.baseurl  }}{% link assets/files/comstock_reference_documentation_2024_1.pdf %}) | May 2024     | 2024/comstock_amy2018_release_1
| [ComStock Reference Documentation: Version 1](https://www.nrel.gov/docs/fy23osti/83819.pdf)                                                 | March 2023   | 2023/comstock_amy2018_release_1<br>2023/comstock_amy2018_release_2   |

</details>


<details markdown="block" class="level1-collapse-section">
<summary><b>Upgrade Measures</b></summary>
The measure documentation describes the modeling methodology, assumptions, relevant ComStock baseline features, and observations from results.

[**Upgrade Measures**]({{  site.baseurl  }}{% link docs/upgrade_measures/upgrade_measures.md %})

</details>


<details markdown="block" class="level1-collapse-section">
<summary><b>Geographic Clustering Reference Documentation</b></summary>
These documents provide reference documentation for the clustering methodology developed by ComStock. The clustering algorithm described in this technical report resulted in 88 clusters across the United States. The clusters are used as the geographic basis for the “U.S. Building Stock Segmentation Series” published by DOE’s Building Technologies Office. This series will provide geographically relevant insight into building stock characteristics, energy and emissions performance, and, eventually, common end use technologies. The cluster definitions file maps counties to building stock segmentation clusters.

[**Building Stock Segmentation Cluster Development**](https://www.nrel.gov/docs/fy23osti/84648.pdf)

**June 2023**

[**Building Stock Segmentation Cluster Definitions**](https://oedi-data-lake.s3.amazonaws.com/nrel-pds-building-stock/end-use-load-profiles-for-us-building-stock/2023/comstock_amy2018_release_1/geographic_information/stock_cluster_definition_2023.11.29.csv)

**July 2023**

</details>

<details markdown="block" class="level1-collapse-section">
<summary><b>Building Stock Segmentation</b></summary>
This document discusses the development of a segmentation approach for the U.S. commercial building stock that focuses on identifying similarities. The resulting nine-segment approach primarily uses similarities in heating, ventilating, and air-conditioning systems, service water heating
systems, and the presence of cooking equipment to separate buildings into categories.

[**Commercial Building Stock Segmentation**](https://www.nrel.gov/docs/fy24osti/88947.pdf)

**May 2024**

</details>

## User Engagement Material

- [BuildStock Listening Session]({{  site.baseurl  }}{% link assets/files/20250924_BuildStock Listening Session.pdf %})