---
layout: default
title: Demand Control Ventilation
parent: Upgrade Measures
nav_order: 10
published: false
---

# Demand Control Ventilation
{: .fw-500 }

Authors: Amy LeBar and Chris CaraDonna

#  Executive Summary

Building on the successfully completed effort to calibrate and validate the U.S. Department of Energy's ResStock™ and ComStock™ models over the past 3 years, the objective of this work is to produce national data sets that empower analysts working for federal, state, utility, city, and manufacturer stakeholders to answer a broad range of analysis questions.

The goal of this work is to develop energy efficiency, electrification, and demand flexibility end-use load shapes (electricity, gas, propane, or fuel oil) that cover a majority of the high-impact, market-ready (or nearly market-ready) measures. "Measures" refers to energy efficiency variables that can be applied to buildings during modeling.

An *end-use savings shape* is the difference in energy consumption between a baseline building and a building with an energy efficiency, electrification, or demand flexibility measure applied. It results in a time-series profile that is broken down by end use and fuel (electricity or on-site gas, propane, or fuel oil use) at each time step.

ComStock is a highly granular, bottom-up model that uses multiple data sources, statistical sampling methods, and advanced building energy simulations to estimate the annual subhourly energy consumption of the commercial building stock across the United States. The baseline model intends to represent the U.S. commercial building stock as it existed in 2018. The methodology and results of the baseline model are discussed in the final technical report of the [End-Use Load Profiles](https://www.nlr.gov/buildings/end-use-load-profiles.html) project.

This documentation focuses on a single end-use savings shape measure---demand control ventilation (DCV). DCV can save energy by reducing the rate at which outdoor air is delivered during periods of less-than-design occupancy. This measure will enable DCV for air loops using applicable HVAC system types (all except dedicated outdoor air systems \[DOAS\], packaged systems, or that have an energy recovery ventilator \[ERV\]) and serving applicable space types (all except kitchens, dining areas, patient spaces, mechanical rooms, stairwells and corridors, or high exhaust space types) using model occupancy schedules to control the DCV. The measure is applicable to approximately 73% of the stock floor area. As office buildings in ComStock are modeled using a single, whole-building space type, DCV is not applied to these building types.

The DCV measure demonstrates 2.6% total site energy savings (119 trillion British thermal units \[TBtu\]) for the U.S. commercial building stock modeled in ComStock (Figure 1). The savings are primarily attributed to:

-   **8.8%** stock **heating gas** savings (72.5 TBtu)

-   **9.3%** stock **heating electricity** savings (18.3 TBtu)

-   **2.1%** stock **cooling electricity** savings (15.2 TBtu)

-   **0.1%** stock **fan electricity** savings (0.7 TBtu).

The DCV measure demonstrates between 2.0 and 3.8 million metric tons (MMT CO<sub>2</sub>e) of greenhouse gas emissions avoided for the three grid electricity scenarios presented, as well as 4.9 MMT CO<sub>2</sub>e of greenhouse gas emissions avoided for on-site natural gas consumption.

# 1. Introduction

This documentation covers "demand control ventilation" upgrade methodology and briefly discusses key results. Results can be accessed on the ComStock™ data lake at "[end-use-load-profiles-for-us-building-stock](https://data.openei.org/s3_viewer?bucket=oedi-data-lake&prefix=nrel-pds-building-stock%2Fend-use-load-profiles-for-us-building-stock%2F)" or via the Data Viewer at [comstock.nlr.gov.](https://comstock.nlr.gov/datasets)

| **Measure Title**      | Demand Control Ventilation                                                                                                                                   |
| **Measure Definition** | This measure applies DCV to applicable air loops in a model. Ventilation rate varies based on the occupancy schedules in the model.        |
| **Applicability**      | This measure is applicable to all air loops, except those that serve the space types and use HVAC system types listed below.         |
| **Not Applicable**     | This measure is not applicable to the following building types: Hotels, Restaurants. This measure is not applicable the following space types: Kitchens, Dining space types, Healthcare patient room space types, Laboratory space types. This measure is also not applicable to models with the following HVAC system types:	Dedicated outdoor air system (DOAS),	Packaged terminal air-conditioner (PTAC),	Packaged terminal heat pump (PTHP),	Energy recovery ventilator (ERV).        |                                                                                                                      
| **Release**            | 2023 Release 2: 2023/comstock_amy2018_release_2/                                                                                                                                                                                                                                                                                                                       |

#  2. Technology Summary

Commercial buildings typically have a fixed outdoor air flow rate designed to provide the required ventilation to the building when at peak conditions (maximum occupancy, high number of pollutants, etc.). While this worst-case approach aims to always maintain acceptable indoor air quality, it often results in over-ventilation during a large portion of building operation, as peak conditions are infrequently encountered. In turn, this results in wasted HVAC energy used to condition the not-needed ventilation air, as the system is treating more outside air than necessary during non-peak conditions. For example, consider a large conference room in an office building. When a meeting is occurring and the space is at a high occupant level, the design outdoor air flow rate determined by the peak condition will satisfy the needs of the room. However, when the meeting concludes and the conference room is empty, the HVAC system will over-ventilate the space and waste energy conditioning outdoor air that is in excess of what is needed to maintain acceptable indoor air quality.

Demand control ventilation (DCV) is a feedback control method that, when properly implemented, can reduce the energy used by HVAC systems while at the same time maintaining indoor air quality by modulating the ventilation rates to match the needs of the building. DCV reduces the rate at which outdoor air is delivered during periods of less-than-design occupancy. There are several DCV technologies available (occupancy schedules, people counters, occupancy sensors, CO<sub>2</sub> sensors), but CO<sub>2</sub> sensors are the most prevalent. These sensors monitor the CO<sub>2</sub> levels in a space to infer occupancy and control outside air rates. Ventilation rates will decrease when the part per million levels of CO<sub>2</sub> fall below the set threshold, indicating there are fewer people in the space. These sensors can be located on the wall of critical zones, or in the return air ducts. Duct sensors are effective if the sensor is serving large open spaces where concentration levels are largely uniform (e.g., lecture halls, theaters, classrooms), but are not suitable for ducts serving multiple spaces with varying levels of occupancy. Critical zones that may have highly variable levels should have their own sensors (typically wall-mounted).

While CO<sub>2</sub> sensors are the most common technology used for DCV, ComStock implements DCV using occupancy schedules. Occupant generation of CO<sub>2</sub> is not included in the ComStock baseline and therefore cannot be used to apply DCV. Additionally, ComStock does not capture behavior of buildings with CO<sub>2</sub> sensors in return air ducts because it uses occupancy schedules at the space level.

This measure will apply DCV to air loops in models that meet the applicability criteria and do not already have it.

# 3. ComStock Baseline Approach

DCV is required in ASHRAE 90.1 (2007--present), IECC (2009--present), and California Title 24 (1996--present) energy codes (see most recent requirements in Table 2). DCV is included in ComStock models when required by the governing ASHRAE 90.1 energy code for the specific spaces/systems in the model. ComStock gathers the necessary criteria for determining DCV requirements and includes DCV functionality only if the space/system requires. Requirement criteria for DCV includes space floor area, space design occupant density, system economizer prevalence, system design outdoor air flow rate, and system energy recovery prevalence. The 90.1 code year for a model is based on the year of the model's last major HVAC replacement. A summary of the floor area served by a system with DCV is shown in Table 1. Note that DCV is not required by ASHRAE 90.1 when an HVAC system has an energy recovery ventilator (ERV).

Table 1. Fraction of Floor Area Controlled by HVAC System with DCV by Building Type and Code Year

<table><thead><tr class="header"><th>Building Type</th><th><blockquote><p>DOE Ref Pre-1980</p></blockquote></th><th><blockquote><p>DOE Ref 1980-2004</p></blockquote></th><th><blockquote><p>90.1-2004</p></blockquote></th><th><blockquote><p>90.1-2007</p></blockquote></th><th><blockquote><p>90.1-2010</p></blockquote></th><th><blockquote><p>90.1-2013</p></blockquote></th><th><blockquote><p>DEER Pre-1975</p></blockquote></th><th><blockquote><p>DEER 1985</p></blockquote></th><th><blockquote><p>DEER 1996</p></blockquote></th><th><blockquote><p>DEER 2003</p></blockquote></th><th><blockquote><p>DEER 2007</p></blockquote></th><th><blockquote><p>DEER 2011</p></blockquote></th><th><blockquote><p>DEER 2014</p></blockquote></th><th><blockquote><p>DEER 2015</p></blockquote></th><th><blockquote><p>DEER 2017</p></blockquote></th></tr></thead><tbody><tr class="odd"><td><p>FullService</p><p>Restaurant</p></td><td>0</td><td>0</td><td>0</td><td>0.263</td><td>0.091</td><td>0.197</td><td>0</td><td>0</td><td>0</td><td>0.559</td><td>0.561</td><td>0.567</td><td>0.541</td><td>0.568</td><td>0.549</td></tr><tr class="even"><td>Hospital</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0.387</td><td>0.423</td><td>0.376</td><td>0.154</td><td>0.226</td><td>0.389</td></tr><tr class="odd"><td>LargeHotel</td><td>0</td><td>0</td><td>0</td><td>0.009</td><td>0.005</td><td>0.023</td><td>0</td><td>0</td><td>0</td><td>0.073</td><td>0.067</td><td>0.051</td><td>0.125</td><td>0.059</td><td>0.143</td></tr><tr class="even"><td>LargeOffice</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td></tr><tr class="odd"><td>MediumOffice</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td></tr><tr class="even"><td>Outpatient</td><td>0</td><td>0</td><td>0</td><td>0.048</td><td>0.015</td><td>0.011</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td></tr><tr class="odd"><td>PrimarySchool</td><td>0</td><td>0</td><td>0</td><td>0.507</td><td>0.023</td><td>0.033</td><td>0</td><td>0</td><td>0</td><td>0.825</td><td>0.806</td><td>0.789</td><td>0.773</td><td>0.823</td><td>0.715</td></tr><tr class="even"><td>QuickService Restaurant</td><td>0</td><td>0</td><td>0</td><td>0.017</td><td>0.008</td><td>0.033</td><td>0</td><td>0</td><td>0</td><td>0.387</td><td>0.388</td><td>0.351</td><td>0.349</td><td>0.366</td><td>0.362</td></tr><tr class="odd"><td>RetailStandalone</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td></tr><tr class="even"><td>RetailStripmall</td><td>0</td><td>0</td><td>0</td><td>0.204</td><td>0.036</td><td>0.025</td><td>0</td><td>0</td><td>0</td><td>0.185</td><td>0.169</td><td>0.163</td><td>0.165</td><td>0.168</td><td>0.113</td></tr><tr class="odd"><td>SecondarySchool</td><td>0</td><td>0</td><td>0.040</td><td>0.512</td><td>0.008</td><td>0.022</td><td>0</td><td>0</td><td>0</td><td>0.772</td><td>0.693</td><td>0.756</td><td>0.830</td><td>0.809</td><td>0.662</td></tr><tr class="even"><td>SmallHotel</td><td>0</td><td>0</td><td>0</td><td>0.002</td><td>0</td><td>0.000</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td></tr><tr class="odd"><td>SmallOffice</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td></tr><tr class="even"><td>Warehouse</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td></tr></tbody></table>

One important observation from these data is that no office buildings include DCV in the ComStock baseline. This is because office buildings are currently modeled using a single, blended space type that is a fractional mix of open offices, enclosed offices, conference rooms, etc. and the occupancy density of this blended space does not exceed the DCV thresholds in ASHARE 90.1. This leads to unrealistically low (0%) DCV in office buildings in the baseline building stock, which may overestimate the savings for stock-wide DCV implementation.

For more details about how DCV is modeled in the ComStock baseline, see the ComStock Reference Documentation \[1\].

Table 2. DCV in Building Energy Codes

![Chart, bar chart Description automatically generated](media\dcv_image21.png)

A_<sub>Z</sub> = Zone area

P_<sub>Z</sub> = People in zone

V_<sub>ot</sub> = Design outdoor airflow

Occ<sub>design</sub> = Design occupancy

# 4. Modeling Approach

## 4.1. Applicability

This measure is applicable to 72.7% of commercial stock floor area.

### 4.1.1. Building Type

Hotels and restaurants will not be modified by the measure. Hotels typically have either single zone unitary systems (e.g., PTAC, PTHP) or dedicated outdoor air systems (DOAS) meeting HVAC needs. Neither is suitable for DCV (see details in HVAC System Type section). Air loops that serve a space type without a code-defined per-person ventilation rate will not have this measure applied (see more details under Methodology). Due to high concentration of heat and effluents, kitchens and restaurants often require the design outdoor air flow rate all day to maintain indoor air quality and should not have the outdoor air flow reduced at periods of lower occupancy.

### 4.1.2. Space Type

There are certain space and building types for which DCV is not appropriate and will be skipped in this measure. The following space types will not be modified:

-   Kitchens

-   Dining

-   Laboratories

-   Healthcare patient spaces

-   Corridors and stairwells

-   Mechanical rooms

-   Data centers

-   High exhaust spaces, such as restrooms and locker rooms

For the reasons listed in the "Building Type" applicability section, kitchens in non-restaurant buildings will not have DCV applied. Dining areas in these buildings will also not have DCV applied because these areas frequently use transfer air from kitchens.

In hospitals, proper ventilation and indoor air quality must be maintained to ensure infection control and patient safety. For this reason, hospital space types dealing with patient care will not have DCV applied (e.g., patient, radiology, exam, and surgical rooms). Non-patient space types such as offices will have DCV applied. Additionally, laboratory space types found in hospitals will not have DCV applied per energy code guidance.

### 4.1.3. HVAC System Type

Regardless of space and building type, any model with a DOAS will not have DCV applied. A DOAS is generally a damperless system and would require significant retrofit to adjust the outdoor air flow to accommodate DCV. Models with single-zone unitary systems (e.g., PTAC, PTHP) will not have DCV applied because they are entirely reliant on occupants to manually control the ventilation air and are unable to be controlled using occupancy or CO<sub>2</sub> sensors. Following ASHRAE 90.1 guidelines, models with energy recovery ventilators (ERVs) will also not have DCV applied.

## 4.2. Methodology

This measure will cycle through the outdoor air loops in a model and enable DCV for air loops serving applicable space types using the EnergyPlus® function "air_loop_hvac_enable_demand_control_ventilation." This will set the DCV field in the mechanical ventilation controller to "Yes" from "No." As ComStock building models do not track CO<sub>2</sub> levels, the models' occupancy schedules will be used to control the DCV.

### 4.2.1. Outdoor Air

For the EnergyPlus DCV function to work properly, a space needs both a per-person and per-area outdoor air rate specified. In OpenStudio® Standards, some space types have either 100% per-person or 100% per-area outdoor air rates specified. For these spaces on applicable air loops, outdoor air rates are converted to a per-person rate of 10 CFM/person, and the remainder of the outdoor air requirement is assigned as per-area.

Air loops that serve both applicable and inapplicable space types will have DCV applied but will only function in spaces that are applicable. To do this, inapplicable space types will have their outdoor air rates converted to 100% per-area to avoid triggering the EnergyPlus function.

## 4.2.2. Greenhouse Gas Emissions

Three electricity grid scenarios are presented to compare the emissions of the ComStock baseline and the DCV scenario. The choice of grid scenario will impact the grid emissions factors used in the simulation, which determines the corresponding emissions produced per kilowatt-hour. Two scenarios---Long-Run Marginal Emissions Rate (LRMER) High Renewable Energy (RE) Cost 15-Year and LRMER Low RE Cost 15-Year---use the Cambium data set, and the last uses the eGrid data set \[2\], \[3\]. All three scenarios vary the emissions factors geospatially to reflect the variation in grid resources used to produce electricity across the United States. The Cambium data sets also vary emissions factors seasonally and by time of day. This study does not imply a preference for any particular grid emissions scenario, but other analysis suggests that the choice of grid emissions scenario can impact results \[4\]. Emissions due to on-site combustion of fossil fuels use the emissions factors shown in Table 3, which are from Table 7.1.2(1) of draft American National Standards Institute/Residential Energy Services Network/International Code Council 301 \[5\]. To compare total emissions due to both on-site fossil fuel consumption and grid electricity generation, the emissions from a single electricity grid scenario should be combined with all three on-site fossil fuel emissions.

Table 3. On-Site Fossil Fuel Emissions Factors 

  | **Natural gas **                                                                         |   147.3 lb/MMBtu (228.0 kg/MWh)<sup>a</sup> |
  | **Propane **                                                                             |   177.8 lb/MMBtu (182.3 kg/MWh)    |
  | **Fuel oil **                                                                            |   195.9 lb/MMBtu (303.2 kg/MWh)    |

  <sup>a</sup> lb = pound; MMBtu = million British thermal units; kg = kilogram; MWh = megawatt-hour   

## 4.2.3. Limitations and Concerns

There is one notable model group excluded from ComStock's baseline DCV application: offices. We expect to see especially high energy savings from this measure in this group. It is important to note that these savings numbers are likely overestimates, as many of the actual buildings in this group use DCV.

# 5. Output Variables

Table 6 includes a list of output variables that are calculated in ComStock. These variables are important in terms of understanding the differences between buildings with and without the DCV measure applied. These output variables can also be used for understanding the economics of the upgrade (e.g., return on investment) if cost information (i.e., material, labor, and maintenance costs for technology implementation) is available.

Table 6. Output Variables Calculated From the Measure Application

  | **Variable Name** | **Description** |
  |------------------- | ------------------------------------------------------------- |
  | Initial condition | Number of air loops with DCV before the measure is applied. |
  | Final condition | Number of air loops with DCV after the measure is applied. |

# 6. Results

In this section, results are presented both at the stock level and for individual buildings through savings distributions. Stock-level results include the combined impact of all the analyzed buildings in ComStock, including buildings that are not applicable to this measure. Therefore, they do not necessarily represent the energy savings of a particular or average building. Stock-level results should not be interpreted as the savings that a building might realize by implementing the DCV upgrade.

Total site energy savings are also presented in this section. Total site energy savings can be a useful metric, especially for quality assurance/quality control, but this metric on its own can have limitations for drawing conclusions. Further context should be considered, as site energy savings alone do not necessarily translate proportionally to savings for a particular fuel type (e.g., gas or electricity), source energy savings, cost savings, or greenhouse gas savings. This is especially important when a measure impacts multiple fuel types or causes decreased consumption of one fuel type and increased consumption of another. Many factors should be considered when analyzing the impact of an energy efficiency or electrification strategy, depending on the use case.

## 6.1. Single Building Example

The operational behavior of an example strip mall model in Georgia is described in this section. The model has 27 air loops, none of which had DCV prior to the DCV measure being applied. The air loops serve strip mall and full-service restaurant space types. Based on the applicability criteria of this measure, we would expect full-service restaurant spaces (which are included in some ComStock strip malls), such as kitchens and dining areas, to be marked as ineligible space types and not have DCV applied.

Following the application of the measure, 18 of the 27 air loops were eligible for DCV. The 9 that were ineligible were those that served full-service restaurant spaces exclusively. Three of the spaces served by the 18 eligible air loops did not have a per-occupant outdoor air requirement in OpenStudio Standards. The measure split the outdoor air requirements for these spaces between per-occupant and per-area rates using the 10 CFM/occupant minimum. After the split, the per-occupant rate was set to 10 CFM/occupant, and the remaining outdoor air requirement was met by the per-area rate. The total outdoor air requirement before and after the measure were the same, indicating that the split was executed correctly. Additionally, we would expect the outdoor air rate to decrease proportionally to occupancy in the spaces. Figure 1 shows three days of outdoor air mass flow rate and occupant count in the model, confirming that the outdoor air flow decreases with decreased occupant count. In general, the DCV measure functions as expected.

{:refdef: style="text-align: center;"}
![Chart, bar chart Description automatically generated](media\dcv_image2.png)
{:refdef}

{:refdef: style="text-align: center;"}
Figure 1. Air system outdoor air rate (right, orange) and zone occupancy (left, blue) over three days
{:refdef}

{:refdef: style="text-align: center;"}
All report figures by the National Laboratory of the Rockies.
{:refdef}

## 6.2. Applicability

Figure 2 shows the DCV measure applicability by building type and total stock weighted floor area. This measure is applicable to 72.7% of the stock floor area. As expected, the measure is not being applied to restaurants or hotels.

{:refdef: style="text-align: center;"}
![Chart, waterfall chart Description automatically generated](media\dcv_image3.png)
{:refdef}

{:refdef: style="text-align: center;"}
Figure 2. Applicability by building type and stock weighted floor area of applicability
{:refdef}

## 6.3. Stock Energy Impacts

The DCV measure demonstrates 2.6% total site energy savings (119 trillion British thermal units \[TBtu\]) for the U.S. commercial building stock modeled in ComStock (Figure 3). The savings are primarily attributed to:

-   **8.8%** stock **heating gas** savings (72.5 TBtu)

-   **9.3%** stock **heating electricity** savings (18.3 TBtu)

-   **2.1%** stock **cooling electricity** savings (15.2 TBtu)

-   **0.1%** stock **fan electricity** savings (0.7 TBtu).

The heating and cooling end uses show the highest savings and are attributable to reduced heating/cooling loads due to the outdoor air reductions from the DCV. During periods of reduced occupancy, the DCV enables the outdoor air flow rate to reduce, which decreases the amount of outdoor air to condition. Note that in models without economizers (in ComStock, these are models that did not require DCV by code), DCV may reduce outdoor air during periods when it is beneficial for cooling, thus increasing the need for mechanical cooling at times. At the stock level, ComStock results show an overall reduction in cooling because of implementing DCV.

{:refdef: style="text-align: center;"}
![Chart, bar chart Description automatically generated](media\dcv_image4.jpeg)
{:refdef}

{:refdef: style="text-align: center;"}
Figure 3. Comparison of annual site energy consumption between the ComStock baseline and the DCV measure scenario. Energy consumption is categorized both by fuel type and end use.
{:refdef}

## 6.4. Stock Greenhouse Gas Emissions Impact

ComStock simulation results show greenhouse gas emissions avoided across all electricity grid scenarios and on-site combustion fuel types (Figure 4).

{:refdef: style="text-align: center;"}
![](media\dcv_image5.png)
{:refdef}

{:refdef: style="text-align: center;"}
Figure 4. Greenhouse gas emissions comparison of the ComStock baseline and the DCV scenario
{:refdef}

Three electricity grid scenarios are presented: Cambium Long-Run Marginal Emissions Rate (LRMER) High Renewable Energy (RE) Cost 15-Year, Cambium LRMER Low RE Cost 15-Year, and eGrid. MMT stands for million metric tons. GHG stands for greenhouse gas.

For the combined impact across all sources, a single electricity grid scenario should be chosen and combined with all three on-site combustion fuel scenarios. Greenhouse gas emissions avoided from the electricity grid are between 1.1% and 1.2%, depending on the scenario. This is due to reduced electricity consumption from the fan and cooling end uses. The 5.9%, 8.3%, and 10% emissions avoided from on-site combustion of natural gas, fuel oil, and propane, respectively, are attributable to reducing the amount of outdoor air required to be conditioned, which reduces heating loads.

## 6.5. Site Energy Savings Distributions

As noted at the beginning of this section, here we discuss site energy consumption for quality assurance/quality control purposes. Site energy savings can be useful for these purposes, but other factors should be considered when drawing conclusions, as these do not necessarily translate proportionally to source energy savings, greenhouse gas emissions avoided, or energy cost.

Figure 5 shows the percent savings distributions of the baseline ComStock models versus the DCV scenario by end use and fuel type for applicable models. Most HVAC end uses show savings from this measure, including heating end uses (electricity, natural gas, other fuel, district heating), district cooling, and fan electricity. DCV lowers ventilation rates during periods of less-than-design occupancy, which reduces the amount of outdoor air required to be conditioned, and results in a decrease in energy use by HVAC systems. This explains the savings for these end uses.

{:refdef: style="text-align: center;"}
![Diagram Description automatically generated](media\dcv_image6.jpeg)
{:refdef}

{:refdef: style="text-align: center;"}
Figure 5. Percent site energy savings distribution for ComStock models with the DCV measure applied by end use and fuel type
{:refdef}

{:refdef: style="text-align: center;"}
The data points that appear above some of the distributions indicate outliers in the distribution, meaning they fall outside 1.5 times the interquartile range. The value for *n* indicates the number of ComStock models that were applicable for energy savings for the fuel type category.
{:refdef}

### 6.5.1. Heating Penalties

Some models in these end uses also saw penalties, particularly in the natural gas heating end use. We would not expect DCV to result in negative natural gas heating savings. Further investigation of these models shows that most are VAV systems located in warm climates and therefore have low heating loads (Figure 6). For example, one of these models had -100% natural gas heating savings. The baseline annual natural gas heating consumption was 2 kWh. After the DCV measure was applied, this increased to 5 kWh, which results in the -100% percent savings. 2 kWh to 5 kWh is not a significant increase and can skew the percent-savings distribution. Furthermore, the design outdoor air rates in these models remained the same between the baseline and upgrade model, but the average outdoor air fraction decreases, indicating the DCV is properly reducing operational outdoor air without impacting the design outdoor air rates. When considering the energy use intensity (EUI) savings distribution (Figure 7), natural gas heating shows practically all energy savings from the DCV measure being applied.

{:refdef: style="text-align: center;"}
![Chart, box and whisker chart Description automatically generated](media\dcv_image7.png)
{:refdef}

{:refdef: style="text-align: center;"}
Figure 6. Natural gas heating savings distributions by ASHRAE 2006 climate zone
{:refdef}

{:refdef: style="text-align: center;"}
![A picture containing diagram Description automatically generated](media\dcv_image8.jpeg)
{:refdef}

{:refdef: style="text-align: center;"}
Figure 7. Site EUI savings distribution for ComStock models with the DCV measure applied by end use and fuel type
{:refdef}

{:refdef: style="text-align: center;"}
The data points that appear above some of the distributions indicate outliers in the distribution, meaning they fall outside 1.5 times the interquartile range. The value for *n* indicates the number of ComStock models that were applicable for energy savings for the fuel type category.
{:refdef}

Some models also saw electric heating penalties (Figure 5), which is unexpected with this measure. As with the models with natural gas heating penalties, most of these models are in warm climates (Figure 8) and therefore have low heating loads. One of these models, a retail building in climate zone 2B with a mixed fuel VAV system, had a 20% electric heating penalty. After the DCV upgrade, almost all gas heating was removed at the air-handling unit, and the model incurred a higher zone reheat electric penalty. The natural gas heating savings were far greater than the electric heating penalty, resulting in net heating savings, which is the case for several other models as well. These models also show no change in their design outdoor air rates between the baseline and upgrade model, and the average outdoor air fraction is reduced, indicating the DCV measure is being applied correctly.

{:refdef: style="text-align: center;"}
![Chart, box and whisker chart Description automatically generated](media\dcv_image9.png)
{:refdef}

{:refdef: style="text-align: center;"}
Figure 8. Electricity heating savings distributions by ASHRAE 2006 climate zone
{:refdef}

### 6.5.2. Cooling Penalties

Additionally, Figure 4 shows that several models with the DCV measure applied have electricity cooling penalties. Most of these models do not have economizers (Figure 9). The cooling penalties in these no-economizer models make sense because DCV will reduce outdoor air rates during periods of low occupancy regardless of whether an air loop is in cooling mode and bringing in design outdoor air would be beneficial to cool the air delivered to the zones. This results in higher cooling energy because the system is cooling the return air rather than benefiting from cooler outdoor air. Cooling penalties in some models with economizers can also be explained using this logic. Figure 10 shows that only 37% of models with economizers and cooling penalties have economizers on all their air loops. The remaining 63% have economizers on a fraction of the air loops, and the no-economizer loops are losing the benefit of cool outdoor air while in cooling mode.

{:refdef: style="text-align: center;"}
![Chart, scatter chart Description automatically generated](media\dcv_image10.png)
{:refdef}

{:refdef: style="text-align: center;"}
Figure 9. Distribution of electricity cooling penalties by economizer type
{:refdef}

{:refdef: style="text-align: center;"}
![Chart, histogram Description automatically generated](media\dcv_image11.png)
{:refdef}

{:refdef: style="text-align: center;"}
Figure 10. Fraction of air loops in a model with economizers for models with electricity cooling penalties and fixed dry bulb, differential dry bulb, or differential enthalpy economizers
{:refdef}

82 of 289 (28%) models have economizers on all air loops.

For models with economizers on all air loops, we would expect the DCV measure to not affect the cooling energy consumption as the economizer will override the DCV when it is advantageous to bring in 100% outdoor air. Most of these models are in Very Cold, Cold, or Marine climate zones (Figure 11) and therefore have low cooling loads. One of these models is a 17,500-ft<sup>2</sup> small office in Fairbanks, Alaska, with a PVAV with parallel fan powered (PFP) boxes HVAC system and fixed dry bulb economizer on its two air loops. The annual electricity cooling consumption increases from 4,700 kWh to 5,800 kWh (22.6% increase) between the baseline and upgrade model. While the percent increase is notable, the actual increase (1,100 kWh) is minimal. These models also show no change in their design outdoor air rates between the baseline and upgrade model, and the average outdoor air fraction is reduced, indicating the DCV measure is being applied correctly.

The models located in a Hot-Dry climate zone had very minimal electricity cooling penalties, between 0.001 and 2%, and all have some form of VAV system (VAV chiller with gas boiler reheat, VAV chiller with PFP boxes, or PVAV with gas heat with electric reheat). Models are showing that the DCV measure is being applied correctly, so the cooling penalties in these models are assumed to be anomalies, but has minimal impact on the overall results of this measure.

{:refdef: style="text-align: center;"}
![Chart, bar chart Description automatically generated](media\dcv_image12.png)
{:refdef}

{:refdef: style="text-align: center;"}
Figure 11. Count of models with economizers on all air loops and electricity cooling penalties by Building America climate zone
{:refdef}

### 6.5.3. Pump Penalties

Several models also had pump electricity penalties with the DCV measure applied (Figure 5). After investigation, these models are the same ones that have heating or cooling penalties. This is in line with expectations as pump and fan energy follows HVAC system consumption.

### 6.5.4. Interior Lighting Impacts

Twenty-nine models (0.008% of the total ComStock models) show interior lighting savings, which is unexpected for this measure. This is due to a known baseline anomaly and is not a result of this measure but has very little impact on the results.

## 6.6. Impacts of Climate Zone and Building Type 

Site energy savings for this measure vary by climate zone and building type. Climate zones 3C, 7A, and 8 saw the highest average percent site energy consumption savings (Figure 12). The median percent savings were similar across climate zones, with climate zones 3C and 4C having the models with the highest savings (Figure 13).

{:refdef: style="text-align: center;"}
![Chart, bar chart Description automatically generated](media\dcv_image13.png)
{:refdef}

{:refdef: style="text-align: center;"}
Figure 12. Average percent site energy consumption savings by ASHRAE 2006 climate zone
{:refdef}

{:refdef: style="text-align: center;"}
![Chart, box and whisker chart Description automatically generated](media\dcv_image14.png)
{:refdef}

{:refdef: style="text-align: center;"}
Figure 13. Percent savings distributions for site energy consumption by ASHRAE 2006 climate zone
{:refdef}

Retail standalone, primary schools and secondary schools saw the highest average percent site energy consumption savings (Figure 14). The median percent savings were similar across climate zones, with both retail buildings (standalone and strip mall) having the models with the highest savings (Figure 15) because of the applicable buildings, retail have the highest design outdoor air flow rate.

{:refdef: style="text-align: center;"}
![Chart, bar chart Description automatically generated](media\dcv_image15.png)
{:refdef}

{:refdef: style="text-align: center;"}
Figure 14. Average percent site energy consumption savings by building type
{:refdef}

{:refdef: style="text-align: center;"}
![Chart, box and whisker chart Description automatically generated](media\dcv_image16.png)
{:refdef}

{:refdef: style="text-align: center;"}
Figure 15. Percent savings distributions for site energy consumption by building type
{:refdef}

# References

\[1\] A. Parker, H. Horsey, M. Dahlhausen, M. Praprost, C. CaraDonna, A. LeBar and L. Klun, \"ComStock Reference Documentation: Version 1,\" National Renewable Energy Laboratory, Golden, CO, 2022.

\[2\] \"Cambium \| Energy Analysis \| NREL,\" \[Online\]. Available: [https://www.nlr.gov/analysis/cambium.html](https://www.nlr.gov/analysis/cambium.html). \[Accessed 02 September 2022\].

\[3\] \"Emissions & Generation Resource Integrated Database (eGRID) \| US EPA,\" \[Online\]. Available: [https://www.epa.gov/egrid](https://www.epa.gov/egrid). \[Accessed 02 September 2022\].

\[4\] E. Present, P. Gagnon, E. J. H. Wilson, N. Merket, P. R. White and S. Horowitz, \"Choosing the Best Carbon Factor for the Job: Exploring Available Carbon Emissions Factors and the Impact of Factor Selection,\" in *2022 ACEEE Summer Study on Energy Efficiency in Buildings*, Pacific Grove, CA, 2022.

\[5\] G. Vijayakumar, *ANSI/RESNET/ICC 301-2022 - Standard for the Calculation and Labeling of the Energy Performance of Dwelling and Sleeping Units using an Energy Rating Index,* Oceanside, CA, 2022.

# Appendix A.
{:refdef: style="text-align: center;"}
![Chart, bar chart Description automatically generated](media\dcv_image17.jpeg)
{:refdef}

{:refdef: style="text-align: center;"}
Figure A-1. Site annual natural gas consumption of the ComStock baseline and the measure scenario by census division
{:refdef}

{:refdef: style="text-align: center;"}
![Chart, bar chart, histogram Description automatically generated](media\dcv_image18.jpeg)
{:refdef}

{:refdef: style="text-align: center;"}
Figure A-2. Site annual natural gas consumption of the ComStock baseline and the measure scenario by building type
{:refdef}

{:refdef: style="text-align: center;"}
![Chart, bar chart Description automatically generated](media\dcv_image19.jpeg)
{:refdef}

{:refdef: style="text-align: center;"}
Figure A-3. Site annual electricity consumption of the ComStock baseline and the measure scenario by building type
{:refdef}

{:refdef: style="text-align: center;"}
![Chart, bar chart Description automatically generated](media\dcv_image20.jpeg)
{:refdef}

{:refdef: style="text-align: center;"}
Figure A-4. Site annual electricity consumption of the ComStock baseline and the measure scenario by census division
{:refdef}
