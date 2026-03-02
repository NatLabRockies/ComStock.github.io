---
layout: default
title: Ground-source Heat Pump Package
parent: Upgrade Measures
published: false
---

# Comprehensive Ground Source Heat Pump Package: Central Water-to-Water GSHP + Packaged Water-to-Air GSHP + Console Water-to-Air GSHP
{: .fw-500 }

Author: Marlena Praprost and Amy Allen

# Executive Summary

Building on the successfully completed effort to calibrate and validate the U.S. Department of Energy's ResStock™ and ComStock™ models over the past 3 years, the objective of this work is to produce national data sets that empower analysts working for federal, state, utility, city, and manufacturer stakeholders to answer a broad range of analysis questions.

The goal of this work is to develop energy efficiency, electrification, and demand flexibility end-use load shapes (electricity, gas, propane, or fuel oil) that cover a majority of the high-impact, market-ready (or nearly market-ready) upgrade measures, or upgrades. "Measures" refers to energy efficiency variables that can be applied to buildings during modeling.

An *end-use savings shape* is the difference in energy consumption between a baseline building and a building with an energy efficiency, electrification, or demand flexibility upgrade applied. It results in a time-series profile that is broken down by end use and fuel (electricity or on-site gas, propane, or fuel oil use) at each time step.

