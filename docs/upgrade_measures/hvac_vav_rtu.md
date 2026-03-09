---
layout: default
title: Advanced Rooftop Unit Control 
parent: Upgrade Measures
nav_order: 18
published: false
---

# Advanced Rooftop Unit Control 
{: .fw-500 }

Authors: Amy Allen and Chris CaraDonna

# Executive Summary

Building on the successfully completed effort to calibrate and validate the U.S. Department of Energy's ResStock™ and ComStock™ models over the past 3 years, the objective of this work is to produce national data sets that empower analysts working for federal, state, utility, city, and manufacturer stakeholders to answer a broad range of analysis questions.

The goal of this work is to develop energy efficiency, electrification, and demand flexibility end-use load shapes (electricity, gas, propane, or fuel oil) that cover a majority of the high-impact, market-ready (or nearly market-ready) measures. "Measures" refers to energy efficiency variables that can be applied to buildings during modeling.

An *end-use savings shape* is the difference in energy consumption between a baseline building and a building with an energy efficiency, electrification, or demand flexibility measure applied. It results in a time-series profile that is broken down by end use and fuel (electricity or on-site gas, propane, or fuel oil use) at each time step.

ComStock is a highly granular, bottom-up model that uses multiple data sources, statistical sampling methods, and advanced building energy simulations to estimate the annual subhourly energy consumption of the commercial building stock across the United States. The baseline model intends to represent the U.S. commercial building stock as it existed in 2018. The methodology and results of the baseline model are discussed in the final technical report of the [End-Use Load Profiles](https://www.nlr.gov/buildings/end-use-load-profiles.html) project.

This documentation focuses on a single end-use savings shape measure---advanced rooftop unit control. This measure implements variable-speed control of rooftop unit (RTU) fans that are currently constant-speed, and includes options for demand-controlled ventilation and air-side economizing. These features are like the functions offered by advanced rooftop unit control (ARC) retrofit kits. This measure is expected to result in fan energy savings from the multi-speed fan control, fan savings from demand-controlled ventilation, and cooling energy savings economizing, respectively. This measure is applicable to about 39% of the floor area modeled in ComStock.

The Advanced RTU Control measure demonstrates 4.2% total site energy savings (182 trillion British thermal units \[TBtu\]) for the U.S. commercial building stock modeled in ComStock (Figure 8). The savings are primarily attributed to:

-   **26%** stock **fan electricity** savings (137 TBtu)

-   **4%** stock **cooling electricity** savings (24 TBtu)

-   **2%** stock **heating gas** savings (18 TBtu).

The Advanced RTU Control measure demonstrates between 7 and 17 million metric tons (MMT) of greenhouse gas emissions avoided for the three grid electricity scenarios presented, as well as one MMT of greenhouse gas emissions avoided for on-site natural gas consumption. This constitutes a reduction in greenhouse gas emissions of 4% across all grid scenarios. The levels of energy savings by end use in applicable buildings are in line with past modeling and field studies of ARC.

# Acknowledgments 
The authors would like to acknowledge the valuable guidance and input provided by the ComStock team.

# 1. Introduction

This documentation covers "Advanced Rooftop Unit Control" upgrade methodology and briefly discusses key results. Results can be accessed on the ComStock™ data lake at "[end-use-load-profiles-for-us-building-stock](https://data.openei.org/s3_viewer?bucket=oedi-data-lake&prefix=nrel-pds-building-stock%2Fend-use-load-profiles-for-us-building-stock%2F2023%2F)" or via the Data Viewer at [comstock.nlr.gov](https://comstock.nlr.gov/).

| **Measure Title**      | Advanced Rooftop Unit Control                                                                                                                                                                                  |
| **Measure Definition** | This measure retrofits existing packaged single-zone systems, which are modeled in ComStock with constant-volume fans, with variable speed fans, and demand-controlled ventilation and integrated economizing. |
| **Applicability**      | This measure is applicable to buildings with packaged single-zone systems, accounting for about 39% of the floor area modeled in ComStock.                                                                     |
| **Not Applicable**     | This measure is not applicable to buildings with built-up air   handling units, or without air distribution systems.                                                                                           |
| **Release**            | 2024 Release 1: 2024/comstock_amy2018_release_1/                                                                                                                                                               |

# 2.  Technology Summary

Rooftop units (RTUs) are the most common HVAC system type in commercial buildings in the United States (US EIA, 2012). Older RTUs, especially single-zone systems, typically use constant-speed fans, which results in wasted energy during periods of low loads (Cai & Braun, 2018). In these systems, supply air temperature is varied by cycling heating or cooling coils on and off, or by adjusting the coil output. Most RTUs over 10 tons in cooling capacity operate with at least two compressors, providing at least two stages of cooling, and some larger RTUs have four compressors (Doebber I. D., 2016). Commercial building HVAC systems fans must operate continually during occupied periods to provide ventilation air, and operating at higher-than-required fan speeds when only ventilation is required wastes fan energy (Cai & Braun, 2018). Advanced rooftop unit controls (ARC) refer to products that retrofit variable-frequency (i.e., variable-speed) drives (VFDs) onto existing constant-speed fans in single-zone RTUs.

VFDs change the frequency and voltage of the input power to control a motor's speed.[^1] ARC may include other energy-saving features as well, including air-side economizing and demand controlled ventilation (DCV) (Energy Solutions, 2019 ).[^2] Air-side economizing involves the increased use of outdoor air, when conditions are favorable, to reduce the requirement for mechanical cooling. Air-side economizers can be controlled based on enthalpy or dry-bulb temperature, based on a relative threshold (compared with return air), or an absolute threshold applied to outdoor air (Pacific Northwest National Lab, 2021). DCV controls ventilation levels in a space based on ventilation requirements. In typical applications, the varying component of a ventilation requirement relates to human occupancy, and DCV systems typically use carbon dioxide sensors for control, as a surrogate for other occupancy-related pollutants (Lawrence Berkeley National Laboratory, N.D. ).

Through a modeling analysis, Fernandez et al. studied the effects of applying various energy efficiency and retro-commissioning measures, including "advanced RTU controls." The results of their analysis were extrapolated to reflect the estimated applicability of such measures in common existing buildings in the U.S. commercial building stock. (The energy efficiency measures also included remedying common HVAC faults, including malfunctioning sensors, leaking valves, low refrigerant charge, malfunctioning economizer controls, and others.) To model the savings associated with these advanced RTU controls, Fernandez et al. incorporated a multi-speed fan into the analysis and estimated a site energy savings of 1.3% across the commercial building stock. In this case, the advanced RTU controls were applied only to buildings assumed to have single-zone packaged units. In building types with single-zone packaged units, this estimated site energy savings was higher---3.5% in strip malls and 3.2% in small office buildings (Fernandez, 2017).

Transformative Wave's Catalyst product is one example of an ARC unit, and it has been studied by several different organizations.[^3] The New York State Energy Research and Development Authority (NYSERDA) funded a study of the Catalyst in a deployment in New York State.[^4] The Catalyst includes a supply fan VFD, controls to implement economizing (integrated economizing with differential dry-bulb control, and an ambient dewpoint temperature lockout), and DCV (Doebber I. D., 2016). The Catalyst also incorporates a "predictive economizing control" that initiates economizing when a cooling load is anticipated (Doebber I. D., 2016). The Catalyst's DCV feature measures carbon dioxide concentration in the return air (Energy Solutions, 2019 ). The Catalyst also incorporates a building automation system with a web-based interface, which monitors the RTUs equipped with the system, and provides some automated fault detection and diagnosis capability (Doebber I. D., 2014).

A study by Energy Solutions considered 186 installations that used the Catalyst product and found a range of 5%--50% RTU electricity savings and 1%--10% RTU gas savings, with an average RTU electricity savings of 35% and average RTU gas savings of 5%. Of the electrical energy savings, 90% was attributed to the fan VFD (with about 10% attributed to improved economizing, and less than 1% attributed to DCV) (Energy Solutions, 2019 ). (Single-zone HVAC systems may offer less potential than multi-zone systems for energy savings from DCV due to the correlation between occupancy and cooling load in a given zone, and the lack of potential for load and occupancy diversity across the system.) Wang et al. (2013) performed a field evaluation of the Catalyst in 66 RTUs (included packaged heat pumps and conventional packaged units with direct expansion (DX) cooling and gas heating) in four U.S. climate zones and found an average of 57% RTU-level energy savings, predominately driven by fan energy savings (Wang, 2013). Heat pump RTUs examined in their study had a lower absolute average of energy savings per hour of operation, because the heat pump units they studied tended to be smaller than packaged units with gas heating (Wang, 2013). (Note that Wang et al. did not include gas energy savings from heating in their analysis.) An energy services contractor's study of 175 RTU retrofits installed in the Pacific Northwest identified savings of at least 30% RTU energy for cases in which VFDs were installed on the units (some small units were retrofitted without VFDs) (Weissert & Smith, 2018).

The National Renewable Energy Laboratory (NREL) studied the performance of the Catalyst with a field demonstration across 11 RTUs at Navy installations in Hawaii and Guam (Doebber I. D., 2014), and found reductions of 5%--15% in building-level HVAC energy use. This is the first known field demonstration of advanced RTU controls in a hot/humid climate (1A). The existing RTUs operated with fixed-position outdoor air dampers, and no air-side economizing. The humid climates present in both Hawaii and Guam mean that the outdoor air dewpoint temperature generally exceeds the 60°F lock-out typically programmed into the Catalyst's economizer control. NREL had this lock-out overridden in this demonstration for purposes of analyzing the economizer's impact in this climate (Doebber I. D., 2014). Through this study, NREL found that the ARC resulted in lower indoor relative humidity compared with the baseline, due to better control of outdoor air (including shut-off of outdoor air overnight), and fan operation at reduced speed. For an optimal return on investment in a hot/humid climate and based on the Navy's installation and energy costs in this location, NREL recommended that ARC be considered only for RTUs meeting the following criteria: operating at least 50 hours per week, at least 7 tons in capacity, fewer than 10 years of age, and on buildings with multiple RTUs (Doebber I. D., 2014).

Meanwhile, Studer et al. (2012) performed a modeling analysis of implementing VFDs or stepped-speed fans in RTUs serving retail buildings, and presented aggregate energy savings as a fraction of whole-building (electric and gas) energy use, finding savings of 0.7 to 8.4% depending on climate zone and the presence or absence of refrigerated cases in the building (Studer, 2012). Studer et al. found a range of 29% to 55% fan energy savings with the stepped-speed fans and 44% to 57% savings with the fully variable speed fans in modeled buildings with refrigeration, and 31%--63% and 50%--72%, respectively, in modeled buildings without refrigeration (Studer, 2012). Refrigeration was modeled as being present in grocery areas of big-box retail stores. They found a small heating energy penalty after the implementation of VFDs or stepped-speed fans due to the reduction in fan heat.

Single-zone VAV systems can be implemented with a fully modulating variable-speed fan or with a two-speed fan. Implementation of a single-zone VAV system should ensure that outdoor airflow remains adequate, even at times of reduced heating and cooling load. This can be accomplished through direct measurement of outdoor airflow, proportional control of outdoor air dampers, or use of two-position outdoor air damper control, among other means (TRANE, 2013). Single-zone VAV systems can also offer improved temperature and humidity control relative to constant-speed systems at part-load conditions, due to reduced compressor cycling (TRANE, 2013).

DX cooling coils generally have airflow constraints that must be respected during operation and considered before pursuing a VAV retrofit. Too little airflow can result in a coil freezing. Airflow rates for DX coils are often expressed in terms of velocity. Some coil manufacturers offer performance data over a range of air velocities (and thus airflow) that allow for a turndown to about 42% of maximum airflow at part-load conditions (Carrier, 2007). Other distributors of DX coils report allowable turndown to only about 2/3 of maximum airflow (Capital Coil and Air, 2023). Some gas and electric heating coils require full airflow during all operation, and in that case, heating airflow must be fixed at that level. Hot water coils generally permit a wide range of modulation of airflow, since the water flow through the coil can be varied directly (TRANE, 2013).

Equipment sizing influences the extent of savings from a single-zone VAV retrofit, as do the system's operating hours (Doebber I. D., 2014). Retrofitting units that are currently oversized in terms of airflow will result in greater energy savings. Units that are constantly operating at near-full load, such as for process cooling, will not exhibit significant savings from a VAV retrofit. (Weissert & Smith, 2018). Some small commercial buildings may operate RTUs from zone thermostats with "fan-auto" control, which does not meet code requirements for providing consistent ventilation. The addition of appropriate ventilation controls through an RTU retrofit could result in a net increase in energy use as a result, despite the implementation of variable-speed fan operation (Weissert & Smith, 2018). Practical barriers also exist to VAV retrofits of single-zone packaged units. If the units are new enough to be under the manufacturer's warranty, a retrofit may void the warranty (Weissert & Smith, 2018). Energy Solutions concluded based on its analysis of the Catalyst that rooftop units have at least five years remaining in their useful life to be good candidates for a VAV retrofit, based on the expected payback time (Energy Solutions, 2019 ).

ASHRAE Standard 90.1 incorporated a requirement for single-zone VAV operation for the first time in 2010. That standard applied to air handling units/fan coil units with chilled water coils equipped with supply fan motors larger than 5 hp and for air handling units/fan coil units with DX coils and capacity greater than about 9 tons (Cai & Braun, 2018) (ASHRAE, 2010). The requirement could be satisfied through two-speed fans, VFDs, or electrically commutated motors (Trane, 2013). In 2022, ASHRAE 90.1 was updated, and an exception now applies for DX units with capacity of fewer than 5.5 tons (American Society of Heating, Refrigeration, and Air Conditioning Engineers, 2022), and chilled water and evaporative cooling units with fan motors of less than 0.25 hp. DX and chilled water cooling units that are subject to the supply fan airflow control requirements in ASHRAE 90.1 2022 and that control the cooling capacity directly based on space temperature (as in a typical constant-volume single-zone system) must have a minimum of two stages of fan control, and the lower stage must not exceed 66% of full speed. DX and chilled water cooling units subject to this requirement that control space temperature by varying airflow must have modulating fan speed, with a minimum fan speed that is no more than 50% of full speed. ASHRAE 90.1 2022 stipulates that low or minimum speed be used at times of low cooling load, or ventilation-only operation.

# 3. ComStock Baseline Approach

Existing constant-volume single-zone systems are potential candidates for a retrofit to single-zone VAV. All packaged single-zone systems modeled in ComStock are currently treated as constant volume (Dahlhausen, 2023). Packaged single-zone systems are modeled in ComStock with "load-based" control, in which a part-load ratio is calculated reflecting the required run time of the heating or cooling coil to meet the load at design airflow, sized separately for heating, cooling, and ventilation modes (US Department of Energy, 2022). Fans operate continuously during the building's occupied hours, with unoccupied operation assigned to one of three schemes (scheduled on during unoccupied hours, cycling to meet setpoint with ventilation during unoccupied hours, and cycling to meet setpoint without ventilation during unoccupied hours) based on a probability distribution (Parker, 2023).

Existing PSZ systems in ComStock have chilled water or DX cooling coils, and gas, hot water, or electric heating coils, or use air-source heat pumps for both heating and cooling (Parker, 2023). Hydronic coils can generally modulate flow rate for compatibility with lower airflows (TRANE, 2013). PSZ systems with heat pumps in ComStock are modeled with single-speed DX heating and cooling coils. As part of this measure, constraints will be enforced on minimum airflow for electric heating and DX coils.

# 4.  Modeling Approach

## 4.1. Applicability

This measure will be applied to all packaged single-zone systems modeled in ComStock, which is about 45% of the weighted floor area. Figure 1 shows the prevalence of HVAC system types in ComStock.

Equipment for conditioning spaces dominated by process loads, such as data centers, is not a good candidate for a VFD retrofit due to the consistent loads. The data center space type present in ComStock, as part of a large office building, is not served by a packaged single-zone system, so this consideration does not affect measure applicability in this case (Parker, 2023).

The DCV component of this measure will not be applied to space types for which DCV is not appropriate, corresponding to the following space types represented in ComStock:

-   Kitchens 
-   Dining 
-   Laboratories 
-   Healthcare patient spaces 
-   Corridors and stairwells 
-   Mechanical rooms 
-   High exhaust spaces, such as restrooms and locker rooms.

Healthcare patient spaces must provide a consistent level of ventilation at a prescribed level to ensure patient safety. Kitchens also require a consistent level of ventilation for safe operation, and kitchens often draw transfer air from adjacent dining spaces. Energy code guidance recommends that laboratory spaces in hospitals not have DCV applied.

This measure is also not intended to be applied to dedicated outdoor air units, as incorporating variable-speed fan control in such a unit would likely involve DCV, which is addressed as a separate option. Fan-coil units can, in some instances, be retrofitted for VAV operation, but are not considered in the scope of this measure due to the different conditions involved in assessing their suitability for retrofit.[^5]

{:refdef: style="text-align: center;"}
![](media\vav_rtu_image1.png)
{:refdef}

{:refdef: style="text-align: center;"}
Figure 1. Prevalence of HVAC system types in ComStock
{:refdef}

## 4.2. Technology Specifics

ARC retrofit kits do not typically involve replacement of the heating or cooling coils in the unit, and in this measure, it will be assumed that a single-zone VAV retrofit does not involve coil replacement (Weissert & Smith, 2018). ARC kits often contain controls and sensors for implementation of DCV and improved air-side economizing, and these features are included in this measure.

A common control sequence for VAV retrofits in RTUs with only one stage each of heating and cooling uses 90% fan speed in both heating and cooling modes, and 40% when the fan is operating for ventilation only. When multiple stages of heating or cooling are present, the fan speed generally increases to 75% for the first stage and then 90% for the second stage, and can also increase in economizing mode while cooling (Weissert & Smith, 2018); (TRANE, 2013); (Wang, 2013). Note that this is effectively a fan control approach with two discrete speeds, and not continuously variable. Note that in this case, to ensure adequate airflows based on EnergyPlus' sizing routines, the new maximum airflow remained the same as the existing maximum value.

Packaged single-zone systems are modeled by ComStock using the AirLoopHVAC:UnitarySystem object in EnergyPlus. To implement a single-zone VAV approach, the existing Fan:OnOff object is replaced with a Fan:VariableSpeed, and the system control type is set to "SingleZoneVAV." (Note that this measure is approximating the performance of a fan with a two-speed control approach with a continuously variable fan.) Under the single-zone VAV control approach, EnergyPlus considers two potential airflow rates---"heating and cooling" and "no load" (which can be applied in periods of low loads, or when only ventilation airflow is required), and a range of supply air temperatures. If the load cannot be met at the "no load" airflow rate and acceptable range of supply air temperatures, the airflow is increased to the heating and cooling supply airflow rate. The existing AirTerminal:SingleDuct:NoReheat is replaced with a AirTerminal:SingleDuct:VAV:HeatAndCool:NoReheat, which allows for increased airflow in heating. The AirTerminal:SingleDuct:VAV:HeatAndCool:NoReheat also allows for maximum and minimum airflow rates to be set, which will be configured based on the operating parameters of the Catalyst ARC retrofit kit, with a minimum of 40% of the design airflow (or the airflow required for ventilation, whichever is higher), and a maximum of the full design airflow. (Note that this was selected to ensure adequate airflow under EnergyPlus' sizing routines. In installed equipment, the degree of over-sizing may be greater in many applications, reflected in the controls of the Catalyst and other ARC units setting the new maximum airflow at 90% of the design maximum.) This airflow pattern is illustrated schematically in Figure 2. Airflow requirements for ventilation are calculated per ANSI/ASHRAE Standard 62.1 based on occupancy and floor area of the zone served, as well as any exhaust requirements. The measure confirms that the airflow limitations for the DX cooling and heating coils are respected.

{:refdef: style="text-align: center;"}
![](media\vav_rtu_image2.png)
{:refdef}

{:refdef: style="text-align: center;"}
Figure 2. Illustration of single-zone VAV airflow control scheme
{:refdef}

Image from Trane (2013)

In EnergyPlus, sizing of design airflow rates for the CoolingCoil: DX Single Speed, Coil:Heating:DX:SingleSpeed, and DX Multi-Speed objects are constrained based on a range of 300--450 cfm/ton (US Department of Energy, 2022). (A lower range of 125--250 cfm/ton applies to coils in dedicated outdoor air systems, but those coils are not the subject of consideration by this measure.) During simulation, EnergyPlus expects airflow of single-speed cooling coils to a range of 200--500 cfm/per ton of rated total cooling capacity and will issue a warning message if this expectation is not met. A constraint based on a flow rate range of 200--600 cfm/ton of rated total heating capacity is applied to DX heating coils (US Department of Energy, 2022). This approach relies on sizing of airflow for DX cooling coils to the upper range of airflow per unit of cooling capacity, to ensure that the 50% reduction can be achieved where applicable. This reduction could be achieved through an iterative approach of hard-sizing coil airflow rates based on the given capacity ratio.

EnergyPlus does not impose constraints on airflow of electric coils, though some coil manufacturers, especially manufacturers of electric coils used in reheat terminal units, recommend restrictions such as minimum airflow of 70 CFM/kW (Price, 2021). These constraints are considered in the measure, if it is possible that airflows could approach those levels.

### 4.2.3. Demand-Controlled Ventilation

To implement DCV, this measure iterates through air loops and enables DCV through the option on the Controller:MechanicalVentilation object in EnergyPlus, if the zone's space type is appropriate for DCV (see Applicability section). Note that in this control approach, occupancy schedules are used as a proxy for carbon dioxide concentration to control ventilation, since EnergyPlus does not separately calculate carbon dioxide levels.

### 4.2.4. Air-Side Economizing

Integrated air-side economizing is also implemented as part of this measure. The Catalyst configures air-side economizing with a differential dry-bulb control, and an ambient dew point temperature lockout of 60°F (Doebber I. D., 2014). For compliance with 2022 ASHRAE 90.1 across climate zones, the economizer is implemented in EnergyPlus with differential enthalpy control, and a high dry-bulb lock-out of 75°F. Since the Catalyst is configured to confirm the existence of a cooling call before initiating economizing, the "LockoutWithHeating" option in EnergyPlus is applied. The measure is configured to confirm the existence of a cooling call.

## 4.3. Greenhouse Gas Emissions

Three electricity grid scenarios are presented to compare the emissions of the ComStock baseline and the window replacement scenario. The choice of grid scenario will impact the grid emissions factors used in the simulation, which determines the corresponding emissions produced per kilowatt-hour. Two scenarios---Long-Run Marginal Emissions Rate (LRMER) High Renewable Energy (RE) Cost 15-Year and LRMER Low RE Cost 15-Year---use the Cambium data set, and the last uses the eGrid data set (NREL, 2022), (US EPA, 2022). All three scenarios vary the emissions factors geospatially to reflect the variation in grid resources used to produce electricity across the United States. The Cambium data sets also vary emissions factors seasonally and by time of day. This study does not imply a preference for any particular grid emissions scenario, but other analysis suggests that the choice of grid emissions scenario can impact results (Present, Gagnon, Wilson, et al. 2022). Emissions due to the on-site combustion of fossil fuels use the emissions factors shown in Table 1, which are from Table 7.1.2(1) of draft American National Standards Institute/Residential Energy Services Network/International Code Council 301 (Vijayakumar et al., 2022). To compare total emissions due to both on-site fossil fuel consumption and grid electricity generation, the emissions from a single electricity grid scenario should be combined with all three on-site fossil fuel emissions.

 Table 1. On-Site Fossil Fuel Emissions Factors 

{:refdef: style="text-align: center;"}
![](media/onsite_fossil_fuels_efs_table.png)
{:refdef}

## 4.4. Limitations and Concerns

ComStock's baseline treats all packaged single-zone (PSZ) systems as constant speed, although some existing systems are variable speed, which will lead to an overestimation of potential savings actually available at the stock level from a VAV conversion. Note that modeling a fan controlled to discrete speeds (as is the case with RTUs retrofitted with common ARC kits) with a continuously variable speed fan introduces some approximations of the energy use. The measure constrains the fan speed operation and airflow delivered to the upper and lower bounds allowed by the typical ARC kits.

ComStock's baseline does not currently model DCV in any office buildings, which may lead to an overestimation of the actual potential savings in the building stock from applying DCV in such buildings, as DCV is already somewhat commonplace in certain office space types, such as conference rooms. (Note that this occurs because of ComStock's approach of averaging occupancy density over multiple space types. As a result, the combined office space types do not meet the occupancy density thresholds in ASHRAE 90.1 for required implementation of DCV, while in reality, spaces such as conference rooms would be subject to the requirement.) The results of the 2018 Commercial Building Energy Consumption Survey performed by the Energy Information Administration concluded that demand-controlled ventilation was present in around 11% of commercial building floor space in the US (US EIA, 2022).

# 5. Output Variables

Table 2 includes a list of output variables that are calculated in ComStock. These variables are important in terms of understanding the differences between buildings with and without the single-zone VAV measure applied. These output variables can also be used for understanding the economics of the upgrade (e.g., return on investment) if cost information (i.e., material, labor, and maintenance costs for technology implementation) is available.

Table 2. Output Variables Calculated From the Measure Application

{:refdef: style="text-align: center;"}
![](media\vav_rtu_table2.png)
{:refdef}

# 6. Results

In this section, results are presented both at the stock level and for individual buildings through savings distributions. Stock-level results include the combined impact of all the analyzed buildings in ComStock, including buildings that are not applicable to this measure. Therefore they do not necessarily represent the energy savings of a particular or average building. Stock-level results should not be interpreted as the savings that a building might realize by implementing the measure.

Total site energy savings are also presented in this section. Total site energy savings can be a useful metric, especially for quality assurance/quality control, but this metric on its own can have limitations for drawing conclusions. Further context should be considered, as site energy savings alone do not necessarily translate proportionally to savings for a particular fuel type (e.g., gas or electricity), source energy savings, cost savings, or greenhouse gas savings. This is especially important when a measure impacts multiple fuel types or causes decreased consumption of one fuel type and increased consumption of another. Many factors should be considered when analyzing the impact of an energy efficiency or electrification strategy, depending on the use case.

## 6.1. Single Building Measure Tests

To demonstrate the functionality of the measure, the Advanced RTU Controls measure was applied to a retail model with single-zone, constant-volume RTU systems in Climate Zone 4A ("retail PSZ AC model"), with the Leesburg, Virginia, weather file. This section reviews the operation of the VAV fan control and air-side economizing features.

Illustrative results are presented for a zone with high fan power relative to the rest of the building, "Retail, Retail B---Story Ground." Figure 3 shows a comparison of the supply airflow rates for that zone for a roughly one-week period in July, for the "base" case and with the measure applied ("measure"). The zone level cooling load for the same period in the "measure" case is shown in the bottom plot in Figure 3. The upper figure shows how, in the measure case, fan speed modulates to meet the cooling load, while reducing to the "low-load" level during periods when loads are low. The implementation of this measure is not expected to change zone-level loads.

{:refdef: style="text-align: center;"}
![](media\vav_rtu_image3.png)
{:refdef}

{:refdef: style="text-align: center;"}
![](media\vav_rtu_image4.png)
{:refdef}

{:refdef: style="text-align: center;"}
Figure 3. Supply airflow rate for the base case and with the measure applied for the selected zone (top), and zone load for the measure case (bottom)
{:refdef}

Note that a negative load corresponds to cooling.

Figure 4 shows the fan power of the corresponding packaged unit for the same period, in the base case and with the measure applied. In this case, and for many zones in the building, the fan operates for much of the year at the 40% of maximum airflow rate that is set as the minimum allowable airflow, consistent with typical ARC configurations. (Note that the minimum was imposed as the larger of 40% of maximum airflow, or the minimum ventilation requirement, so this minimum airflow may exceed 40% of maximum in some zones.) A theoretical purely cubic fan law would result in fan power of 8% of the base case, under these conditions, with the airflow around 40% of maximum. Fan curves in practice and as represented in EnergyPlus are not purely cubic, and in modeling this system, a fan power minimum of 30% was imposed, reflecting the typical minimum turndown for VFDs attached to non-inverter duty motors, representative of retrofits of older existing systems. Thus, when the fan is operating at low-load conditions, as it does for many hours of the year, it is operating at 30% of the maximum power. (VFD losses were not accounted for as part of this analysis. Some single zone VAV retrofits can be accomplished through the use of electronically commutated motors, which in certain circumstances, can operate more efficiently than a motor and VFD combination.) In both the base and measure cases, the occupied hours in which a zone setpoint was not met were similar, and low, indicating that the systems continue to meet loads effectively with this measure applied. Note that the constant-volume fan in the baseline (represented with a Fan:OnOff object), as modeled in EnergyPlus, can drop to a lower minimum power level, resulting in the lower minimum fan power for the baseline shown in the figure.

{:refdef: style="text-align: center;"}
![](media\vav_rtu_image5.png)
{:refdef}

{:refdef: style="text-align: center;"}
Figure 4. Fan power for the base case and with the measure applied for the selected zone
{:refdef}

Figure 5 (a and b) confirms the economizer performance over a several-day period in October. The economizer is controlled based on differential enthalpy. As shown in Figure 5a, outdoor air enthalpy is less than return air enthalpy throughout the entire period. The system is generally operating at 100% outdoor air throughout this period, as shown in Figure 5b (top). Due to the limited cooling load (as shown in Figure 5b (bottom), the system is generally operating at its "low load" flow rate throughout this period. In this zone, and in several other zones in the building, 40% of the maximum airflow is higher than the ventilation requirement, and thus the effects of DCV are not evident, as they would result in airflow below the minimum set based on the characteristics of common ARC controls. This is illustrated in Figure 6, showing that the airflow never drops below 0.6 kg/s (40% of the maximum airflow) during occupied periods, even with varying levels of occupants during the occupied periods. Figure 7 shows the zone occupant count over that time period, for reference. (Note that a lower minimum airflow rate would allow for greater opportunities for DCV implementation in spaces such as this in which the ventilation airflow is less than 40% of the maximum.)

{:refdef: style="text-align: center;"}
![](media\vav_rtu_image6.png)
{:refdef}

{:refdef: style="text-align: center;"}
Figure 5a. Outdoor air and return air enthalpy for the selected zone
{:refdef}

{:refdef: style="text-align: center;"}
![](media\vav_rtu_image7.png)
{:refdef}

{:refdef: style="text-align: center;"}
![](media\vav_rtu_image8.png)
{:refdef}

{:refdef: style="text-align: center;"}
Figure 5b. System total airflow and outdoor airflow (top), and zone cooling load (bottom)
{:refdef}

{:refdef: style="text-align: center;"}
![](media\vav_rtu_image9.png)
{:refdef}

{:refdef: style="text-align: center;"}
Figure 6. Outdoor airflow and overall fan airflow for selected zone, with measure applied
{:refdef}

{:refdef: style="text-align: center;"}
![](media\vav_rtu_image10.png)
{:refdef}

{:refdef: style="text-align: center;"}
Figure 7. Zone occupant count for selected zone, with measure applied
{:refdef}

## 6.2. Stock Energy Impacts

The Advanced RTU Controls measure demonstrates 4.2% total site energy savings (182 trillion TBtu) for the U.S. commercial building stock modeled in ComStock (Figure 8). The savings are primarily attributed to:

-   **2.1%** stock **heating gas** savings (18.1 TBtu)

-   **3.6%** stock **cooling** savings (24.0 TBtu)

-   **26%** stock **fan** savings (137.1 TBtu).

{:refdef: style="text-align: center;"}
![](media\vav_rtu_image11.jpeg)
{:refdef}

{:refdef: style="text-align: center;"}
Figure 8. Comparison of annual site energy consumption between the ComStock baseline and the advanced RTU controls measure scenario. Energy consumption is categorized both by fuel type and end use.
{:refdef}

{:refdef: style="text-align: center;"}
![](media\vav_rtu_image12.jpeg)
{:refdef}

{:refdef: style="text-align: center;"}
Figure 9. Comparison of annual site energy consumption between the ComStock baseline and the Advanced RTU Controls measure scenario for applicable buildings only. Energy consumption is categorized both by fuel type and end use.
{:refdef}

Figure 9 shows a comparison of annual site energy consumption between the ComStock baseline and the Advanced RTU Controls measure scenario for only those buildings to which the measure was applicable, which provides a more direct basis for comparison to some past studies. The end use with the highest relative reduction is fan energy (53.3% savings among applicable buildings), which is generally consistent with the findings of other studies, including the evaluation by Energy Solutions in 2019, in terms of fan energy being a driver of energy savings, and the relative quantity of fan savings (Energy Solutions, 2019 ). (Note that the implementation of this measure also assumed that after the retrofit, systems operate with fan motors that meet the ASHRAE 90.1-2019 efficiency standard, reflecting the likelihood of motor replacement being necessary for older, non-inverter-driven motors for VFD compatibility.) The primary mechanism driving fan energy savings is the use of variable-speed fans. Energy Solutions found a range of 5%--50% RTU electricity savings (with RTU electricity including fan and cooling energy use), primarily attributable to fan energy, and a range of 1%--10% RTU gas savings (consisting of heating only). The evaluation by Wang et al. (2013) of the Catalyst found an average of 55% RTU energy savings (with RTU energy including heating, cooling, and fan energy), primarily driven by fan energy, and did not consider gas heating savings. The modeling analysis of Studer et al. (2013) found a range of 55%--72% fan energy savings in buildings without case refrigeration with retrofits to fully variable-speed fans. The fan energy savings identified here are consistent with fan operation at 30% power (the power minimum assumed in modeling, which governs over the power at the airflow minimum) for extended periods of time, as observed in time series results for individual buildings. This low fan power mode corresponds to the "No Load/Low Load" modeled for the single-zone RTUs.

The relatively small gas heating savings identified here (6.1% among applicable buildings) falls in the range observed by Energy Solutions (2019). Heating savings in this measure results from reduced load for conditioning outdoor air as a result of DCV, compensating for the heating penalty from the reduction in fan heat with variable-speed fans. (Note that this penalty is likely the cause for the small aggregate electric heating penalty.) Note that ComStock's sample includes a much wider range of climate zones than the buildings evaluated by Energy Solutions in New York State, and that in ComStock many of the buildings in which the measure was implemented had a least some variation in loads, creating opportunity for savings from variable-speed fans, while Energy Solutions did not report screening building candidates based on that criteria. Modeling does not fully encapsulate the faults that may be present in real buildings and undermine the efficacy of advanced RTU controls and other retrofits, which can result in higher estimated savings than those that would be observed in practice, even for a similar building type. Energy savings from variable-speed fans is also highly dependent on the approach for sizing the fans relative to current loads, and existing buildings may exhibit differing degrees of under- or over-sizing of equipment relative to current loads. ComStock uses EnergyPlus sizing routines, leveraging location-specific design days. Per ASHRAE Standard 90.1 Appendix G, systems are over-sized by 15% in cooling and 25% in heating (Parker, 2023).

Cooling energy savings (11.2% among applicable buildings) from this measure result from the use of air-side economizing, as well as the reduction in cooling load from outdoor air from the use of DCV, and reduction in fan heat from the use of variable-speed fans. In their analysis, Energy Solutions attributed 90% of the observed energy savings to fan energy, 10% to economizing, and less than 1% to DCV. Among applicable buildings in ComStock, 77% of the observed energy site savings was attributable to fan energy, 14% to cooling, and 9% to heating.

## 6.3. Stock Greenhouse Gas Emissions Impact

Figure 10 shows a comparison of aggregate greenhouse gas emissions under several different scenarios reflecting different carbon intensity of electricity. Under the three scenarios shown here, implementation of this measure results in a reduction of carbon emissions of 4% relative to the base case. The vast majority of the site energy savings from the Advanced RTU Controls measure (91% of the savings among applicable buildings) comes from electricity, and thus the associated reductions in carbon emissions will be sensitive to carbon intensity of the electricity supply, in a particular location or at a particular future time. Note that these scenarios are presented for illustrative purposes. In general, a reduction in carbon emissions lower than the reduction in overall site energy use (11.3%) is to be expected for this measure, since the majority of the energy savings result from electricity, which has a lower carbon intensity than natural gas in the scenarios presented here.

{:refdef: style="text-align: center;"}
![](media\vav_rtu_image13.jpeg)
{:refdef}

{:refdef: style="text-align: center;"}
Figure 10. Greenhouse gas emissions comparison of the ComStock baseline and the Advanced RTU Controls scenario
{:refdef}

Three electricity grid scenarios are presented: Cambium Long-Run Marginal Emissions Rate (LRMER) High Renewable Energy (RE) Cost 15-Year, Cambium LRMER Low RE Cost 15-Year, and eGrid.

## 6.4. Site Energy Savings Distributions

This section discusses site energy consumption for quality assurance/quality control purposes. Note that site energy savings can be useful for these purposes, but other factors should be considered when drawing conclusions, as they do not necessarily translate proportionally to source energy savings, greenhouse gas emissions avoided, or energy cost. 

Distributions of energy savings, by end use and fuel type, across the ComStock sample from the Advanced RTU Controls measure, were also considered. Figure 11 shows these results for the measure applied with both economizing and demand-controlled ventilation.

Fan energy use is the end use with the greatest energy savings in the 25th to 75th percentiles, as expected. For fan energy use, this range extends from roughly 30% to 67% of baseline fan energy use. In many models, fans operated for a significant portion of the year at the "low load" airflow rate, at 30% fan power (the minimum limit for fan power to reflect installation of VFDs on existing fans). Operation at 30% fan power throughout the year would result in 70% fan energy savings, so this value is generally expected as the upper range of the bulk of the distribution. The number of hours throughout the year that fans operate at low-load or cooling/heating flow rates is dependent on the loads in zones throughout the building, so variation in the level of fan energy savings is expected. There were no datapoints with fan energy savings exceeding 67%, confirming the theoretical upper limit of savings expected.

The 25th-75th percentile range of cooling energy savings roughly corresponds to 5% to 15% of cooling energy use. Through application of this measure, cooling energy savings results from economizing (where economizing was not present in the baseline), the reduction in cooling load from outdoor air through the use of DCV, and the reduction in fan heat during cooling periods. The extent of savings from economizing is dependent on building loads (influencing the distribution of cooling demand throughout the year) and local weather conditions (influencing the range of time periods during which economizing is feasible). The reduction in cooling load from the use of DCV is dependent on the reduction in ventilation load from DCV (reflecting zone occupancy, and the proportion of ventilation attributable to the occupancy component as opposed to the floor area component), and local weather conditions (reflecting the cooling load imposed from the additional ventilation in the baseline). The combined fan and cooling energy savings corresponding to the 25th--75th percentile range of the distribution is roughly 20% to 67%, which compares reasonably well with the 5%--50% range of RTU energy observed in the Energy Solutions study (Energy Solutions, 2019 ). Note that the Energy Solutions study addressed buildings in New York State only, not encompassing all the climate zones considered in this analysis, and that modeling analysis is likely in general to somewhat overestimate savings observed in a field study due to the inherent limitations in capturing all sources of fault or inefficacy of actual implementations.

The distributions of heating energy savings (across fuel sources: electricity, natural gas, and other) generally have a median value of around zero. This reflects the fact that elements of this measure can contribute to heating energy savings (ventilation load reduction through DCV) or a heating energy penalty (reduction in useful fan heat through the use of VAV fans). The extent to which this results in a net savings or penalty depends on the distribution of fan operating hours at "low load" speeds in heating mode in the measure case, and the reduction in ventilation load due to DCV.

Several of the end uses show outliers with very high energy savings or very high energy penalties. The relatively small number of buildings with high proportionate cooling energy penalties tend to be warehouses modeled under the DEER approach in California (in mild coastal climate zones), with non-integrated economizers in the baseline. The economizer component of this measure maintains the non-integrated approach, since it does not alter controls for existing economizers. In the measure case operating at low-flow conditions, the lower flow of outdoor air available for cooling causes the cooling coil to begin operating when it would not have in the baseline with higher airflow, locking out the economizer, and increasing the amount of mechanical cooling performed relative to economizing. This results in a cooling energy penalty. In these buildings, there was generally still a result of overall site energy savings from implementation of this measure. A recent adjustment to system assignment for warehouse buildings may eliminate these cases.

In the case of pump energy use, with one exception, the outliers with high energy penalties reflect buildings with very low pump energy use in the base case, in which the model is very sensitive to small variations that therefore results in large percentage changes. Note that in buildings with a heating penalty (due to the reduction in fan heat), and using hydronic heating systems, we could expect a small increase in pump energy resulting from pumps serving the hydronic heating loops.

In the case of electric and gas heating, buildings exhibiting large energy penalties, with one exception for each, had low electric and gas heating energy use, respectively, in the baseline case, so a small fluctuation became a large penalty proportionately. Note that electric and gas heating penalties can be expected, due to the reduction in fan heat. A calculation of fan energy savings and expected reduction in fan heat relative to the heating energy penalty in a sampling of buildings confirms that the order of magnitude of the heating penalty is generally as expected, and less than the magnitude of fan energy savings. (The theoretical upper bound on any heating penalty is the fan energy use divided by the efficiency of the heating source, which for natural gas coils is generally modeled as 0.8 in ComStock.) In general, after the single-zone VAV retrofit is implemented, many zones operate during the majority of their hours in "low load" airflows, in which the heating penalty could be expected to result, if a heating load is present. Note that in buildings with high outdoor air ventilation loads, the heating energy savings from DCV can eclipse this penalty. The outliers with very high heating electricity savings also had very low heating energy use in the baseline, leading to a high sensitivity to small fluctuations in heating energy use.

No increase in fan energy use is expected as a result of application of this measure. The outlier data points exhibiting a fan energy penalty had very low fan energy in the base case, and thus a small variation resulted in a large percentage change. The heat recovery penalty observed in a small subset of the sample may be due to increased operation (and reduced lockout) of the enthalpy wheel due to a reduction in fan heat.

{:refdef: style="text-align: center;"}
![](media\vav_rtu_image14.jpeg)
{:refdef}

{:refdef: style="text-align: center;"}
Figure 11. Percent site energy savings distribution for ComStock models with the Advanced RTU Controls measure applied by end use and fuel type
{:refdef}

The data points that appear above some of the distributions indicate outliers in the distribution, meaning they fall outside 1.5 times the interquartile range. The value for n indicates the number of ComStock models that were applicable for energy savings for the fuel type category.

It is also illustrative to consider the site energy savings distributions by HVAC system type, as shown in Figure 12. In Figure 12, the two system types with the largest representations in the sample, PSZ with gas coil and PSZ with electric coil, had similar ranges of site energy savings, with 50% of the samples having roughly 5%--15% site energy savings. Note that in systems with effectively higher heating efficiencies on a site energy basis (heat pumps, electric resistance, or district heating), the effects of a heating penalty from a reduction in fan heat will be more muted for site energy savings than in systems with gas-fired boilers or gas heating coils, which may partially explain the higher upper range of savings observed in the former group of system types. (Note that the PSZ-AC with district chilled water and an electric heating coil had only 12 incidences in the sample.)

{:refdef: style="text-align: center;"}
![](media\vav_rtu_image15.jpg)
{:refdef}

{:refdef: style="text-align: center;"}
Figure 12. Percent site energy savings distribution for ComStock models with the applied Advanced RTU Control measure by HVAC system type
{:refdef}

The data points that appear above some of the distributions indicate outliers in the distribution, meaning they fall outside 1.5 times the interquartile range. The value for n indicates the number of ComStock models that were applicable for energy savings for the HVAC system type category.

It is also informative to view savings distributions disaggregated by building type. The variable volume fan component in the measure has the highest energy savings potential in zones with variable loads and high peak loads (which is often building-type driven). Figure 13 shows the distributions of relative site energy savings by building type. Both primary and secondary schools, especially in gym and cafeteria systems, can have high peak loads and highly variable loads, which can explain the comparatively high savings fraction for these buildings at the 75% percentile, around 18%.

{:refdef: style="text-align: center;"}
![](media\vav_rtu_image16.jpeg)
{:refdef}

{:refdef: style="text-align: center;"}
Figure 13. Percent site energy savings distribution for ComStock models with the applied Advanced RTU Control measure by building type
{:refdef}

The data points that appear above some of the distributions indicate outliers in the distribution, meaning they fall outside 1.5 times the interquartile range. The value for n indicates the number of ComStock models that were applicable for the building type.

Figure 14 shows the distribution of relative site energy savings by climate zones. The individual elements of the measure---variable speed fans, economizing, and DCV---each have potential distinct influences from climate zone and building loads. The economizing measure has higher savings potential in areas in which there are frequently cool outdoor temperatures while a demand for cooling exists inside (cooling loads are common in many commercial buildings throughout much of the year), and DCV has the greatest savings potential in buildings with high peak ventilation requirements and variable occupancy (while being eligible for DCV), and many hours with "extreme" (hot, cold, and/or humid) outdoor conditions.

However, a comparison at the 10,000-sample size indicated that savings distributions by climate zone looked very similar for the measure options with VAV fans only, VAV fans and economizing, and VAV fans, economizing, and DCV. (These distributions are not shown here for concision.) Note that Climate Zone 3C is located only in California, in which the DEER-based modeling methods are applied, making it not directly comparable to other climate zones. A 25th percentile around 2.5% site energy savings and a 75th percentile around 12.5% site energy savings was common across many climate zones, and similar ranges also existed across building types, suggesting that on the whole, the variability in savings from this measure may be driven largely by building specific factors related to the ratio of average and peak thermal loads and load variability, and not climate zone or building type.

{:refdef: style="text-align: center;"}
![](media\vav_rtu_image17.jpeg)
{:refdef}

{:refdef: style="text-align: center;"}
Figure 14. Percent site energy savings distribution for ComStock models with the applied Advanced RTU Control measure (with DCV and economizing) by climate zone
{:refdef}

The data points that appear above some of the distributions indicate outliers in the distribution, meaning they fall outside 1.5 times the interquartile range. The value for n indicates the number of ComStock models that were applicable for the climate zone.

## 6.5. Heating Energy Use Implications

In about 11.5% of the buildings to which this measure was applicable, a natural gas heating penalty was observed, indicating that a heating penalty from reduced fan power exceeded the magnitude of any heating energy savings from implementation of DCV, and reduced zone over-heating from modulation of fan speed. Figure 15 illustrates with a scatterplot the magnitude of the natural gas heating penalty as a function of fan energy savings for a subset of these buildings (the small number of buildings in this set with fan energy savings greater than 300,000 kWh were omitted here for clarity). (Natural gas savings is presented in kWh to allow a direct comparison between the quantities.) The red line in Figure 15 shows the maximum expected natural gas heating penalty as a function of fan energy use, given an assumption of all fan heat being transferred to the airstream (reflecting conditions in the ComStock models considered), and a natural gas heating efficiency of 80% (typical of natural gas heating coils in air handling units). In Figure 15, the natural gas penalty values generally lie above, and in many cases, significantly above, the red line, indicating that the penalty observed is less than the maximum value expected. This provides a "sanity check" on the results. In general, the degree of the heating penalty is a function of the distribution of zone heating hours over fan operating conditions at low speed (when the reduction in fan power will be applicable).

{:refdef: style="text-align: center;"}
![](media\vav_rtu_image18.png)
{:refdef}

{:refdef: style="text-align: center;"}
Figure 15. Natural gas energy savings (negative, a penalty) as a function of fan energy savings for a selection of datapoints in which a natural gas heating energy penalty was observed. Points are colored by ASHRAE climate zone.
{:refdef}

# 7. References

AirRevive. (N.D.). *AirRevive*. Retrieved October 29, 2023, from FAN COIL UNIT & AIR HANDLER/ROOF TOP UNIT REFURBISHMENT PROJECTS: http://www.airrevive.com/projects-case-studies/

ASHRAE. (2010). *Standard 90.1: Energy Standard for Buildings Except Low-Rise Residential Buildings.* Atlanta, Georgia: American Society of Heating, Refrigeration, and Air-Conditioning Engineers.

ASHRAE. (2022). *Energy Standard for Sites and Buildings Except Low-Rise Residential Buildings (I-P Edition).* Atlanta, Georgia: ASHRAE.

Cai, J., & Braun, J. (2018). Assessments of variable-speed equipment for packaged rooftop units (RTUs) in the United States. *Energy and Buildings, 164*, 203-218.

Capital Coil and Air. (2023). *GUIDELINES FOR AIR VELOCITIES*. Retrieved from Capital Coil and Air: https://www.capitalcoil.com/guidelines-air-velocities/Carrier. (2007). *Product Data: NuFin Coils.* Carrier Corporation.Dahlhausen, M. P. (2023). OpenStudio Standards. National Renewable Energy Laboratory . Retrieved from https://github.com/NREL/openstudio-standards

Doebber, I. D. (2014). *RM12-2703 Advanced Rooftop Unit Control Retrofit Kit Field: Hawaii and Guam Energy Improvement Technology Demonstration Project.* 2014: National Renewable Energy Laboratory.

Doebber, I. D. (2016). *Turnkey Heating, Ventilating, and Lighting Retrofit Solution Combining Energy Efficiency and Demand Response Benefits.* 2016: National Renewable Energy Laboratory.

Energy Solutions. (2019). *Scaled Deployment of Advanced Rooftop Unit Controls in New York State.* Oakland, CA: Energy Solutions .Fernandez, N. K. (2017). *Impacts of Commercial Building Controls on Energy Savings and Peak Load Reduction .* Pacific Northwest National Lab.

International Environmental (IEC). (2023, October 29). *EnviroKit: Expanding the Life of Your Fan Coil Units*. Retrieved from International Environmental (IEC): https://iec-okc.com/envirokit.html

Lawrence Berkeley National Laboratory. (N.D.). Retrieved from Demand-Controlled Ventilation (DCV): https://svach.lbl.gov/demand-controlled-ventilation/

Northwest Power and Conservation Council . (2022). *Regional Technical Forum*. Retrieved from Advanced Rooftop Controls: https://rtf.nwcouncil.org/measure/advanced-rooftop-controls/\#:\~:text=An%20Advanced%20Rooftop%20Control%20(ARC,as%20%E2%80%9CARC%2Dlight%E2%80%9D.

NREL. (2022, September 2). *"Cambium \| Energy Analysis \| NREL."*. Retrieved from https://www.nlr.gov/analysis/cambium.html.

Pacific Northwest National Lab. (2021). *Best Practices for Air-Side Economizers Operation and Maintenance*. Retrieved from O&M Best Practices: https://www.pnnl.gov/projects/om-best-practices/air-side-economizers

Parker, A. H. (2023). *ComStock Reference Documentation Version 1.* Golden, CO : National Renewable Energy Laboratory.Present, E. G. (2022). *Choosing the Best Carbon Factor for the Job: Exploring Available Carbon Emissions Factors and the Impact of Factor Selection.* NREL.

Price. (2021). *Price*. Retrieved from Electric Coils for VAV Terminals: https://www.priceindustries.com/content/uploads/assets/literature/manuals/section%20f/electric-coils-for-vav-terminals-manual.pdf

ProStar Energy Solutions. (N.D.). *HVAC-R Solutions*. Retrieved November 5, 2023, from ProStar Energy Solutions: https://prostarenergy.com/solutions/hvacr-solutions/\#catalyst

Shoukas, G. B. (2020). *Analysis of Fault Data Collected from Automated Fault Detection and Diagnostic Units for Packaged Rooftop Units.* Golden, CO. : National Renewable Energy Laboratory.

Studer, D. R. (2012). *Energy Implications of Retrofitting Retail Sector Rooftop Units with Stepped-Speed and Variable-Speed Functionality.* 2012: National Renewable Energy Laboratory.

TRANE. (2013). *\"Understanding Single Zone VAV Systems\".* Dublin, Ireland: Trane. Retrieved from https://www.trane.com/content/dam/Trane/Commercial/global/products-systems/education-training/engineers-newsletters/airside-design/admapn047en_0413.pdf

Unilux Suite Solutions. (2021). *CASE STUDY: ECM vs. PSC Motors*. Retrieved October 29, 2023, from Unilux Suite Solutions: https://uniluxsuitesolutions.com/ecm-vs-psc-motors/

US Department of Energy. (2019). EnergyPlus. Retrieved from https://energyplus.net/US Department of Energy. (2022). *EnergyPlus Engineering Reference.* Washington, DC. : US Department of Energy.

US EIA. (2012). *Commercial Building Energy Consumption Survey.* Washington DC: US Energy Information Administration.

US EIA. (2022). *2018 Commercial Buildings Energy Consumption Survey: Building Characteristic Highlights .* Washington, DC. : US Energy Information Administration. Retrieved from https://www.eia.gov/consumption/commercial/data/2018/pdf/CBECS_2018_Building_Characteristics_Flipbook.pdf

US EPA. (2022). *Emissions & Generation Resource Integrated Database (eGRID) \| US EPA*. Retrieved 9 2, 2022, from https://www.epa.gov/egrid.

Vijayakumar, G. e. (2022). *ANSI/RESNET/ICC 301-2022 - Standard for the Calculation and Labeling of the Energy Performance of Dwelling and Sleeping Units using an Energy Rating Index.* Oceanside, CA.

Wang, W. K. (2013). *Advanced Rooftop Control (ARC) Retrofit: Field Test Results .* Pacific Northwest National Laboratory.Weissert, J., & Smith, M. (2018). Advanced RTU Controls -- Opening up a New Retrofit Market. *ACEEE.*

# A.  Additional Figures

{:refdef: style="text-align: center;"}
![Chart, bar chart Description automatically generated](media\vav_rtu_image19.jpeg)
{:refdef}

{:refdef: style="text-align: center;"}
Figure A-1. Site annual natural gas consumption of the ComStock baseline and the measure scenario by census division
{:refdef}

{:refdef: style="text-align: center;"}
![Chart, bar chart Description automatically generated](media\vav_rtu_image20.jpeg)
{:refdef}

{:refdef: style="text-align: center;"}
Figure A-2. Site annual natural gas consumption of the ComStock baseline and the measure scenario by building type
{:refdef}

{:refdef: style="text-align: center;"}
![Chart, bar chart Description automatically generated](media\vav_rtu_image21.jpeg)
{:refdef}

{:refdef: style="text-align: center;"}
Figure A-3. Site annual electricity consumption of the ComStock baseline and the measure scenario by building type
{:refdef}

{:refdef: style="text-align: center;"}
![Chart, bar chart Description automatically generated](media\vav_rtu_image22.jpeg)
{:refdef}

{:refdef: style="text-align: center;"}
Figure A-4. Site annual electricity consumption of the ComStock baseline and the measure scenario by census division
{:refdef}

[^1]: Due to the fan and pump affinity laws, VFDs also result in significantly greater energy savings than other means of controlling fan (such as inlet vanes) or pump (throttling valves) flow.

[^2]: Some energy efficiency programs distinguish between "full" and "light" ARC, with "full" ARC incorporating DCV and improved economizing as well as supply fan VFDs, and "light" meaning supply fan VFDs only (Weissert & Smith, 2018) (Northwest Power and Conservation Council , 2022).

[^3]: Note that any reference to manufacturer or model information in this paper are included as examples only. NREL does not endorse or support any particular brand, type, or model of equipment.

[^4]: The Catalyst is now sold by ProStar Energy Solutions (ProStar Energy Solutions, N.D.).

[^5]: Several companies make retrofit kits to install electronically commutated motors in fan coil units, which can facilitate variable-speed fan operation. These include AirRevive, Unilux Solutions, and IEC's EnviroKit (International Environmental (IEC), 2023), (Unilux Suite Solutions, 2021), (AirRevive, N.D.)).
