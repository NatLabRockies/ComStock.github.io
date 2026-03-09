---
layout: default
title: Cost Analysis
parent: Resources
nav_order: 5
---

# Using ComStock to Analyze Cost

In addition to providing information on energy use in the U.S. commercial building stock, ComStock™ can also support assessment of energy-related costs if combined with external data sets. This document discusses how ComStock data can be combined with external cost data. We discuss two different classes of costs:

1)	**Utility cost** – This is the cost of purchasing heat, fuel, and/or electricity for a building under specific operation.

2)	**Upgrade cost** – This is the cost of installing an upgrade, including both the purchase of equipment and materials and the labor costs for installing the upgrade.

## About Utility Cost Assessment
As of dataset 2024 Release 1, ComStock includes utility cost data. The primary resource for electric utility rates is the [Utility Rate Database](https://apps.openei.org/USURDB/) (URDB). This database contains machine-readable descriptions of electric rate structures which have been compiled by manually processing utility rate documentation published by utilities. URDB includes both current and historic rates. ComStock only applies current rates to the models. Electricity rates are assigned to models based on their serving utility (EIA utility ID), annual peak demand range, and energy consumption. Annual utility bills are reported for the minimum, maximum, mean, and median of all applicable rates for each model. See the [ComStock reference documentation]({{site.baseurl}}{% link docs/resources/resources.md %}#references) for more detail. Natural gas prices were assumed to be volumetric due to a lack of detailed public databases. [2022 U.S. EIA Commercial Natural Gas Prices and Heat Content](https://www.eia.gov/energyexplained/natural-gas/use-of-natural-gas.php) data were used to calculate an energy price in dollars per kBtu. Similarly, there was no detailed database of rates for propane or fuel oil, and we assumed volumetric pricing. These fuels are typically delivered in batches, with annual deliveries varying. Minimum delivery charges are assumed to be included in the volumetric price. [2022 U.S. EIA residential No. 2 Distillate and Weekly Heating Oil & Propane Prices (Oct–Mar)](https://www.eia.gov/dnav/pet/pet_pri_dist_a_epd2_prt_dpgal_a.htm), along with EIA heat content data, were used where state-level prices were unavailable.

This approach provides a snapshot of utility costs under present day utility rates, with variation by location and utility. For more details about the data sources, methods and assumptions used to estimate utility costs in the public datasets, please see the [ComStock reference documentation]({{site.baseurl}}{% link docs/resources/resources.md %}#references).

For users desiring a custom cost assessment, ComStock provides both annual and timeseries energy consumption by fuel, location, and building type. When matched with a utility rate, this energy consumption can be converted to energy cost. As a simple example, if electricity costs are $0.14 per kWh, the total annual cost of electricity in a building or group of buildings could be found by multiplying the annual sum of the total electricity in a building (variable *out.electricity.total.energy_consumption*) by $0.14 plus any fixed charges from the utility. For an assessment looking at the costs before and after an upgrade, this base cost could be compared to the energy costs after the upgrade with the same variable; the change in cost can be calculated by using the ComStock output variable on energy savings (*out.electricity.total.energy_savings*). Furthermore, because energy consumption and energy savings are both output by ComStock as time series, more complicated tariff structures could also be applied, such as demand charges or time-of-use rates. A full list of the basic energy output variables is given below.

|                        | **Total Energy**                                | **Energy Savings (by upgrade)**             |
| ---------------------- | ----------------------------------------------- | ------------------------------------------- |
| Electricity (kWh)      | _out.electricity.total.energy_consumption_      | _out.electricity.total.energy_savings_      |
| Natural Gas (kWh)      | _out.natural_gas.total.energy_consumption_      | _out.natural_gas.total.energy_savings_      |
| Other Fuels (kWh)      | _out.other_fuel.total.energy_consumption_       | _out.other_fuel.total.energy_savings_       |
| District Heating (kWh) | _out.district_heating.total.energy_consumption_ | _out.district_heating.total.energy_savings_ |
| District Cooling (kWh) | _out.district_cooling.total.energy_consumption_ | _out.district_cooling.total.energy_savings_ |

<u>Note</u>: These energy outputs are all given in kWh for ease of adding up total energy use, but kWh is unlikely to be the default unit for fuel costs outside electricity.

In addition to these variables, which give total energy consumption and savings by fuel, ComStock also provides end-use level consumption and savings if more detailed cost assessment is desired (e.g., HVAC-related energy costs). For a full list of ComStock output variables, reference the “data_dictionary.tsv” file for the relevant release on the [Open Energy Data Initiative](https://data.openei.org/s3_viewer?bucket=oedi-data-lake&prefix=nrel-pds-building-stock%2Fend-use-load-profiles-for-us-building-stock%2F).

## About Upgrade Cost Assessment
The cost of installing energy-related upgrades is important for a wide range of stakeholders in selecting and designing building retrofits, but ComStock does not currently provide this information. Understanding how much an upgrade will cost is dependent on several factors including local labor costs, workforce capabilities, equipment and material costs, seasonal factors, and supply chain constraints, all of which vary over time depending on other economic factors. For this reason, upgrade costs vary greatly by location and time. 

In this documentation, we provide information on how ComStock output variables could be matched with local data on upgrade costs to perform a stock-level assessment of upgrade costs. Different types of upgrades will have different cost considerations. Throughout the discussion, we assign “confidence” levels to each of the variables. See the table below for the confidence levels and their definitions. Furthermore, we highlight variables that are commonly used in cost assessment, but are missing from the ComStock output, with a “Not Output” tag. These confidence levels will be updated as additional improvements are added to ComStock.

<p style="text-align: center;"><b>Output Variable Confidence Level Definitions</b></p>

| **Confidence Level** | **Definition**                                                                                                                                                                                      |
| -------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| High                 | Output variable offers an appropriate approximation of the real commercial building stock and could be used directly in costing.                                                                    |
| Moderate             | Output variable could be used in costing, depending on the use case. Generally suitable for back-of-the-envelope calculations but may be off by a factor of 2–10.                                   |
| Low                  | There might be significant discrepancies between ComStock and real commercial building stock characteristics, and the variable should be used with caution. Results could be off by a factor of 10. |
| Not Output           | Variable could be used in a cost assessment but is not currently a ComStock output.                                                                                                                 |

For a full list of ComStock output variables, reference the “data_dictionary.tsv” file for the relevant release on the [Open Energy Data Initiative](https://data.openei.org/s3_viewer?bucket=oedi-data-lake&prefix=nrel-pds-building-stock%2Fend-use-load-profiles-for-us-building-stock%2F). For complete details about how ComStock models the following systems, see the [ComStock reference documentation]({{site.baseurl}}{% link docs/resources/resources.md %}#references).

### 1. Windows and Opaque Envelope - Caveats and Considerations
The most relevant variables for envelope upgrade costs—the surface areas of the windows, exterior walls, and roof—are listed in the table below.

|                        | **Variable Name**            | **Units**      | **Confidence Level** |
| ---------------------- | ---------------------------- | ------------- | -------------------- |
| **Roof Area**          | _out.params.ext_roof_area_   | m<sup>2</sup> | High                 |
| **Window Area**        | _out.params.ext_window_area_ | m<sup>2</sup> | Moderate             |
| **Exterior Wall Area** | _out.params.ext_wall_area_   | m<sup>2</sup> | Moderate             |
| **Window Count**       | N/A                          | #             | Not Output           |

In the most simplistic application of these variables, the total area of each component could be multiplied by the unit material cost of the upgrade, adding in the labor associated with installation. However, there are a few considerations for using these variables for cost analysis based on how ComStock calculates geometry. ComStock does not use real building geometry such as that found in lidar or building footprint databases; instead, it uses the [bar method](https://github.com/NatLabRockies/openstudio-model-articulation-gem/tree/develop/lib/measures/create_bar_from_doe_building_type_ratios) to estimate building geometry. A key limitation of this approach is that it underestimates exterior surface area, which impacts both window area and exterior opaque envelope. If these ComStock variables are used for a cost analysis, the user should be aware that they are likely underestimates for the real amounts of building surface area that would need to be upgraded. Furthermore, ComStock does not provide a *count* of windows upgraded in a building, which is impactful for window upgrade cost.

A few other ComStock variables, shown below, might aid in envelope upgrade cost calculation, depending on the cost data available or supplemental methods for determining retrofit material volume.

|                                 | **Variable Name**                                                 | **Variable Description**                                               | **Confidence Level** |
| ------------------------------- | ----------------------------------------------------------------- | ---------------------------------------------------------------------- | -------------- |
| **Floor area**                  | _in.sqft_                                                         | Building total floor area (ft<sup>2</sup>)                             | High           |
| **Original energy code**        | _in.energy_code_followed_during___original_building_construction_ | Energy code used during initial construction                           | Moderate       |
| **Building system energy code** | _in.energy_code_followed_last___\*_replacement_                   | Energy code used for most recent building system replacement           | Moderate       |
| **Floors**                      | _in.number_of_stories_                                            | Above-grade stories                                                    | Moderate       |
| **Total floors**                | _in.number_stories_                                               | Total floors (including below-grade)                                   | Moderate       |
| **Wall type**                   | _in.wall_construction_type_                                       | Type of construction (e.g., steel, wood)                               | Moderate       |
| **Building aspect ratio**       | _in.aspect_ratio_                                                 | Ratio of North/South facade length relative to East/West facade length | Moderate       |

\* Building system, such as walls, roof, lighting, and interior equipment.

### 2. Heating, Ventilating, and Air Conditioning (HVAC) - Caveats and Considerations
The ComStock calibration process utilizes energy use as the metric of performance, not necessarily equipment counts. For costing, this is relevant to many equipment categories, but especially HVAC. In ComStock, the total building heating or cooling equipment capacity is reasonably estimated, so cost methodologies should focus on backing out the number of units from the total capacity based on floor area, not using equipment counts directly. For some HVAC categories, ComStock does provide equipment counts and approximate size estimates of different equipment (e.g., 5-ton chiller), but these should be used cautiously. As a byproduct of the model creation process, the various heating and cooling zones created in the models could be substantially bigger or smaller than a typical zone that would be found in reality, leading to oversizing or undersizing of HVAC equipment.
 
In the table below, the most significant HVAC variables are highlighted.

|                                                                                                                    | **Variable Name**                                        | **Units**                                         | **Confidence Level** |
| ------------------------------------------------------------------------------------------------------------------ | -------------------------------------------------------- | ------------------------------------------------- | -------------- |
| **HVAC system energy code**                                                                                        | _in.energy_code_followed_during_last___hvac_replacement_ | Energy code used for most recent HVAC replacement | Moderate       |
| **Total HVAC capacity in building by equipment type (e.g., chiller, boiler, heat pump, water-air heat pump, VRF)** | _out.params.chiller_capacity_                            | tons                                              | High           |
|       | _out.params.boiler_capacity_                                                                                       | kBTU/hr                                                  | High                                              |
|       | _out.params.furnace_capacity_                                                                                      | kBTU/hr                                                  | High                                              |
|       | _out.params.heat_pump_cooling___capacity_                                                                          | kBTU/hr                                                  | High                                              |
|       | _out.params.heat_pump_heating___capacity_                                                                          | kBTU/hr                                                  | High                                              |
|       | _out.params.wa_hp_cooling_capacity_                                                                                | W                                                        | Moderate                                          |
|       | _out.params.wa_hp_heating_capacity_                                                                                | W                                                        | Moderate                                          |
|       | _out.params.vrf_total_indoor_unit__cooling_capacity_                                                              | W                                                        | Moderate                                          |
|       | _out.params.vrf_total_indoor_unit__heating_capacity_                                                              | W                                                        | Moderate                                          |
|       | _out.params.vrf_total_outdoor_unit__cooling_capacity_                                                             | W                                                        | Moderate                                          |
|       | _out.params.vrf_total_outdoor_unit__heating_capacity_                                                             | W                                                        | Moderate                                          |
| **Total building cooling capacity**                                                                                | _out.params.cooling_equipment_capacity_                  | tons                                              | Moderate       |
| **Total building heating capacity**                                                                                | _out.params.heating_equipment_                           | kBTU/hr                                           | Moderate       |
| **HVAC count variables x size (e.g., 10 5-ton units)**                                                             | _out.params.hvac_count_boilers__\*\*_                     | #                                                 | Low            |
|       | _out.params.hvac_count_chillers__\*\*_                                                                              | #                                                        | Low                                               |
|       | _out.params.hvac_count_dx_cooling__\*\*_                                                                            | #                                                        | Low                                               |
|       | _out.params.hvac_count_dx_heating__\*\*_                                                                            | #                                                        | Low                                               |
|       | _out.params.hvac_count_furnace__\*\*_                                                                               | #                                                        | Low                                               |
|       | _out.params.hvac_count_heat_pumps_cooling_                                                                         | #                                                        | Low                                               |
|       | _out.params.hvac_count_heat_pumps_heating_                                                                         | #                                                        | Low                                               |
|       | _out.params.vrf_indoor_unit_count_                                                                                 | #                                                        | Low                                               |
|       | _out.params.vrf_outdoor_unit_count_                                                                                | #                                                        | Low                                               |
| **Fan****, pump, motor sizing for whole building equipment**                                                       | N/A                                                      |                                                   | Not Output     |
| **Fan, pump, motor sizing for zonal equipment**                                                                    | N/A                                                      |                                                   | Not Output     |
| **Duct run lengths**                                                                                               | N/A                                                      |                                                 | Not Output     |

** Multiple variables with similar names. For example, _out.params.hvac_count_furnace__\*\*_ represents all of the different size bin counts in the output, such as _out.params.hvac_count_furnace_0_to_30_kbtuh_ and _out.params.hvac_count_furnace_65_to_135_kbtuh_.


### 3. Lighting - Caveats and Considerations
Similar to HVAC, ComStock provides estimates of capacity, in this case interior lighting power density, with high confidence, but the number of individual lighting fixtures is not provided in the output. Interior lighting costing methodologies could focus on backing out the number of fixtures desired for the upgrade based on the lighting power density and the building floor area, which is also output with high confidence from ComStock. ComStock also provides the peak power used in exterior lighting with moderate confidence. This is not a typical variable used in costing, but could perhaps be coupled with other information from ComStock such as building size and/or type to provide an approximation of the cost of upgrading these fixtures. 

|                                                    | **Variable Name**                            | **Units**        | **Confidence Level** |
| -------------------------------------------------- | -------------------------------------------- | ---------------- | -------------------- |
| **Whole building interior lighting power density** | _out.params.interior_lighting_power_density_ | W/ft<sup>2</sup> | High                 |
| **Peak exterior lighting electricity usage**       | _out.params.exterior_lighting_power_         | W                | Moderate             |
| **Lighting fixture counts**                        | N/A                                          | #                | Not Output           |
| **Exterior lighting fixture counts**               | N/A                                          | #                | Not Output           |

### 4. Designated Water Heating - Caveats and Considerations
For buildings that have separate water heating systems outside of co-generation with the HVAC system, data are provided on the total heating capacity volume with low confidence. Data on the counts of equipment, either given as total counts or counts by capacity size range, are provided in ComStock outputs with low confidence. In most ComStock models, a single water heater is used to meet the demands of the entire building. Some models use a booster water loop, in which case a separate water heater system will be modeled for the boost loop. Booster loops are included for buildings with kitchen space types, such as restaurants and schools.

|                                                    | **Variable Name**                                                                                          | **Units**                                                  | **Confidence Level** |
| -------------------------------------------------- | ---------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------- | -------------------- |
| **Service water heating energy code**              | _in.energy_code_followed_during_last_svc_water_htg_replacement_                                          | Energy code used for most recent water heating replacement | Moderate             |
| **Total water heating storage capacity**           | _out.params.hp_water_heater_total_volume_ | gal | Low |
|                                                    | _out.params.non_hp_water_heater_total_volume_               | gal                                                       | Low                  |
| **Total water heating capacity**                   | _out.params.hp_water_heater_capacity_                                                                      | W                                                          | Low                  |
| **Water heating equipment capacity by size range** | _out.params.hp_water_heater__\*__gal_total_volume_ | gal | Low |
|                                                    |_out.params.non_hp_water_heater__\*__gal_total_volume_ | gal                                                        | Low                  |
| **Water heating equipment count**                  | _out.params.hp_water_heater \_count_                                                                       | #                                                          | Low                  |
| **Whole service water heating equipment size**     | _out.params.hp_water_heater__\*__gal__count_                                                                | #                                                          | Low                  |
| **Piping lengths**                                 | N/A                                                                                                        | ft                                                         | Not Output           |

### 5. Other Equipment - Caveats and Considerations
ComStock provides equipment counts for some kitchen equipment listed below. In general, these could be used as orders of magnitude bounding for costing, but the overall confidence in using these variables is low. Potential methodologies for costing these components could focus on supplementing the floor area renovated with this equipment with external data on the average size of kitchen spaces and the counts of equipment typically present in these spaces.

|                           | **Variable Name**         | **Units** | **Confidence Level** |
| ------------------------- | ------------------------- | --------- | -------------------- |
| **Equipment counts**      | _out.params.num_broilers_ | #         | Low                  |
|                           | _out.params.num_fryers_   | #         | Low                  |
|                           | _out.params.num_griddles_ | #         | Low                  |
|                           | _out.params.num_ovens_    | #         | Low                  |
|                           | _out.params.num_ranges_   | #         | Low                  |
|                           | _out.params.num_steamers_ | #         | Low                  |