ComStock is a highly granular, bottom-up model that uses multiple data sources, statistical sampling methods, and advanced building energy simulations to estimate the annual sub-hourly energy consumption of the commercial building stock across the United States. The baseline model intends to represent the U.S. commercial building stock as it existed in 2018. The methodology and results of the baseline model are discussed in the final technical report of the [End-Use Load Profiles](https://www.nrel.gov/buildings/end-use-load-profiles.html) project.

An upgrade package applies one or more End-Use Savings Shapes upgrades to a single building model simulation. Since ComStock is a bottom-up physics-based model, an upgrade package will go beyond aggregating or summing the individual upgrade results and produce novel results by simulating interactions between the upgrades. For example, pairing an envelope upgrade with an electrification upgrade would likely result in higher savings results than the sum of these upgrades individually, and the size of the heating, ventilating, and air conditioning (HVAC) equipment may be reduced if the envelope upgrade reduces the loads significantly.

This documentation focuses on an upgrade package of three End-Use Savings Shapes upgrades--- [Hydronic GSHP]({{site.baseurl}}{% link docs/upgrade_measures/hvac_hydronic_gshp.md %}), [Packaged GSHP]({{site.baseurl}}{% link docs/upgrade_measures/hvac_gshp_packaged.md %}), and [Console GSHP]({{site.baseurl}}{% link docs/upgrade_measures/hvac_console_gshp.md %})---which we will refer to collectively as the "Comprehensive GSHP" package. These measures serve different existing HVAC system types. This documentation will cover the ComStock applicability criteria for each measure, and results of the package applied across the ComStock sample. A given building will have only one (if any) of these measures applied to it as part of the package. As a result, there are no interactive effects among these measures. Documentation for each of these individual upgrades can be found on the [ComStock Measures Documentation]({{site.baseurl}}{% link docs/upgrade_measures/upgrade_measures.md %}) page.

The Packaged GSHP measure had the highest applicability at 56% of the stock floor area, while the Hydronic GSHP was applicable to 13% and the Console GSHP measure was applicable to 11%. In total, the package was applicable to 80% of the ComStock floor area.

This package demonstrates 18.0% total site energy savings (783 trillion British thermal units \[TBtu\]) for the U.S. commercial building stock modeled in ComStock (Figure 3). The savings are attributed to the electrification of natural gas heating systems, as well as improvements to cooling due to the installation of a ground source heat pump:

-   **83.2%** stock **heating gas** savings (711.7 TBtu)

-   **-96.7%** stock **heating electricity** savings (-169.4 TBtu)

-   **29.4%** stock **cooling electricity** savings (196.3 TBtu)

-   **-92.5%** stock **pump electricity** savings (39.4 TBtu).

Three electricity grid scenarios are presented to compare the emissions of the ComStock baseline and the GSHP Package. Two scenarios---Long-Run Marginal Emissions Rate (LRMER) High Renewable Energy (RE) Cost 15-Year and LRMER Low RE Cost 15-Year---use the Cambium data set, and the last uses the Emissions & Generation Resource Integrated Database (eGRID) data set ​ \[1\], \[2\]​. Across the three electricity grid scenarios presented, electricity emissions increased by 1.0-8.6% (3-13 million metric tons (MMT) of carbon dioxide equivalent (CO<sub>2</sub>e)) (Figure 4). Natural gas emissions dropped by 59.3% (48 MMT CO<sub>2</sub>e), resulting in an overall greenhouse gas reduction across all fuel types of 13-17% (41-51 MMT CO<sub>2</sub>e) depending on the grid scenario. 

# Acknowledgments

The authors would like to acknowledge additional contributors to the measures in the Comprehensive GSHP package: Chris CaraDonna, Matt Leach, Matt Mitchell, and Andrew Parker. In addition, we would like to thank our peer reviewers of this package, Eric Bonnema and Landan Taylor.

# 1. Introduction

## Accessing Results

This documentation covers the GSHP upgrade package methodology and briefly discusses key results. Results can be accessed via the ComStock™ [Published Datasets]({{site.baseurl}}{% link docs/data.md %}) page.

## Upgrade Package Summary

| Package Title      | Comprehensive GSHP Upgrade                                                                                     |
|--------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Package Definition | This package replaces existing HVAC systems with one of three ground source heat pump-based systems in applicable models. It is a combination of three upgrades (Hydronic GSHP, Packaged GSHP, and Console GSHP), also documented in 2024 Release 1. |
| Applicability      | This package is applicable to 80% of the stock floor area. For a package to be applicable, the model must meet the applicability criteria of one or more of the measures.<br>**Packaged Water-to-Air GSHP**: Models with gas-fired or electric resistance packaged rooftop systems or packaged variable air volume systems (PVAV). This measure is applicable to 56% of ComStock floor area.<br>**Central Hydronic Water-to-Water GSHP**: Models with central hydronic systems including variable air volume (VAV) systems and dedicated outdoor air systems (DOAS) that are served by a boiler and/or chiller. This measure is applicable to 13% of ComStock floor area.<br>**Console Water-to-Air GSHP**: Models with minimal or no ductwork, including packaged terminal units, baseboard electric, gas unit heaters, and residential-style systems. This measure is applicable to 11% of ComStock floor area. |
| Not Applicable     | This package is not applicable to 20% of the stock floor area.<br>**Packaged Water-to-Air GSHP**: Models that do not contain gas-fired or electric resistance packaged RTUs or PVAVs.<br>**Central Hydronic Water-to-Water GSHP**: Models that do not have hydronic distribution. In addition, this measure is not applicable to buildings with hot water baseboards or  buildings served by district thermal energy systems.<br>**Console Water-to-Air GSHP**: Models with ducted systems, including RTUs, PVAVs, and VAVs.      |
| Release            | 2024 Release 1: 2024/comstock_amy2018_release_1/                                                                                                 |

# 2. Technology Summary

The carbon intensity of electricity in the United States declined by almost 40% from 2001 to 2020​ \[3\], \[4\]​. However, space and process heating needs continue to be met predominately by fossil fuels, and the combustion of fossil fuels for space and domestic hot water heating accounts for roughly 10% of U.S. carbon emissions \[5\]. Properly designed ground heat exchanger coupled heating and cooling systems can offer benefits in energy efficiency relative to "conventional" heating, ventilating, and air-conditioning (HVAC) systems and facilitate beneficial electrification. A detailed study of 36 buildings in the United States that are fully or partially served by HVAC systems tied to ground heat exchangers found the potential for very efficient energy performance (with one-third having EnergyStar® ratings above 90). The results of the study, while limited, also highlighted the potential pitfalls of poor design practices​ \[6\]. Ground source heat pumps can be configured as water-to-air or water-to-water, depending on whether the building uses air-based distribution or hydronic distribution. The Packaged GSHP and Console GSHP measures are configured as water-to-air heat pumps, while the Hydronic GSHP measure is a water-to-water heat pump.  

Packaged water-to-air heat pumps consist of a refrigerant-to-water heat exchanger (which draws heat from or rejects heat to the condenser, or source-side loop), an air-to-refrigerant coil (which conditions supply air), a supply fan, and ductwork connections. (Note that the source-side loop is sometimes referred to as the ground loop but will be referred to here as the condenser loop to account for the potential of a heat exchanger separating the two loops.) In the configuration considered in this measure, the source side of the heat pumps is tied to a loop that circulates a water-based heat transfer fluid through a ground heat exchanger. Water-to-air heat pumps are the most common form of ground-coupled heat pump and are commonly available as packaged options in capacities ranging from 0.5 to 30 ton​ \[7\], \[8\]​, though custom equipment may be available in larger sizes. Although different Air-Conditioning, Heating, and Refrigeration Institute (AHRI) standards exist to emulate different types of source loop equipment, water-to-air heat pumps sold in the United States are generally tested under all of them, and thus the same equipment can be used for water loops tied to ground heat exchangers or to cooling towers and boilers for heat rejection/addition. The relevant AHRI standards are Water Loop (AHRI 320), Ground Water (AHRI 325), and Ground Loop (AHRI 330) ​ \[7\]​.  

Console water-to-air heat pumps are not ducted (or are minimally ducted) and serve individual spaces. They are "all-in-one" units and can be coupled to a ground loop on the source side. Console water-to-air heat pumps can replace electric baseboard heaters or air-source packaged terminal heat pumps. They can also bring in outdoor air for ventilation.  Console water-to-air heat pumps consist of a refrigerant-to-water heat exchanger (which draws heat from or rejects heat to the condenser loop), an air-to-refrigerant coil (which conditions supply air), a blower, and, in some cases, limited ductwork. In the absence of ductwork, supply air is provided directly through a grille on the unit itself. These units can also incorporate hot gas reheat coils to reheat dehumidified air, and some have an option for supplemental electric heating​​. Console water-to-air heat pump coils (and coils of this nature in general) often have relatively high minimum entering air temperatures (50°F--55°F), as specified by the manufacturer; thus, if the units are providing outdoor air, gas or electric coils must be used to precondition the outdoor air in cold conditions \[9\], \[10\]​​.  The Console GSHP measure is configured to supply outdoor air through the console heat pumps, however, outdoor air could also be supplied through a separate DOAS.

Hydronic HVAC systems are common in commercial buildings, and often use boilers and chillers as the primary equipment for heating and cooling. The Hydronic GSHP measure focuses on the retrofit of hydronic HVAC systems to use central, ground-coupled, water-to-water heat pumps as primary equipment. Separate heat pumps are used for heating and cooling in this configuration. The "source" side of the heat pumps is tied to a loop that circulates a water-based heat transfer fluid through the ground heat exchanger. The "load" side of each heat pump circulates water through the building's existing hydronic systems for hot water and chilled water. Water-to-water heat pumps are particularly efficient when delivering water at "moderate" temperatures (for example, below 115°F in heating), and are now available in multistage and variable-speed configurations \[6\]. Commercial water-to-water heat pumps are available in capacities ranging from 5 tons to 100 tons, and some models are compatible with variable flow on the source side ​ \[11\]​. Most water-to-water heat pumps can generate hot water at temperatures up to around 130°F--150°F \[12\].

# 3. ComStock Baseline Approach

Of the buildings represented in ComStock™,  about 17% are served by hydronic HVAC systems, 42% are served by rooftop units, 19% are served by PVAV systems, and 7% are served by PTACs (excludes buildings served by district energy systems). The remaining 15% of buildings in ComStock are served by a variety of other HVAC systems with relatively low prevalence. Of the 42 HVAC systems modeled in ComStock, 21 systems, representing 80% of the stock floor area, received one of the three GSHP upgrade measures.

The GSHP measures do not apply to buildings served by district energy (heating hot water or chilled water) systems, as this retrofit is intended to represent the installation of a ground heat exchanger to serve a single building's load. Other system types not applicable include direct evaporative coolers, variable refrigerant flow (VRF) systems, and some DOAS systems. The applicability for each measure was determined based on thorough research into the ease and practicality of retrofit and discussions with industry experts as part of a Technical Advisory Group (TAG). More details about the modeling approach for each measure can be found in the individual measure documents. Table 1 summarizes the HVAC systems that were applicable to each of the GSHP measures and their corresponding percentage of the ComStock floor area.

Table 1. GSHP Upgrade Applicability by Baseline HVAC System and Percent of Floor Area

![](media\package_gshp_table1.png)

# 4.  Modeling Approach

The following sections briefly summarize the modeling approach and applicability for the three measures included in the Comprehensive GSHP package. For more detailed descriptions, reference the individual upgrade documentation. This package is applicable to 80% of the ComStock floor area.

## 4.1. Central Hydronic Water-to-Water Ground Source Heat Pump

For each hydronic HVAC loop that currently exists in the building, the measure removes the existing primary equipment from the loop and replaces it with a water-to-water heat pump, with separate heat pumps supplying heating and cooling. The source side of each heat pump is tied to a common ground loop. In air handlers performing space conditioning (as opposed to tempering ventilation air), the measure will replace any direct expansion cooling or gas or electric resistance heating coils with hydronic coils and create new plant loops served by water-source heat pumps to condition these loops as necessary. Existing electric baseboard systems will be replaced with hot water fan coils.  Existing hot water baseboard systems are not addressed by this measure.  

This measure will replace any natural gas coils in DOAS with electric coils, to obtain a fully electric heating and cooling system. Existing electric resistance heating coils in DOAS units will be left as is. Air-side distribution systems (fans, dampers, etc.) are not affected by this measure. Existing systems for providing outdoor air will be preserved. This approach was selected to reflect practical constraints in existing buildings and isolate the effects of this retrofit measure. Details of the sizing approach for this measure are discussed in the individual measure documentation.   

### 4.1.2. Applicability

This measure is applicable to buildings with hydronic HVAC systems supplying heating or cooling or both, with the exception of (1) buildings served by hydronic baseboards (due to their high temperature requirements) and (2) district thermal energy systems. This includes central VAV systems with chilled water coils, and fan coil-based systems, among other system types. Table 1 summarizes the HVAC system types that received the Hydronic GSHP measure and their corresponding percent of the ComStock floor area. In total, this measure was applicable to 13.5% of the ComStock floor area.

## 4.2. Packaged Water-to-Air Ground Source Heat Pump

This measure replaces the existing applicable HVAC equipment with a new Packaged GSHP system. Specifically, this measure implements single-zone water-to-air heat pump systems with the source-side loop connected to a ground heat exchanger. The heat pump coils are configured with their load side as part of the existing air handling units, and their source side tied to a condenser loop that is configured to draw heat from and reject heat to the ground heat exchanger loop. Packaged water-to-air heat pumps can have variable-speed supply fans and can incorporate air- or water-side economizing and demand-controlled ventilation (DCV). These units can also incorporate hot gas reheat coils (which use heat recovered from the refrigeration cycle) to reheat dehumidified air. Because water-source heat pump coils often have relatively high minimum entering air temperatures (50°F--55°F), as specified by the manufacturers, gas or electric coils must be used to precondition outdoor air in cold conditions if the units are providing outdoor air ​ \[8\], \[9\]​. Packaged water-to-air heat pumps are connected both to ductwork (for air distribution) and to condenser water serving the heat pumps. The Packaged GSHP configuration also incorporates additional efficiency features including DCV and economizers.

### 4.2.1. Applicability

This measure is applicable to most buildings that have an existing packaged single zone RTU system. The exception is RTU systems that operate using district chilled water or district hot water, to which this measure is not applicable. Table 1 summarizes the HVAC system types that received the Packaged GSHP measure and their corresponding percent of the ComStock floor area. In total, this measure was applicable to 56.1% of the ComStock floor area.

## 4.3. Console Water-to-Air Ground Source Heat Pump

Console water-to-air heat pumps are generally non-ducted or have minimal ducting and are intended to serve relatively small spaces. They are available in sizes from 0.5 to 1.5 tons ​\[9\], \[13\]​. Some console water-to-air heat pumps offer outdoor air dampers​​, and others do not ​\[9\], ​\[14\]. This measure will supply ventilation directly through the water-to-air heat pump, as opposed to separately through a DOAS. Console (and other) water-to-air heat pumps that are configured for providing outdoor air have relatively high minimum entering air temperature requirements. Thus, this measure will configure console units that are tempering outdoor air with an electric preheat coil.

### 4.3.1. Applicability

This measure is applicable to buildings that have an existing packaged terminal system, including packaged terminal air conditioners (PTAC) and packaged terminal heat pumps (PTHP). In addition to packaged terminal systems, console water-to-air heat pumps are an appropriate retrofit for other non-ducted systems, such as baseboard heaters and unit heaters, or for residential-style systems. In some cases, the existing system in the baseline does not currently have cooling. Retrofitting a heating-only or cooling-only system with a GSHP could result in extreme load imbalance and negatively affect the performance of the ground heat exchanger over time​ \[6\]. Therefore, the heat pump will supply both heating and cooling to the building, and cooling will be added to heating-only systems via the console heat pump retrofit. Table 1 summarizes the HVAC system types that received the Console GSHP measure and their corresponding percent of the ComStock floor area. In total, this measure was applicable to 10.8% of the ComStock floor area.

## 4.4. Performance Data

Real performance data was used to model each of the three GSHP configurations. Table 2 summarizes the properties of the performance data used for each measure, but more details can be found in the individual measure documentation.

Table 2. Performance Data Used for Each GSHP Measure

| GSHP Measure | Manufacturer | Product          | Capacity |
|--------------|--------------|------------------|----------|
| Hydronic     | Carrier      | 60WG/30WG Series | 56-ton   |
| Packaged     | Trane        | Axiom GWS        | 10-ton   |
| Console      | Trane        | Axiom GWS        | 3-ton    |

## 4.5.  Greenhouse Gas Emissions

Three electricity grid scenarios are presented to compare the emissions of the ComStock baseline and the Comprehensive GSHP scenario. The choice of grid scenario will impact the grid emissions factors used in the simulation, which determines the corresponding emissions produced per kilowatt-hour. Two scenarios---Long-Run Marginal Emissions Rate (LRMER) High Renewable Energy (RE) Cost 15-Year and LRMER Low RE Cost 15-Year---use the Cambium data set, and the last uses the eGrid dataset \[1\], \[2\]. All three scenarios vary the emissions factors geospatially to reflect the variation in grid resources used to produce electricity across the United States. The Cambium data sets also vary emissions factors seasonally and by time of day. This study does not imply a preference for any particular grid emissions scenario, but other analysis suggests that the choice of grid emissions scenario can impact results \[15\]. Emissions due to on-site combustion of fossil fuels use the emissions factors shown in Table 11, which are from Table 7.1.2(1) of draft American National Standards Institute/Residential Energy Services Network/International Code Council 301 \[16\]. To compare total emissions due to both on-site fossil fuel consumption and grid electricity generation, the emissions from a single electricity grid scenario should be combined with all three on-site fossil fuel emissions.

Table 3. On-Site Fossil Fuel Emissions Factors 

|**Natural gas**  | 147\.3 lb/MMBtu (228.0 kg/MWh)<sup>a</sup>   |
|**Propane**      | 177\.8 lb/MMBtu (182.3 kg/MWh)    |
|**Fuel oil**     | 195\.9 lb/MMBtu (303.2 kg/MWh)    |

  <sup>a</sup> lb = pound; MMBtu = million British thermal units; kg = kilogram; MWh = megawatt-hour

## 4.6. GHEDesigner Workflow

GHEDesigner is a Python package for designing ground heat exchangers used with ground-source heat pump systems​ \[17\]​. The GSHP upgrade measures leverage GHEDesigner for sizing the vertical ground heat exchangers used in the ComStock models. GHEDesigner is called and run within the GSHP measures. Figure 1 shows the full ComStock GSHP measure workflow. First, the GSHP measure is applied[, ]{.ul}which replaces the existing system with one of the GSHP configurations. An initial sizing run determines the annual loads the ground heat exchanger needs to supply. The ground loads are exported to GHEDesigner in the form of a JavaScript Object Notation (JSON) file. GHEDesigner runs calculations to determine the g-function, which is used to size the ground heat exchanger in the model. A final simulation is run with the ground heat exchanger sized to the full building load. See the individual measure documents for more detail about the GHEDesigner workflow.   

{:refdef: style="text-align: center;"}
![](media\package_gshp_image1.png)
{:refdef}

{:refdef: style="text-align: center;"}
Figure 1. ComStock GHEDesigner workflow
{:refdef}

## 4.7. Limitations and Concerns

The representation of heat pump performance in EnergyPlus relies on data obtained from manufacturers. These data are not fully representative of all heat pumps of this type available in the United States. Field demonstrations to document the reasonableness of the curves have not been performed.  

For the Packaged GSHP measure, available space is a consideration in the feasibility of the retrofit. Newer RTUs, and heat pumps in particular, may require a larger footprint than older models with direct-expansion cooling and gas or electric heating​ \[18\]​.  

# 5. Output Variables

Table 4 includes a list of output variables that are calculated in ComStock. These variables are important to understand the differences between buildings with and without the GSHP upgrade measures applied. Additionally, these output variables can also be used for understanding the economics (e.g., return on investment) of the upgrade if cost information (i.e., material, labor, and maintenance cost for technology implementation) is available.  

Table 4. Output Variables Calculated from the Measure Application

| Variable Name                                | Description                                             |
|----------------------------------------------|---------------------------------------------------------|
| com_report_ghx_borehole_depth_ft             | Borehole depth calculated from GHEDesigner (feet)       |
| com_report_ghx_design_flow_rate_ft_3_per_min | Ground heat exchanger flow rate (cubic feet per minute) |
| com_report_ghx_num_boreholes                 | Number of boreholes calculated from GHEDesigner         |

# 6. Results

In this section, results are presented both at the stock level and for individual buildings through savings distributions. Stock-level results include the combined impact of all the analyzed buildings in ComStock, including buildings that are not applicable to this upgrade. Therefore, they do not necessarily represent the energy savings of a particular or average building. Stock-level results should not be interpreted as the savings that a building might realize by implementing the Comprehensive GSHP upgrade package.

Total site energy savings are also presented in this section. Total site energy savings can be a useful metric, especially for quality assurance/quality control, but this metric on its own can have limitations for drawing conclusions. Further context should be considered, as site energy savings alone do not necessarily translate proportionally to savings for a particular fuel type (e.g., gas or electricity), source energy savings, cost savings, or greenhouse gas savings. This is especially important when an upgrade impacts multiple fuel types or causes decreased consumption of one fuel type and increased consumption of another. Many factors should be considered when analyzing the impact of an energy efficiency or electrification strategy, depending on the use case.

## 6.1. Realized Applicability

Figure 2 provides a breakdown of the applicability of the Comprehensive GSHP package by individual measure. 28% of total floor area was applicable to both upgrades, while 37% only received the lighting upgrade and 16% received only the heat pump upgrade. In total, at least one measure from the package was applicable to 81% of the stock floor area.

{:refdef: style="text-align: center;"}
![](media\package_gshp_image2.png)
{:refdef}

{:refdef: style="text-align: center;"}
Figure 2. Applicability for the Comprehensive GSHP package and by individual measure
{:refdef}

## 6.2. Stock Energy Impacts

This package demonstrates 18.0% total site energy savings (783 trillion British thermal units \[TBtu\]) for the U.S. commercial building stock modeled in ComStock (Figure 3). The savings are attributed to the electrification of natural gas heating systems, as well as improvements to cooling due to the installation of a ground source heat pump:

-   **83.2%** stock **heating gas** savings (711.7 TBtu)

-   **-96.7%** stock **heating electricity** savings (-169.4 TBtu)

-   **29.4%** stock **cooling electricity** savings (196.3 TBtu)

-   **-92.5%** stock **pump electricity** savings (39.4 TBtu).

{:refdef: style="text-align: center;"}
![](media\package_gshp_image3.jpeg)
{:refdef}

{:refdef: style="text-align: center;"}
Figure 3. Comparison of annual site energy consumption between the ComStock baseline and the Comprehensive GSHP scenario.
{:refdef}
{:refdef: style="text-align: center;"}
Energy consumption is categorized both by fuel type and end use.
{:refdef}

This package reduces stock-level natural gas heating by 83%, as to be expected since implementation of the GSHP measures results in nearly full electrification of space heating in buildings for which it is applicable. There are a few exceptions to this, including some warehouse and retail spaces, which in the baseline RTU systems do not get the Packaged GSHP upgrade, which is described in the individual documentation for that measure. In addition, 20% of floor area is made up of HVAC systems that do not receive any of the three GSHP measures, which includes district systems, direct evaporative coolers, and a few other less common system types. Therefore, this package does not electrify 100% of the natural gas heating load.

There is a 97% increase in electric heating, which is also to be expected when transitioning from gas heating to electric heating. Note that the magnitude of the site's gas space heating savings is much larger than the magnitude of the site's electric space heating increase. This is due to the same space heating load being met more efficiently with heat pumps (heating COP typically between 3 and 5) relative to the existing gas or electric coils (efficiency 0.8--1). As a result, the total heating load across all heating fuels decreased by 54%, which contributes substantially to the total site energy savings of 18%. However, note that site energy savings is not a comprehensive assessment of other notable considerations such as source energy savings, energy costs, greenhouse gas emissions, or peak demand.  

We also notice cooling savings of 29% as a result of the higher efficiency performance of the various heat pump configurations compared with existing cooling systems. The fans end use also showed a 2% savings, which is a result of numerous changes to operation across the three GSHP configurations including changes in supply temperatures, cooling load requirements, and air flow rates. The pumps end use, while they make up a small portion of the total stock energy, experienced a 93% increase due to the pumping demands of the ground and condenser loops of the new GSHP systems.

## 6.3. Stock Greenhouse Gas Emissions Impact

ComStock simulation results show greenhouse gas emissions avoided across all electricity grid scenarios for all on-site combustion fuel types (Figure 4). Across all fuels, greenhouse gas emissions avoided are 41 to 51 million metric tons of CO~2~-equivalent (13% to 17%) depending on the grid scenario chosen. Electricity greenhouse gas emissions increased by 3 to 13 million tons (1.0% to 8.6%). Because this upgrade involves electrifying a large portion of the space heating load, an increase in electricity emissions is not unexpected.  

Natural gas emissions avoided by this upgrade equate to 48 million tons of CO~2~, or 59.3%. This is driven by transitioning natural gas heating systems in approximately 80% of the stock. Note that there are other natural gas loads in buildings, such as kitchen equipment, that are not electrified through this upgrade package. The natural gas results remain consistent across all three grid scenarios, as these scenarios exclusively pertain to modifications within the electricity grid, without affecting natural gas outcomes.  

{:refdef: style="text-align: center;"}
![](media\package_gshp_image4.jpeg)
{:refdef}

{:refdef: style="text-align: center;"}
Figure 4. Greenhouse gas emissions comparison of the ComStock baseline and the Comprehensive GSHP scenario
{:refdef}
{:refdef: style="text-align: center;"}
Three electricity grid scenarios are presented: Cambium Long-Run Marginal Emissions Rate (LRMER) High Renewable Energy (RE) Cost 15-Year, Cambium LRMER Low RE Cost 15-Year, and eGRID.
Note: MMT stands for million metric tons. GHG stands for greenhouse gas.
{:refdef}

## 6.4. Site Energy Savings Distributions

This section discusses site energy consumption for quality assurance/quality control purposes. Note that site energy savings can be useful for these purposes, but other factors should be considered when drawing conclusions, as they do not necessarily translate proportionally to source energy savings, greenhouse gas emissions avoided, or energy cost. 

Figure 5 shows the percent savings distributions of the baseline ComStock models versus the Comprehensive GSHP package by end use and fuel type for applicable models. In other words, each data point in the distribution represents the percent energy savings between a baseline ComStock model and the corresponding upgrade model with measures applied.  

Natural gas heating and other fuel heating show nearly 100% savings in most buildings. However, as discussed previously and in the individual measure documentation, some warehouse and retail spaces were not upgraded due to very low load requirements (mainly pertaining to the Packaged GSHP upgrade), resulting in less than 100% natural gas savings in some buildings. Some buildings show low natural gas savings (\<50%), which is primarily warehouses with a large portion of square footage attributed to these storage spaces that were not upgraded.

Electric heating savings are in the range of 30%--70% for most buildings. It is important to note that this plot only shows buildings that started out with some amount of electric heating. Buildings that started with 100% natural gas heating would have an infinite increase in electric heating and are not shown on this plot. Therefore, the 30%--70% electric heating savings can be attributed to swapping out electric resistance heating with heat pumps with a heating COP of 3--5. Some buildings show very negative electric heating savings. This represents buildings that started out with mostly gas heating but a small portion of electric baseboards; they therefore show a large increase in electric heating when electrifying the majority of the heating load.  

Cooling energy in applicable buildings decreased by an average of 20%--40% as a result of replacing existing direct-expansion cooling systems with more efficient heat pumps. A small number of buildings experienced cooling penalties, which are mainly warehouses with very low cooling loads, making the percentage savings very sensitive to small changes in magnitude.

Pump energy increased by more than 30% in most buildings due to the added pumping demands of vertical GSHP systems and additional pumps being added to the HVAC configurations. Pumps represent a relatively small percentage of the overall stock energy (\<2%), so the high percent savings can be misleading. Fan electricity increased in some buildings and decreased in others, with some having large percentage changes in fan energy. Many of the buildings with very large changes in fan energy were found to be warehouses, which start out with low HVAC loads and therefore percentage changes are sensitive. In addition, the system replacement undergone results in a variety of changes that can affect fan energy including lower supply temperatures, increase pumping energy, replacement or addition of fans on the loop, and more. The median percent change in fan energy across the stock is around a 15% savings.

The heat recovery end use showed nontrivial percentage changes in some buildings. However, this is solely based on the changes causing existing heat recovery systems to run more or less. The magnitude of this end use is minimal compared to total building energy usage. 

Minimal or no differences are observed for service water heating systems, refrigeration, interior lighting, and interior equipment, as these systems are not directly impacted by the upgrade package. However, some buildings show minor changes due only to subtle differences in ambient air temperature that affect the operation of these systems.

{:refdef: style="text-align: center;"}
![](media\package_gshp_image5.jpeg)
{:refdef}

{:refdef: style="text-align: center;"}
Figure 5. Percent site energy savings distribution for ComStock models with the Comprehensive GHP package applied by end use and fuel type
{:refdef}
{:refdef: style="text-align: center;"}
The data points that appear above some of the distributions indicate outliers in the distribution, meaning they fall outside 1.5 times the interquartile range. The value for n indicates the number of ComStock models that were applicable for energy savings for the fuel type category.
{:refdef}
## 6.5. Peak Impacts

Figure 6 shows the impact of the Packaged GSHP upgrade on seasonal peak hours. The winter peak is shifted earlier in the day by nearly an hour as morning electric heating now has more influence on peak load in winter. The summer and shoulder peaks move slightly later in the day. Because the efficiency of the new GSHP system is no longer dependent on outdoor air temperature, as is the case with the direct-expansion cooling systems in the baseline, we may expect the cooling-dominated peaks to change timing slightly with the new systems.

{:refdef: style="text-align: center;"}
![](media\package_gshp_image6.png)
{:refdef}

{:refdef: style="text-align: center;"}
Figure 6. Maximum daily peak timing by season between the baseline and Comprehensive GSHP scenario
{:refdef}

Figure 7 shows the impact of the Packaged GSHP upgrade on seasonal peak intensity for the median building. As shown, summer peaks are reduced across all climate zones due to the reductions in cooling from the more efficient heat pumps. In most climate zones except Subarctic, the median summer peak intensity was reduced by 24%--30%. Winter peaks in heating-dominated climates (Cold, Very Cold) experienced a 24%-27% increase due to the electrification of the heating load. In cooling-dominated and moderate climates, the peak intensity was reduced by up to 16%. This is because heating is generally not as big a driver of the winter peak in these climates, so the HVAC efficiency improvements outweighed the added heating load.  

In shoulder seasons, the peak intensity was reduced across all climates by 14%--19% due to the HVAC efficiency improvements of the GSHPs. In the Subarctic climate zone, there was no change in winter or shoulder peak in the median building, but there was a very small sample size for this climate. In addition, the subarctic climate does not have any days in the year that are considered "summer," which is why this part of the graph is blank.  

{:refdef: style="text-align: center;"}
![](media\package_gshp_image7.png)
{:refdef}

{:refdef: style="text-align: center;"}
Figure 7. Maximum daily peak intensity by season between the baseline and Comprehensive GSHP scenario
{:refdef}

## 6.6. Climate Zone Impacts

Figure 8 shows the percentage of total site energy savings by climate zone. There is a clear trend of increased savings as we move from the hottest climate zone (1A -- Hot Humid) to the coldest climate zone (8 - Subarctic). In warmest climate zones (1A through 3A), most buildings are seeing savings of around 10% or less with the 75<sup>th</sup> percentile reaching around 20% site energy savings. In colder climate zones (5 through 7) we notice average energy savings up to 20%--30% with the 75<sup>th</sup> percentile reaching 40% of more.

The lower quartile (25%) of buildings in climate zones 1A--3A show an increase in site energy with the upgrade applied. Further investigation of these buildings revealed that large increases in fan and pump energy were the driving end uses behind the increased site energy consumption. In hot climates, cooling tends to dominate the building load, meaning heat is rejected to the ground for much of the year and ground temperatures increase over time. This imbalanced load can result in increased pumping energy to maintain temperatures within a desired range on the main heat pump condenser loop. Compressor heat can also exacerbate imbalanced loads. In climates with a more balanced heating and cooling load, the ground temperatures remain more stable through the year, and over the course of many years of operation. As such, in climate zones 6 and 7, very few buildings show an increase in site energy.

Despite cold climates experiencing more significant air temperature swings throughout the year, the ground temperature stays relatively stable, resulting in consistent and efficient performance of ground-source heat pumps year-round. Due to the fact that cooling loads are present during most parts of the year in commercial buildings, cold climates tend to exhibit more balanced load profiles. Ground-source heat pumps can be more advantageous in climates with more balanced heating and cooling loads because of the long-term ground temperature implications on the borefield size. For this reason, the warm climates might require more costly installation and realize lower savings compared to a building of similar size in a cold climate.  

{:refdef: style="text-align: center;"}
![](media\package_gshp_image8.jpeg)
{:refdef}

{:refdef: style="text-align: center;"}
Figure 8. Percent site energy savings by climate zone between the baseline and Comprehensive
{:refdef}

# 7. References

\[1\] \"Cambium \| Energy Analysis \| NREL,\" \[Online\]. Available: <https://www.nrel.gov/analysis/cambium.html>. \[Accessed 02 September 2022\].

\[2\] \"Emissions & Generation Resource Integrated Database (eGRID) \| US EPA,\" \[Online\]. Available: <https://www.epa.gov/egrid>. \[Accessed 02 September 2022\].

\[3\] G. Schivley, I. Azevedo and C. Samaras, \"Assessing the evolution of power sector carbon intensity in the United States,\" *Environmental Research Letters,* 2018.

\[4\] US EIA, \"In 2020, the United States produced the least CO2 emissions from energy in nearly 40 years,\" 26 July 2021. \[Online\]. Available: <https://eee.eia.gov/todayinenergy/detail.php?id=48856>.

\[5\] S. Billimoria, M. Henchen, L. Guccione and L. Louis-Prescott, \"The Economics of Electrifying Buildings: How Electric Space and Water Heating Supports Decarbonization of Residential Buildings,\" Rocky Mountain Institute, 2018.

\[6\] K. D. Rafferty and S. P. Kavanaugh, \"Geothermal Heating and Cooling: Design of Ground Source Heat Pump Systems,\" in *ASHRAE*, Atlanta, Georgia, 2014.

\[7\] TRANE, \"Product Catalog: Water Source Heat Pump Axiom Rooftop\--GWS\*,\" Trane Technologies, Dublin, Ireland, 2021.

\[8\] Carrier, \"Product Data: Aquazone Large Capacity Water Source Heat Pumps,\" Carrier Corporation, Syracuse, New York, 2021.

\[9\] TRANE, \"Water-Source Heat Pump Axiom High Efficiency Console GEC,\" Trane Technologies, Dublin, Ireland, 2021.

\[10\] United Techologies, \"Water-Cooled and Condenserless Liquid Chillers/Water-Sourced Heat Pumps: 61WG/30WG/30WGA,\" Carrier.

\[11\] TRANE, \"Water-Cooled Modular Chillers,\" TRANE Technologies, 2021.

\[12\] TRANE, \"Water Source Heat Pump Axiom™ Water-to-Water --- EXW,\" Trane Technologies, Dublin, Ireland, 2023.

\[13\] Daikin, \"Enfinity™ Console Water Source Heat Pumps,\" Daikin Industries, 2020.

\[14\] ASHRAE, 2015 ASHRAE Handbook HVAC Applications, 2015.

\[15\] E. Present, P. Gagnon, E. J. H. Wilson, N. Merket, P. R. White and S. Horowitz, \"Choosing the Best Carbon Factor for the Job: Exploring Available Carbon Emissions Factors and the Impact of Factor Selection,\" in *2022 ACEEE Summer Study on Energy Efficiency in Buildings*, Pacific Grove, CA, 2022.

\[16\] G. Vijayakumar, *ANSI/RESNET/ICC 301-2022 - Standard for the Calculation and Labeling of the Energy Performance of Dwelling and Sleeping Units using an Energy Rating Index,* Oceanside, CA, 2022.

\[17\] United States Department of Energy, \"GHEDesigner 1.5,\" 3 April 2024. \[Online\]. Available: <https://pypi.org/project/GHEDesigner/>.

\[18\] US DOE, \"Decarbonizing HVAC and Water Heating in Commercial Buildings,\" US Department of Energy, Washington DC , 2021.
