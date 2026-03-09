---
layout: default
title: Improved Fan Scheduling and Control of Outdoor Air During Unoccupied Periods  
parent: Upgrade Measures
nav_order: 18
published: false
---

# Improved Fan Scheduling and Control of Outdoor Air During Unoccupied Periods  
{: .fw-500 }

Authors: Amy Allen and Chris CaraDonna

# Executive Summary

Building on the successful effort to calibrate and validate the U.S. Department of Energy's ResStock and ComStock models over the past 3 years, the objective of this work is to produce national data sets that empower analysts working for federal, state, utility, city, and manufacturer stakeholders to answer a broad range of analysis questions.

The goal of this work is to develop energy efficiency, electrification, and demand flexibility end-use load shapes (electricity, gas, propane, or fuel oil) that cover a majority of the high-impact, market-ready (or nearly market-ready) measures. "Measures" refers to energy efficiency variables that can be applied to buildings during modeling.

An *end-use savings shape* is the difference in energy consumption between a baseline building and a building with an energy efficiency, electrification, or demand flexibility measure applied. It results in a time-series profile that is broken down by end use and fuel (electricity or on-site gas, propane, or fuel oil use) at each time step.

ComStock is a highly granular, bottom-up model that uses multiple data sources, statistical sampling methods, and advanced building energy simulations to estimate the annual subhourly energy consumption of the commercial building stock across the United States. The baseline model intends to represent the U.S. commercial building stock as it existed in 2018. The methodology and results of the baseline model are discussed in the final technical report of the [End-Use Load Profiles](https://www.nlr.gov/buildings/end-use-load-profiles.html) project.

This documentation focuses on a single end-use savings shape measure---Improved Fan Scheduling and Unoccupied Outdoor Air Control.

This measure shuts off outdoor air supply for ventilation during periods when buildings are unoccupied for an extended time (i.e., overnight), as well as aligning fan operating schedules with the occupancy of the zones that the fans serve. Air-side economizing can still take place, but no minimum outdoor air requirement is enforced. This saves energy through reduction in fan speed and the avoidance of thermal loads to condition outdoor air. This measure is applicable to air handling unit (AHU)-based systems that do not currently have ventilation scheduled off during unoccupied times.

Note that some buildings with ventilation scheduled off at night in their primary heating, ventilating, and air conditioning (HVAC) systems have dedicated systems serving other spaces (such as data centers) that have constant schedules applied to their minimum outdoor air levels. These secondary system schedules were changed to reflect building occupancy as part of this measure, but because the minimum outdoor air levels for these spaces was generally set to zero, this schedule change did not affect actual operations and produced only a minor (\<0.01%) change in site energy use.

The Improved Fan Scheduling and Outdoor Air Control measure demonstrates 3.5% total site energy savings (150 TBtu) for the U.S. commercial building stock modeled in ComStock (Figure 4). The savings are primarily attributed to:

-   **5.6%** stock **heating gas** savings (50 TBtu)

-   **3.3%** stock **cooling electricity** savings (24 TBtu)

-   **14.5%** stock **fan electricity** savings (87 TBtu).

The Improved Fan Scheduling and Outdoor Air Control measure demonstrates between 7 and 9 MMT of greenhouse gas emissions avoided for the three grid electricity scenarios presented, as well as 3 MMT of greenhouse gas emissions avoided for on-site natural gas consumption. This constitutes a reduction in greenhouse gas emissions of 3%-4% depending on the scenario.

Energy savings associated with this measure are highly dependent on baseline conditions regarding AHU scheduling and outdoor air control.

# Acknowledgments
The authors would like to acknowledge the valuable guidance and input provided by the ComStock<sup>™</sup> team.

# 1. Introduction

This documentation covers the Improved Fan Scheduling and Outdoor Air Control upgrade methodology and briefly discusses key results. Results can be accessed on the ComStock data lake at "[end-use-load-profiles-for-us-building-stock](https://data.openei.org/s3_viewer?bucket=oedi-data-lake&prefix=nrel-pds-building-stock%2Fend-use-load-profiles-for-us-building-stock%2F2023%2F)" or via the Data Viewer at [comstock.nlr.gov](https://comstock.nlr.gov/).

| **Measure Title**      | Improved Fan Scheduling and Outdoor Air Control                                                                                                                                                                                                                                                                    |
| **Measure Definition** | This measure shuts off outdoor air during extended periods of no occupancy (generally overnight), except when air-side economizing is activated, and aligns heating, ventilating, and air conditioning (HVAC) operating hours with building occupancy.                                                             |
| **Applicability**      | This measure is applicable to all air handling unit (AHU)-based systems in ComStock that have ventilation provided during unoccupied periods. This measure  is applicable to about 58% of the floor area modeled in ComStock.                                                                                      |
| **Not Applicable**     | This measure is not applicable to buildings modeled without air distribution systems, or AHU-based systems that already shut off ventilation during unoccupied periods. This measure is also not applicable to systems that operate 24/7 due to process requirements (such as patient-serving areas of hospitals). |
| **Release**            | 2024 Release 1: 2024/comstock _amy2018_release_1/                                                                                                                                                                                                                                                                  |

# 2.  Technology Summary

Commercial buildings are required to provide adequate ventilation during occupied periods. As a result of lack of adequate controls or faults, many commercial buildings continue to provide ventilation during unoccupied periods (such as overnight), contributing to energy waste through increased fan operation and unnecessary conditioning of outdoor air. An analysis of building automation system (BAS) data from 843 commercial buildings found that only 23% of AHUs in the sample fully aligned with ASHRAE 90.1 requirements, and that 27% of AHUs in the sample operated continuously (CaraDonna & Dombrovski 2022). Similarly, a study of 215 AHUs at new and recently renovated buildings in California in 2002 found that 30% had continuously operating ventilation systems (Jacobs 2003).

In commercial buildings, a space temperature setback should be implemented during unoccupied periods, with the fans controlled to cycle to meet the space temperature setpoint, with no provision of outdoor air during unoccupied periods apart from airside economizing. The outdoor air damper can be scheduled closed overnight, and "permitted" to open only during conditions suitable for economizing (Pacific Northwest National Laboratory N.D.). Exhaust fans should also be scheduled off overnight to avoid creating additional heating or cooling loads through induced infiltration (Pacific Northwest National Laboratory N.D.).

Provision of outdoor air during unoccupied periods can occur due to a lack of scheduling for HVAC equipment, schedules that do not reflect the building's operating hours, or more general faults such as outdoor air dampers that are "stuck" at an open or partially open position and fail to actuate (Fernandez, Katipamula, Wang, et al. 2017). Many BAS offer "optimum start" or "optimum stop" logic to start morning warmup (or cooldown) no earlier than necessary to meet the building's setpoints before occupancy begins, or to allow the building to "coast" without AHU operation at the end of the day, if outdoor air temperatures are moderate ((Fernandez, Katipamula, Wang, et al. 2017, Murphy 2006).

Fernandez, Katipamula, Wang et al. analyzed the effects of applying various energy efficiency and retro-commissioning measures, including "outdoor air damper control," through a modeling analysis, and extrapolated the results to represent the estimated prevalence of such faults in common existing buildings in the U.S. commercial building stock. The outdoor air damper control measure included both the eliminating of ventilation during unoccupied hours and ensuring that adequate ventilation was provided during occupied periods. Operating schedules were applied based on a single deterministic schedule for all buildings of a given type. Due to the assumption of some building types, such as schools, currently being under-ventilated, this measure resulted in a slight increase of site energy use in the aggregate (an increase of about 0.3%) but did result in energy savings in certain building types, with 7.3% site energy savings identified in small office buildings (Fernandez, Katipamula, Wang, et al. 2017).

A building's actual operating schedule, and the schedules, if any, currently governing HVAC operation, are key factors in influencing the calculation of energy savings from implementation of ventilation control during unoccupied periods. Some buildings, such as offices and retail, have well-defined daily and weekly operating schedules. Existing ventilation levels are also an important factor in influencing energy savings, if any, from implementation of ventilation control during unoccupied periods, as illustrated by Fernandez, Katipamula, Wang, et al. (2017). Implementation of a BAS in buildings that currently lack one may be necessary to facilitate long-term persistence of ventilation control during unoccupied periods. Per data from the 2018 Commercial Buildings Energy Consumption Survey, 42% of commercial building floor area in the United States is controlled by a BAS (U.S. Energy Information Administration 2018). In buildings with a BAS, scheduling ventilation off during unoccupied hours can generally be accomplished through the BAS itself. The schedule can fail to persist over time if schedules are modified by operators or overridden to address perceived problems with thermal comfort or to accommodate short-term changes in occupancy patterns. (In addition, many buildings increased day and nighttime ventilation rates during the COVID-19 pandemic in response to CDC guidance encouraging higher ventilation rates to mitigate spread of the virus. (CDC, 2023). ) Some BAS offer occupants the ability to override temperature setbacks for a pre-defined period, which allow for localized control for users that may be in the building during "off" hours, but should be carefully monitored to avoid undermining the effectiveness of schedules overall. The energy savings from this measure can also be undermined due to other controls faults, such as "stuck" outdoor air dampers that fail to actuate properly.

Using airside economizing to meet cooling setpoints during unoccupied periods is an appropriate energy-efficiency strategy and permitted by ASHRAE 90.1, although the focus of this measure is on ventilation control during extended periods during which an entire building is unoccupied, such as overnight, and not on short periods of time during the day when an individual space may be unoccupied. Demand-controlled ventilation and other strategies can address ventilation control during these shorter unoccupied periods at the space level.

# 3. ComStock Baseline Approach

ComStock models all buildings conditioned with commercial systems as being ventilated per the requirements of ASHRAE 62.1, with requirements based on floor area, occupancy, and exhaust rates, for a given space type (Parker 2023). The small subset of buildings conditioned with residential systems is not modeled as having mechanical ventilation. Residential systems are not in the scope of applicability of this measure, as they typically do not provide consistent outdoor air ventilation.

ComStock generally uses the distributions reported by CaraDonna and Dombrovski (2022) to characterize AHU operation during unoccupied hours under three schemes. Under Scheme 1, AHUs are scheduled on and operating during unoccupied hours. Under Scheme 2, AHUs are scheduled off during unoccupied hours, with fans cycling to meet setpoints and bringing in outdoor air when they operate. Under Scheme 3, AHUs are scheduled off during unoccupied hours and fans cycle without ventilation to meet setpoints (Parker 2023). Distributions from CaraDonna and Dombrovski (2022) are applied for each building type described in Table 24 in the ComStock documentation (Parker 2023). Building types with fewer than 25 samples in the underlying dataset analyzed by CaraDonna and Dombrovski are assigned a scheme based on the aggregate distribution across building types. These building types with limited representation in the underlying dataset include warehouses, hotels, and health care facilities. In the aggregate, 27% of buildings are assigned Scheme 1, 50% Scheme 2, and 23%, Scheme 3 (Parker 2023). Hospitals and outpatient care facilities were not represented in the underlying data set and are not subject to these ventilation schemes in ComStock (Parker 2023). These distributions are also not applied to schools, because the application of the distributions to school building ventilation resulted in notably higher energy consumption than that reported for schools in the Commercial Buildings Energy Consumption Survey, suggesting that the distributions were not broadly representative of scheduling of ventilation in schools.

# 4.  Modeling Approach

## 4.1. Applicability

In general, this measure is applicable to AHU-based (packaged single zone and variable air volume \[VAV\] systems) that do not currently have ventilation scheduled off overnight (that is, are not currently controlled by Scheme 3), which account for about 45% of the sample in weighted floor area. Figure 1 shows the prevalence of HVAC system types in ComStock to which this measure is applicable. This measure is not applicable to hospitals and outpatient health care facilities, because they are not represented in the underlying data set from which information on ventilation schedules was obtained, and also is not applicable to schools, based on evidence that the distributions are not broadly representative for them (Parker 2023). This measure will also not be applied to any other spaces operating with 24/7 occupancy or ventilation requirements, as this would result in no change in operations.

This measure is also not applied to dedicated outdoor air systems, as these systems are already scheduled in ComStock to align their operation with the building's occupied hours. This measure is not applicable to zone-level systems such as packaged terminal air conditioning units or packaged-terminal heat pumps, because such units often do not offer the ability to schedule operations. Note that implementation of this measure assumes that a motorized damper is retrofitted to control outdoor air flow if one is not already present.

Some buildings with ventilation scheduled off at night in their primary HVAC systems have dedicated systems serving other spaces (such as data centers) that have constant schedules applied to their minimum outdoor air levels. These secondary system schedules were changed to reflect building occupancy as part of this measure, but because the minimum outdoor air levels for these spaces were generally set to zero, this schedule change did not affect actual operations and produced only a minor (\<0.01%) change in site energy use.

{:refdef: style="text-align: center;"}
![](media\oa_control_image1.png)
{:refdef}

{:refdef: style="text-align: center;"}
Figure 1. Prevalence of HVAC system types to which this measure is applicable (note that chilled water is abbreviated "CHW" and heating hot water "HHW.")
{:refdef}

## 4.2. Technology Specifics Such as Sizing, Performance, and Configuration

This measure will iterate through air loops in a building and set the minimum outdoor air schedule for each Controller:OutdoorAir to be identical to the availability manager schedule for the air loop, so that no requirement for a minimum outdoor air level for ventilation is applied when the area served by the air loop is unoccupied. Because this is simply setting the minimum level of outdoor airflow to zero, it continues to allow for airside economizing.

ASHRAE 90.1. permits use of outdoor air during unoccupied periods for airside economizing. Specifically, ASHRAE 90.1 2022 (Section 6.4.3.4.2) provides an exception to the requirement for ventilation control during unoccupied periods for "when the supply of outdoor air reduces energy costs," or when outdoor air must be supplied to meet code requirements (ASHRAE 2022).

This measure will also set the availability schedule of air loops to align with occupancy of their corresponding zones and create an availability manager for each air loop to configure it to cycle on if any connected zone the current temperature differs from the setpoint by more than one-half of a pre-set tolerance value.

## 4.3. Greenhouse Gas Emissions

Three electricity grid scenarios are presented to compare the emissions of the ComStock baseline and the window replacement scenario. The choice of grid scenario will impact the grid emissions factors used in the simulation, which determines the corresponding emissions produced per kilowatt-hour. Two scenarios---Long-Run Marginal Emissions Rate (LRMER) High Renewable Energy (RE) Cost 15-Year and LRMER Low RE Cost 15-Year---use the Cambium data set, and the last uses the eGrid data set (NREL, 2022), (US EPA, 2022). All three scenarios vary the emissions factors geospatially to reflect the variation in grid resources used to produce electricity across the United States. The Cambium data sets also vary emissions factors seasonally and by time of day. This study does not imply a preference for any particular grid emissions scenario, but other analysis suggests that the choice of grid emissions scenario can impact results (Present, Gagnon, Wilson, et al. 2022). Emissions due to the on-site combustion of fossil fuels use the emissions factors shown in Table 1, which are from Table 7.1.2(1) of draft American National Standards Institute/Residential Energy Services Network/International Code Council 301 (Vijayakumar et al., 2022). To compare total emissions due to both on-site fossil fuel consumption and grid electricity generation, the emissions from a single electricity grid scenario should be combined with all three on-site fossil fuel emissions.

 Table 1. On-Site Fossil Fuel Emissions Factors 

{:refdef: style="text-align: center;"}
![](media/onsite_fossil_fuels_efs_table.png)
{:refdef}

## 4.4. Limitations and Concerns

The prevalence of the underlying faults of ventilation operation during unoccupied periods is based on an analysis of a dataset representing over 5,700 AHUs (CaraDonna 2022). While this represents the best available analysis of such data, there may be limitations in the applicability of distributions of faults from this data to classes of building in the U.S. commercial building stock (The distributions are not applied to building types that were not well-represented in the underlying data set, and are also not applied to schools, based on evidence that the data set was not broadly representative for school buildings.). Building owners are often reluctant, for a variety of reasons, to provide access to BAS data.

In practice, some commercial buildings do not provide adequate mechanical ventilation during occupied periods (Fernandez 2017). Thus, the baseline assumption in ComStock of adequate ventilation being provided by commercial systems could overestimate the savings that would be observed in practice by appropriate ventilation scheduling, as in under-ventilated buildings, this would likely occur in conjunction with retro-commissioning to ensure adequate ventilation during occupied periods.

Note that some buildings with ventilation scheduled off at night in their primary HVAC systems have dedicated systems serving other spaces (such as data centers) that have constant schedules applied to their minimum outdoor air levels. These secondary system schedules were changed to reflect building occupancy as part of this measure, but because the minimum outdoor air levels for these spaces were generally set to zero, this schedule change did not affect actual operations and produced only a minor (\<0.01%) change in site energy use.

# 5. Output Variables

Table 2 includes a list of output variables that will be calculated in ComStock. These variables are important in terms of understanding the differences between buildings with and without the Improved Fan Scheduling and Outdoor Air Control measure applied. These output variables can also be used to understand the economics of the upgrade (e.g., return on investment) if cost information (i.e., material, labor, and maintenance costs for technology implementation) is available.

Table 2. Output Variables Calculated From the Measure Application

| Variable Name                                | Description                                                                                                                                   |
|----------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------|
| air_system_outdoor_air_minimum_flow_fraction | A distribution of this variable, or a count of zero values, can help confirm that the minimum outdoor air flow rate is set to zero overnight. |
| air_system_outdoor_air_mass_flow_rate        | A distribution, or average, of this variable can help confirm that the minimum outdoor air flow rate is set to zero overnight.                |

# 6. Results

In this section, results are presented both at the stock level and for individual buildings through savings distributions. Stock-level results include the combined impact of all the analyzed buildings in ComStock, including buildings that are not applicable to this measure. Therefore these results do not necessarily represent the energy savings of a particular or average building. Stock-level results should not be interpreted as the savings that a building might realize by implementing the measure.

Total site energy savings are also presented in this section. Total site energy savings can be a useful metric, especially for quality assurance/quality control, but this metric on its own can have limitations for drawing conclusions. Further context should be considered, as site energy savings alone do not necessarily translate proportionally to savings for a particular fuel type (e.g., gas or electricity), source energy savings, cost savings, or greenhouse gas savings. This is especially important when a measure impacts multiple fuel types or causes decreased consumption of one fuel type and increased consumption of another. Many factors should be considered when analyzing the impact of an energy efficiency or electrification strategy, depending on the use case.

## 6.1. Single Building Measure Tests

To demonstrate the functionality of the measure on a single building, this measure was applied to a modified version of the packaged VAV warehouse model for Climate Zone 2A. The model uses a 10-zone packaged VAV system, with direct expansion cooling coils, and hot water coils for tempering supply air and for reheat. The "baseline" version of the model, reflecting conditions without the measure applied, was configured with an "always on" minimum outdoor air schedule and an air loop schedule based on the "vent cycle" (cycle ventilation on during unoccupied periods to meet thermostat setpoints) approach.

The model was run with the Port Arthur, Texas, weather file. Measure tests were run to confirm that the version of the model with the measure applied ran successfully, and that the system outdoor air controller was now configured with a minimum outdoor air level set by a "Schedule: Ruleset" and not a "Schedule: Constant" object to reflect that minimum outdoor air levels are time-dependent. Time-series results were also evaluated for the example building model.

Figure 2 shows outdoor air flow through the air handling unit for a representative period of about 1 week (corresponding to January 1--8) in the base case and after the measure was applied. After applying the measure, the AHU does not bring in outdoor air during unoccupied periods, as neither of the models have an economizer enabled (If an economizer were enabled, economizing during unoccupied periods to meet temperature setpoints would be permitted.). The results shown in Figure 2 confirm this effect.

{:refdef: style="text-align: center;"}
![](media\oa_control_image2.png)
{:refdef}

{:refdef: style="text-align: center;"}
Figure 2. Outdoor air mass flow rate for AHU with fan cycling with ventilation ( base case), and fan cycling without ventilation (after measure applied)
{:refdef}

Figure 3 shows a comparison of energy use in end uses affected by the measure (electricity for cooling and fans and natural gas heating), before and after implementation in the example warehouse building. Because the baseline model uses a "vent cycle" approach (the supply fans cycle to meet load during unoccupied hours), a reduction in fan operating hours is not expected through application of the measure. There was a reduction in heating and cooling energy use observed, as expected, through the reduction in load from tempering outdoor air. The reduction in fan energy use can also be attributed to this reduction in heating and cooling load.

{:refdef: style="text-align: center;"}
![](media\oa_control_image3.png)
{:refdef}

{:refdef: style="text-align: center;"}
Figure 3. Comparison of key energy end uses before and after implementing Improved Fan Scheduling and Outdoor Air Control measure in example building
{:refdef}

## 6.2. Stock Energy Impacts

The Improved Fan Scheduling and Outdoor Air Control measure demonstrates 3.5% total site energy savings (150 TBtu) for the U.S. commercial building stock modeled in ComStock (Figure 4). The savings are primarily attributed to:

-   **11%** stock **fan** savings (58.3 TBtu).

-   **7.2%** stock **heating electricity** savings (12.7 TBtu)

-   **5.6%** stock **heating gas** savings (50.5 TBtu)

-   **2.7%** stock **cooling** savings (18.3 TBtu).

{:refdef: style="text-align: center;"}
![](media\oa_control_image4.jpeg)
{:refdef}

{:refdef: style="text-align: center;"}
Figure 4. Comparison of annual site energy consumption between the ComStock baseline and the Improved Fan Scheduling and Outdoor Air Control measure scenario across the building stock
{:refdef}

Energy consumption is categorized both by fuel type and end use.

{:refdef: style="text-align: center;"}
![](media\oa_control_image5.jpeg)
{:refdef}

{:refdef: style="text-align: center;"}
Figure 5. Comparison of annual site energy consumption between the ComStock baseline and the Improved Fan Scheduling and Outdoor Air Control measure scenario in applicable buildings only
{:refdef}

Energy consumption is categorized both by fuel type and end use.

Figure 5 shows the comparison of site energy consumption disaggregated by end use with and without the measure applied for buildings that were applicable to the upgrade. The aggregate site energy savings observed (9.5%) in buildings in which the measure was applicable (those with some degree of ventilation during unoccupied periods), is similar to the percentage site energy savings of 7.3% observed by Fernandez et al. (2017) in small office buildings. The implementation of the measure by Fernandez et al. (2013) also involved ensuring that minimum ventilation levels were met, which resulted in a slight site energy penalty (0.3%) across the entire building stock, because some buildings were under-ventilated in baseline conditions. Note that ComStock in general assumes that minimum ventilation levels are met in baseline conditions, and this analysis did not extend the measure to building types in which adequate data were not available to characterize the prevalence of existing ventilation control faults (hospitals and outpatient health facilities). Note that this measure was also not applied to schools, as past ComStock analysis has indicated that available data on fault prevalence may not be widely representative.

Note that implementation of this measure also involved control of AHU fans to operate during unoccupied periods only when a call for heating or cooling occurred, as well as the elimination of ventilation during unoccupied periods. Thus, application of this measure resulted in notable fan energy savings, as well as heating energy savings, with a smaller fraction of energy savings from cooling, because the majority of unoccupied hours for most buildings occur at nighttime, when outdoor air is more likely to impose a heating load than cooling load in many climates. Additionally, in buildings with gas or electric resistance heating, a reduction in each heating load generally results in greater site energy savings than the same reduction in cooling load, due to the higher coefficient of performance of direct expansion cooling systems (often 3--4), than the efficiencies of natural gas (often 80%) or electric resistance (exactly 100%) heating. The fraction of fan energy savings (26%) in buildings to which the measure was applicable is notable but expected. In the buildings in which the "upper bound" of relative energy savings will occur (those with "fan on-vent" schedules in unoccupied periods), the measure could result in reduction of fan operating hours by as much as 60% (based on the average operating hours among applicable buildings in this sample for the small office building). The small office building is one of the most common building types with this measure applicable.

Some buildings with ventilation scheduled off at night in their primary HVAC systems have dedicated systems serving other spaces (such as data centers) that have constant schedules applied to their minimum outdoor air levels. These secondary system schedules were changed to reflect building occupancy as part of this measure, but because the minimum outdoor air levels for these spaces were generally set to zero, this schedule change did not affect actual operations and produced only a minor (\<0.01%) change in site energy use.

## 6.3. Stock Greenhouse Gas Emissions Impact

Figure 6 shows a comparison of aggregate greenhouse gas emissions under several different scenarios reflecting different levels of carbon intensity of electricity. Note that these scenarios are presented for illustrative purposes. Under the three scenarios shown here, implementing this measure reduces the carbon emissions by 3%--4% relative to the base case. The emissions associated with electricity vary notably among these scenarios, due to the varying assumptions for carbon intensity of electricity. Natural gas heating energy savings account for 35% of the site energy savings from this measure at the full stock level, and thus the emissions reduction share associated with that portion is not dependent on carbon intensity of electricity, leading to a slightly higher proportionate emissions reduction under the scenarios assuming lower electric carbon intensity.

{:refdef: style="text-align: center;"}
![](media\oa_control_image6.jpeg)
{:refdef}

{:refdef: style="text-align: center;"}
Figure 6. Greenhouse gas emissions comparison of the ComStock baseline and the Improved Fan Scheduling and Outdoor Air Control measure
{:refdef}

Three electricity grid scenarios are presented: Cambium LRMER High Renewable Energy Cost 15-Year, Cambium LRMER Low Renewable Energy Cost 15-Year, and eGrid.

## 6.4. Site Energy Savings Distributions

In proportional terms, the primary end uses in which energy savings were observed from this measure were fans, space heating (electricity and natural gas), and pumps, with more minor savings from cooling (with the 75<sup>th</sup> percentile under 10%). Heating and cooling energy savings result from the reduced load from outdoor air during unoccupied periods, and fan energy savings generally result from improved scheduling (in buildings with continuous supply fan operation in the baseline). Pump energy savings result from reduced heating and cooling loads in buildings with hydronic systems. Due to the relatively small energy use by pumps in aggregate across the sample, the pump energy savings are not notable at the stock level.

The degree of energy savings (in all relevant end uses) resulting from this measure is highly dependent on baseline assumptions regarding the prevalence of various faults in outdoor air control, reflected in supply fan and outdoor air schedules. The baseline assumptions in ComStock related to outdoor air control and prevalence of various related faults are documented in CaraDonna (2022) and Parker (2023). A separate HVAC control variability measure implements these configurations in ComStock that were applicable to the upgrade. The unoccupied outdoor air control strategies reflected in the ComStock baseline include fans running continuously at nighttime, with outdoor air ("fan on vent"), fans cycling at night while bringing in outdoor air ("night fan cycle vent") and fans cycling at night without bringing in outdoor air ("night fan cycle no vent"). The "night fan cycle no vent" option is effectively implemented by this measure, and thus this measure was not applied to buildings already having that configuration. The "fan on vent" configuration presents the greater opportunity for energy savings, and "night fan cycle vent," a lesser opportunity for savings.

In this sample, 20% of buildings had retained their underlying nighttime outdoor air control strategy, 17% had "night fan cycle no vent" implemented through the variability measure, 40% had "night fan cycle vent" implemented through the variability measure, and 23% had "night fan on vent" implemented through the variability measure.

Figure 8 shows the distribution of savings percentage by end use and fuel type. The gas heating penalty that occurs in some buildings is generally the result of the loss of fan heat due to improved scheduling. Though fans continue to operate when heating or cooling loads exist, fan operation during other periods can also offset the load on the heating coil in certain conditions (This penalty, where it occurs, is generally offset by fan energy savings.). This effect on natural gas heating energy use was observed most often in packaged single-zone systems with a gas coil, though it also manifested as an electric heating penalty in packaged systems with heat pumps or with electric resistance heating coils (Only two buildings experienced a penalty in heating energy use from district heating.).

In the cases of the highest absolute gas heating penalty, the magnitude in energy consumption of the gas heating penalty was always less, and often notably less, than the observed fan energy savings. In practice, the upper bound on the loss of useful fan heat is the fan energy savings, and the upper bound on the expected heating energy penalty is this value divided by the coil heating efficiency. In cases of the highest absolute electric heating energy penalty, with several exceptions, the heating energy penalty was notably less than the fan energy savings. With these systems, the upper bound on the expected heating penalty is the fan energy savings (in the case of electric resistance coils, with an efficiency of one), or the fan energy savings divided by the heating coefficient of performance of the heat pump under the same conditions as the fan energy savings accrued. Heating coefficient of performance of an air-source heat pump varies as a function of outdoor air temperature and other factors. The distribution of fan energy savings over periods in which heating is required and the fan heat is useful is dependent on climate zone and building loads. These observations for gas and electric heating energy penalties confirm that their magnitudes are generally reasonable relative to the observed fan energy savings.

The high proportionate penalty in pump energy use generally occurred in buildings with low baseline pump energy and was associated with a heating penalty resulting from the reduction in fan heat in buildings with hydronic heating systems. These penalties, where they occurred, were generally offset by fan energy savings. The increase in fan energy that occurs in some buildings is generally due to operation at higher fan power because of increased cooling load during unoccupied periods in systems without economizers. This is discussed in more detail in subsection 5.6. The cooling energy penalty generally occurs in buildings without economizers, in which the reduction in outdoor air ventilation during unoccupied periods results in compensation with additional mechanical cooling energy use.

The minor changes in water systems- and refrigeration-related energy use are due to small differences in zone air conditions resulting from applying this measure. The minor variation in lighting energy use observed in a very small number of buildings is a minor unexpected result that is being investigated. These minor variations do not affect the aggregate results or the results of other end uses.

{:refdef: style="text-align: center;"}
![](media\oa_control_image7.jpeg)
{:refdef}

{:refdef: style="text-align: center;"}
Figure 7. Percentage site energy savings distribution for ComStock models with the Improved Fan Scheduling and Outdoor Air Control measure applied by end use and fuel type
{:refdef}

The data points that appear above some of the distributions indicate outliers in the distribution, meaning they fall outside 1.5 times the interquartile range. The value for *n* indicates the number of ComStock models that were applicable for energy savings for the fuel type category.

Figure 8 shows the distribution of overall site energy savings by HVAC system type. The most common HVAC system types in the sample to which this measure was applied (package single-zone air conditioner \[PSZ-AC\] with gas coil, PSZ-AC with electric coil, and packaged VAV with gas boiler reheat) generally had similar ranges of proportionate site energy savings. The prior operating conditions of the fan during unoccupied periods are a notable driver of energy savings from this measure, and one would expect a higher potential for energy savings from constant-speed systems (such as the PSZ-ACs) than VAV systems, if fans were operating at lower speeds during unoccupied periods in the baseline for ventilation only. Lower fan energy savings would also be expected to reduce the magnitude of any heating energy penalty associated with the measure.

The fan energy penalty appears to occur in central VAV or packaged VAV systems, and primarily in those without economizers that previously had the "night fan cycle vent" control strategy. The elimination of the cooling from outdoor air brought in during unoccupied periods causes the fans to cycle more frequently and at higher flow rates to meet cooling loads in the zone overnight, which often occur in interior zones in large buildings. These buildings still generally had overall net site energy savings from implementing this measure, due to the reduction in load from outdoor air at other times. In the packaged single-zone heat pump (PSZ-HP) systems in this sample that have a site energy penalty resulting from this measure, the energy penalty tends to result from an increased electricity use for heating.

{:refdef: style="text-align: center;"}
![](media\oa_control_image8.jpeg)
{:refdef}

{:refdef: style="text-align: center;"}
Figure 8. Percentage site energy savings distribution for ComStock models with the applied Improved Fan Scheduling and Outdoor Air Control by HVAC system.
{:refdef}

The data points that appear above some of the distributions indicate outliers in the distribution, meaning they fall outside 1.5 times the interquartile range. The value for *n* indicates the number of ComStock models that were applicable for energy savings for the HVAC system type category.

Figure 9 shows the distribution of site energy use intensity savings from the Unoccupied OA control measure by building type. The buildings with the highest proportional site energy savings generally had "night fan on vent" control in the baseline. This control scheme is expected, as it offers the greatest potential for fan, heating, and cooling energy savings through improved ventilation controls (Only 7 small hotels and 43 hospitals appeared in this sample, making it difficult to draw conclusions based on their savings distributions.). The net site energy penalty that is observed in some buildings generally occurred in buildings without economizers, and was due to increased cooling energy use. In buildings without economizers, ventilation during unoccupied periods can, in effect, provide the benefits of economizing at certain times.

{:refdef: style="text-align: center;"}
![](media\oa_control_image9.png)
{:refdef}

{:refdef: style="text-align: center;"}
Figure 9. Percentage site energy use intensity savings distribution for ComStock models with the applied Improved Fan Scheduling and Outdoor Air Control by building type
{:refdef}

The data points that appear above some of the distributions indicate outliers in the distribution, meaning they fall outside 1.5 times the interquartile range. The value for *n* indicates the number of ComStock models that were applicable for energy savings for the building type category.

Figure 10 shows distributions of site energy savings by climate zone. The 75<sup>th</sup> percentile of site energy savings is generally less than 15% across all climate zones, with several exceptions. Note that Climate Zone 3C is located only in California, in which the Database of Energy Efficiency Resources (DEER)\--based modeling methods are applied, making it not directly comparable to other climate zones. (Climate Zone 3B also has a significant number of buildings located in California.) The DEER-based methods reflect California's Title 24 energy code, and entail a different set of assumptions than those applied in the rest of the ComStock models, included related to plug loads, equipment efficiency, and, significantly for this measure, outdoor air ventilation rates. The potential for energy savings from this measure is driven by ventilation rates, baseline control and scheduling conditions in a given building, as well as outdoor air conditions overnight, and energy savings accrue from improved fan scheduling as well as reduction in outdoor air loads, which is reflected in the generally consistent ranges of energy savings across other climate zones.

{:refdef: style="text-align: center;"}
![](media\oa_control_image10.jpeg)
{:refdef}

{:refdef: style="text-align: center;"}
Figure 10. Percentage site energy savings distribution for ComStock models with the applied Improved Fan Scheduling and Outdoor Air Control measure by climate zone
{:refdef}

The data points that appear above some of the distributions indicate outliers in the distribution, meaning they fall outside 1.5 times the interquartile range. The value for *n* indicates the number of ComStock models that were applicable for the climate zone.

## 6.5. Influence of Night Cycle Mode on Energy Savings

As discussed previously, the baseline night cycle mode is a key factor influencing energy savings from implementation of this measure. Figure 11 shows boxplots with these distributions without outliers (at left) and with outliers (at right). Note the differing y-axis scales between the two plots. These results bear out the expectation that those buildings with "night fan on vent" (supply fans operating throughout the night, with outdoor air) will experience the highest potential savings from this measure, and those with "night fan cycle vent" (supply fans cycling at night, with outdoor air), will experience some savings, but to a lesser extent. The range of savings observed for each case reflects the varied share of HVAC energy use (addressed by this measure) relative to overall site energy use and variability due to climate zone, which influences the heating and cooling load imposed by nighttime ventilation, among other factors.

Note that this measure was applicable to some buildings with the "night fan cycle no vent" configuration, if they had constant schedules controlling HVAC systems. In all buildings, these schedules were modified to align with building occupancy schedules. The energy savings from this change can be highly variable as a function of overall site energy use, leading to the wide dispersal of outliers. Note that some buildings with "night fan cycle no vent" schedules governing their primary HVAC systems have dedicated systems serving other spaces (such as data centers) that have constant schedules applied to their minimum outdoor air levels. These schedules were changed to reflect building occupancy as part of this measure, but because the minimum outdoor air levels for these spaces were generally set to zero, this schedule change did not affect actual operations and produced only a minor (\<0.01%) change in site energy use. This explains some of the distribution concentrated at 0% for this group of buildings.

{:refdef: style="text-align: center;"}
![](media\oa_control_image11.png)
{:refdef}

{:refdef: style="text-align: center;"}
![](media\oa_control_image12.png)
{:refdef}

{:refdef: style="text-align: center;"}
Figure 11. Boxplot of site energy savings distributions by night cycle mode for those buildings with night cycle mode changed from default (without outliers at left and with outliers at right), with sample sizes: "night fan cycle no vent"=1678, "night fan cycle vent"=3876, and "night fan on vent"=2230
{:refdef}

*T*he remaining samples retained their original nighttime ventilation control.

## 6.6. Heating Energy Use Implications

In about 3.4% of the buildings to which this measure was applicable, a natural gas heating penalty was observed, primarily due to a reduction in fan heat from improved scheduling that, in those buildings, outweighed energy heating energy savings from the reduction in outdoor air load during unoccupied hours. Figure 11 shows scatterplots of a subset of those buildings, with the natural gas space heating penalty plotted as a function of the fan energy savings. The red line in Figure 11 shows the maximum expected natural gas heating penalty as a function of fan energy use, given an assumption of all fan heat being transferred to the airstream (reflecting conditions in the ComStock models considered), and a natural gas heating efficiency of 80% (typical of natural gas heating coils in air handling units). In Figure 11, the natural gas penalty values generally lie above, and in many cases, significantly above, the red line, indicating that the penalty observed is less than the maximum value expected. This provides a "gut check" on the results (for clarity, the small number of buildings with a natural gas heating penalty and fan energy savings of greater than 60,000 kWh are not shown here).

{:refdef: style="text-align: center;"}
![](media\oa_control_image13.png)
{:refdef}

{:refdef: style="text-align: center;"}
![](media\oa_control_image14.png)
{:refdef}

{:refdef: style="text-align: center;"}
Figure 12. Natural gas heating energy savings (a negative value, reflecting an energy penalty) as a function of fan energy savings for a selection of buildings in which a natural gas heating penalty was observed
{:refdef}

The figure at left is a "zoomed-in" version of the figure at right for clarity. Points are colored by ASHRAE climate zone.

# 7. References

American Society of Heating, Refrigeration, and Air Conditioning Engineers. (2022). *Energy Standard for Sites and Buildings Except Low-Rise Residential Buildings (I-P Edition).* Atlanta, Georgia: ASHRAE. Retrieved from https://www.ashrae.org/technical-resources/bookstore/standard-90-1

CaraDonna, C., & Dombrovski, K. (2022). Air Handling Unit Shutdowns During Scheduled Unoccupied Hours: US Commercial Building Stock Prevalence and Energy Impact. *Journal of Engineering for Sustainable Buildings and Cities*, 3(3): 031003. doi:https://doi.org/10.1115/1.4055887CDC. (2023, May 12). *Ventilation in Buildings*. Retrieved from COVID-10: https://www.cdc.gov/coronavirus/2019-ncov/community/ventilation.html

Fernandez, N., Katipamula, S., Wang, W., & al., e. (2017). *Impacts of Commercial Building Controls on Energy Savings and Peak Load Reduction.* Pacific Northwest National Lab. Retrieved from https://buildingretuning.pnnl.gov/publications/PNNL-25985.pdf

Jacobs, P. (2003). *Small HVAC Problems and Potential Savings Reports (P500-03-082-A-25) .* Sacramento, CA.: California Energy Commission. Retrieved from https://newbuildings.org/wp-content/uploads/2015/11/A-25_Small_HVAC_Probs_Svgs.pdf

Katipamula, S. U. (2021). Prevalence of typical operational problems and energy savings opportunities in U.S. commercial buildings. *Energy and Buidlings, 253*, 111544. doi:https://doi.org/10.1016/j.enbuild.2021.111544

Murphy, J. (2006). *Energy-saving control strategies for rooftop VAV systems.* Trane. Retrieved from https://www.trane.com/content/dam/Trane/Commercial/global/products-systems/education-training/engineers-newsletters/energy-environment/admapn022en_1006.pdf

NREL. (2022, September 2). *"Cambium \| Energy Analysis \| NREL."*. Retrieved from https://www.nlr.gov/analysis/cambium.html.Pacific Northwest National Lab. (N.D. ). *Building Re-Tuning Training Guide: Occupancy Scheduling: Night and Weekend Temperature Set back and Supply Fan Cycling during Unoccupied Hours.* Pacific Northwest National Lab. Retrieved from https://buildingretuning.pnnl.gov/documents/pnnl_sa_85194.pdf

Parker, A. H. (2023). *"ComStock Reference Documentation: Version 1,".* Golden, CO: NREL. Retrieved from https://www.nlr.gov/docs/fy23osti/83819.pdfPresent, E. G. (2022). *Choosing the Best Carbon Factor for the Job: Exploring Available Carbon Emissions Factors and the Impact of Factor Selection.* NREL.

US Energy Information Administration. (2018). *2018 CBECS Survey Data.* Washington DC : US EIA .US EPA. (2022). *Emissions & Generation Resource Integrated Database (eGRID) \| US EPA*. Retrieved 9 2, 2022, from https://www.epa.gov/egrid.

Vijayakumar, G. e. (2022). *ANSI/RESNET/ICC 301-2022 - Standard for the Calculation and Labeling of the Energy Performance of Dwelling and Sleeping Units using an Energy Rating Index.* Oceanside, CA.

# A.  Additional Figures

{:refdef: style="text-align: center;"}
![](media\oa_control_image15.jpeg)
{:refdef}

{:refdef: style="text-align: center;"}
Figure A-1. Site annual natural gas consumption of the ComStock baseline and the measure scenario by census division
{:refdef}

{:refdef: style="text-align: center;"}
![](media\oa_control_image16.jpeg)
{:refdef}

{:refdef: style="text-align: center;"}
Figure A-2. Site annual electricity consumption of the ComStock baseline and the measure scenario by census division
{:refdef}

{:refdef: style="text-align: center;"}
![](media\oa_control_image17.jpeg)
{:refdef}

{:refdef: style="text-align: center;"}
Figure A-3. Site annual natural gas consumption of the ComStock baseline and the measure scenario by building type
{:refdef}

{:refdef: style="text-align: center;"}
![](media\oa_control_image18.jpeg)
{:refdef}

{:refdef: style="text-align: center;"}
Figure A-4. Site annual electricity consumption of the ComStock baseline and the measure scenario by building type
{:refdef}
