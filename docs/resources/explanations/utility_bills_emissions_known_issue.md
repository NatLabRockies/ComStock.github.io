---
layout: default
title: Utility Bills and Emissions in ComStock 2024 Release 2
parent: Resources
nav_order: 9
---

# Issue Report: Utility Bills and Emissions in ComStock 2024 Release 2

## Summary of Issue
In ComStock’s new sampling method (effective starting with 2024 Release 2), a single energy model may be used multiple times to represent similar buildings across distinct but comparable geographies (e.g., census tracts) (see a description of the new sampling method [here]({{  site.baseurl  }}{% link docs/resources/explanations/new_sampling_method.md %})). Utility bills and emissions due to electricity are based on the utility and grid region where the original model was sampled. When a model is reallocated to a different location, the utility and grid region of the original model may no longer reflect those of the new location, resulting in incorrect utility bill and emissions values for the reallocated models. Natural gas, fuel oil, and propane utility bills are calculated using state-level data and may also be affected by this issue.

**Note:** This issue only affects emissions associated with electricity use. Non-electric emissions are calculated using national average emission factors found in *Table 5.1.2(1) National Average Emission Factors for Household Fuels defined in ANSI/RESNET/ICCC 301 Standard for the Calculation and Labeling of the Energy Performance of Dwelling and Sleeping Units using an Energy Rating Index* and are not affected by this issue.[^1]

## Status
Impacts ComStock 2024 Release 2 only. We are actively working to address this issue in future dataset releases.

## Details
ComStock’s new sampling method (effective starting with 2024 Release 2) involves the reuse of a single energy model to represent similar buildings across distinct but comparable geographies (e.g., census tracts). This approach improves the dataset's granularity but the existing utility bill and emissions calculation workflow and structure of the results files lead to potential inaccuracies in utility bill and emissions calculations. These inaccuracies arise because utility rates and emissions factors are calculated based on the location of the original energy model's sampling. When a model is reallocated to a different location, the utility and grid region of the original model may no longer reflect those of the new location. Users should exercise caution when analyzing certain fields in the dataset, described below, as they may contain incorrect or misleading values. The ComStock team is working to address this issue in future dataset releases.

For more details about how utility bills and emissions are calculated in ComStock, see the [reference documentation]({{site.baseurl}}{% link docs/resources/resources.md %}#references).

### Utility Bill Fields
The following fields related to utility bills are directly impacted by the reallocation of energy models to new locations. Since utility rates vary significantly by geography, these fields may not accurately represent the characteristics of the reallocated location:

<details>
    <summary>
        Affected Utility Bill Fields (click to expand)
    </summary>
        <ul>
            <li>out.utility_bills.electricity_utility_eia_id</li>
            <li>out.utility_bills.electricity_bill_intensity</li>
            <li>out.utility_bills.electricity_bill_max</li>
            <li>out.utility_bills.electricity_bill_mean</li>
            <li>out.utility_bills.electricity_bill_median</li>
            <li>out.utility_bills.electricity_bill_min</li>
            <li>out.utility_bills.electricity_bill_number_of_rates</li>
            <li>out.utility_bills.electricity_energy_rate</li>
            <li>out.utility_bills.fuel_oil_bill</li>
            <li>out.utility_bills.fuel_oil_bill_intensity</li>
            <li>out.utility_bills.fuel_oil_rate_name</li>
            <li>out.utility_bills.natural_gas_bill</li>
            <li>out.utility_bills.natural_gas_bill_intensity</li>
            <li>out.utility_bills.natural_gas_energy_rate</li>
            <li>out.utility_bills.natural_gas_rate_name</li>
            <li>out.utility_bills.propane_bill</li>
            <li>out.utility_bills.propane_bill_intensity</li>
            <li>out.utility_bills.propane_rate_name</li>
        </ul>
</details>

### Emissions Fields
Electric emissions calculations are similarly affected, as they depend on the grid mix or emissions factors specific to the model’s original location. Note that emissions from natural gas, propane, and fuel oil are not affected by this issue. The affected fields include:

<details>
    <summary>
        Affected Emissions Fields (click to expand)
    </summary>
        <ul>
            <li>in.cambium_grid_region</li>
            <li>out.emissions.electricity.aer_95_decarb_by_2035_from_2023</li>
            <li>out.emissions.electricity.aer_95_decarb_by_2050_from_2023</li>
            <li>out.emissions.electricity.aer_high_re_cost_from_2023</li>
            <li>out.emissions.electricity.aer_low_re_cost_from_2023</li>
            <li>out.emissions.electricity.aer_mid_case_from_2023</li>
            <li>out.emissions.electricity.cooling.aer_95_decarb_by_2035_from_2023</li>
            <li>out.emissions.electricity.cooling.aer_95_decarb_by_2050_from_2023</li>
            <li>out.emissions.electricity.cooling.aer_high_re_cost_from_2023</li>
            <li>out.emissions.electricity.cooling.aer_low_re_cost_from_2023</li>
            <li>out.emissions.electricity.cooling.aer_mid_case_from_2023</li>
            <li>out.emissions.electricity.cooling.egrid_2018_state</li>
            <li>out.emissions.electricity.cooling.egrid_2018_subregion</li>
            <li>out.emissions.electricity.cooling.egrid_2019_state</li>
            <li>out.emissions.electricity.cooling.egrid_2019_subregion</li>
            <li>out.emissions.electricity.cooling.egrid_2020_state</li>
            <li>out.emissions.electricity.cooling.egrid_2020_subregion</li>
            <li>out.emissions.electricity.cooling.egrid_2021_state</li>
            <li>out.emissions.electricity.cooling.egrid_2021_subregion</li>
            <li>out.emissions.electricity.cooling.lrmer_95_decarb_by_2035_15_2023_start</li>
            <li>out.emissions.electricity.cooling.lrmer_95_decarb_by_2035_15_2025_start</li>
            <li>out.emissions.electricity.cooling.lrmer_95_decarb_by_2035_25_2025_start</li>
            <li>out.emissions.electricity.cooling.lrmer_95_decarb_by_2035_30_2023_start</li>
            <li>out.emissions.electricity.cooling.lrmer_95_decarb_by_2050_15_2023_start</li>
            <li>out.emissions.electricity.cooling.lrmer_95_decarb_by_2050_30_2023_start</li>
            <li>out.emissions.electricity.cooling.lrmer_high_re_cost_15_2023_start</li>
            <li>out.emissions.electricity.cooling.lrmer_high_re_cost_30_2023_start</li>
            <li>out.emissions.electricity.cooling.lrmer_low_re_cost_15_2023_start</li>
            <li>out.emissions.electricity.cooling.lrmer_low_re_cost_15_2025_start</li>
            <li>out.emissions.electricity.cooling.lrmer_low_re_cost_25_2025_start</li>
            <li>out.emissions.electricity.cooling.lrmer_low_re_cost_30_2023_start</li>
            <li>out.emissions.electricity.cooling.lrmer_mid_case_15_2023_start</li>
            <li>out.emissions.electricity.cooling.lrmer_mid_case_15_2025_start</li>
            <li>out.emissions.electricity.cooling.lrmer_mid_case_25_2025_start</li>
            <li>out.emissions.electricity.cooling.lrmer_mid_case_30_2023_start</li>
            <li>out.emissions.electricity.egrid_2018_state</li>
            <li>out.emissions.electricity.egrid_2018_subregion</li>
            <li>out.emissions.electricity.egrid_2019_state</li>
            <li>out.emissions.electricity.egrid_2019_subregion</li>
            <li>out.emissions.electricity.egrid_2020_state</li>
            <li>out.emissions.electricity.egrid_2020_subregion</li>
            <li>out.emissions.electricity.egrid_2021_state</li>
            <li>out.emissions.electricity.egrid_2021_subregion</li>
            <li>out.emissions.electricity.enduse_group.hvac.aer_95_decarb_by_2035_from_2023</li>
            <li>out.emissions.electricity.enduse_group.hvac.aer_95_decarb_by_2050_from_2023</li>
            <li>out.emissions.electricity.enduse_group.hvac.aer_high_re_cost_from_2023</li>
            <li>out.emissions.electricity.enduse_group.hvac.aer_low_re_cost_from_2023</li>
            <li>out.emissions.electricity.enduse_group.hvac.aer_mid_case_from_2023</li>
            <li>out.emissions.electricity.enduse_group.hvac.egrid_2018_state</li>
            <li>out.emissions.electricity.enduse_group.hvac.egrid_2018_subregion</li>
            <li>out.emissions.electricity.enduse_group.hvac.egrid_2019_state</li>
            <li>out.emissions.electricity.enduse_group.hvac.egrid_2019_subregion</li>
            <li>out.emissions.electricity.enduse_group.hvac.egrid_2020_state</li>
            <li>out.emissions.electricity.enduse_group.hvac.egrid_2020_subregion</li>
            <li>out.emissions.electricity.enduse_group.hvac.egrid_2021_state</li>
            <li>out.emissions.electricity.enduse_group.hvac.egrid_2021_subregion</li>
            <li>out.emissions.electricity.enduse_group.hvac.lrmer_95_decarb_by_2035_15_2023_start</li>
            <li>out.emissions.electricity.enduse_group.hvac.lrmer_95_decarb_by_2035_15_2025_start</li>
            <li>out.emissions.electricity.enduse_group.hvac.lrmer_95_decarb_by_2035_25_2025_start</li>
            <li>out.emissions.electricity.enduse_group.hvac.lrmer_95_decarb_by_2035_30_2023_start</li>
            <li>out.emissions.electricity.enduse_group.hvac.lrmer_95_decarb_by_2050_15_2023_start</li>
            <li>out.emissions.electricity.enduse_group.hvac.lrmer_95_decarb_by_2050_30_2023_start</li>
            <li>out.emissions.electricity.enduse_group.hvac.lrmer_high_re_cost_15_2023_start</li>
            <li>out.emissions.electricity.enduse_group.hvac.lrmer_high_re_cost_30_2023_start</li>
            <li>out.emissions.electricity.enduse_group.hvac.lrmer_low_re_cost_15_2023_start</li>
            <li>out.emissions.electricity.enduse_group.hvac.lrmer_low_re_cost_15_2025_start</li>
            <li>out.emissions.electricity.enduse_group.hvac.lrmer_low_re_cost_25_2025_start</li>
            <li>out.emissions.electricity.enduse_group.hvac.lrmer_low_re_cost_30_2023_start</li>
            <li>out.emissions.electricity.enduse_group.hvac.lrmer_mid_case_15_2023_start</li>
            <li>out.emissions.electricity.enduse_group.hvac.lrmer_mid_case_15_2025_start</li>
            <li>out.emissions.electricity.enduse_group.hvac.lrmer_mid_case_25_2025_start</li>
            <li>out.emissions.electricity.enduse_group.hvac.lrmer_mid_case_30_2023_start</li>
            <li>out.emissions.electricity.exterior_lighting.aer_95_decarb_by_2035_from_2023</li>
            <li>out.emissions.electricity.exterior_lighting.aer_95_decarb_by_2050_from_2023</li>
            <li>out.emissions.electricity.exterior_lighting.aer_high_re_cost_from_2023</li>
            <li>out.emissions.electricity.exterior_lighting.aer_low_re_cost_from_2023</li>
            <li>out.emissions.electricity.exterior_lighting.aer_mid_case_from_2023</li>
            <li>out.emissions.electricity.exterior_lighting.egrid_2018_state</li>
            <li>out.emissions.electricity.exterior_lighting.egrid_2018_subregion</li>
            <li>out.emissions.electricity.exterior_lighting.egrid_2019_state</li>
            <li>out.emissions.electricity.exterior_lighting.egrid_2019_subregion</li>
            <li>out.emissions.electricity.exterior_lighting.egrid_2020_state</li>
            <li>out.emissions.electricity.exterior_lighting.egrid_2020_subregion</li>
            <li>out.emissions.electricity.exterior_lighting.egrid_2021_state</li>
            <li>out.emissions.electricity.exterior_lighting.egrid_2021_subregion</li>
            <li>out.emissions.electricity.exterior_lighting.lrmer_95_decarb_by_2035_15_2023_start</li>
            <li>out.emissions.electricity.exterior_lighting.lrmer_95_decarb_by_2035_15_2025_start</li>
            <li>out.emissions.electricity.exterior_lighting.lrmer_95_decarb_by_2035_25_2025_start</li>
            <li>out.emissions.electricity.exterior_lighting.lrmer_95_decarb_by_2035_30_2023_start</li>
            <li>out.emissions.electricity.exterior_lighting.lrmer_95_decarb_by_2050_15_2023_start</li>
            <li>out.emissions.electricity.exterior_lighting.lrmer_95_decarb_by_2050_30_2023_start</li>
            <li>out.emissions.electricity.exterior_lighting.lrmer_high_re_cost_15_2023_start</li>
            <li>out.emissions.electricity.exterior_lighting.lrmer_high_re_cost_30_2023_start</li>
            <li>out.emissions.electricity.exterior_lighting.lrmer_low_re_cost_15_2023_start</li>
            <li>out.emissions.electricity.exterior_lighting.lrmer_low_re_cost_15_2025_start</li>
            <li>out.emissions.electricity.exterior_lighting.lrmer_low_re_cost_25_2025_start</li>
            <li>out.emissions.electricity.exterior_lighting.lrmer_low_re_cost_30_2023_start</li>
            <li>out.emissions.electricity.exterior_lighting.lrmer_mid_case_15_2023_start</li>
            <li>out.emissions.electricity.exterior_lighting.lrmer_mid_case_15_2025_start</li>
            <li>out.emissions.electricity.exterior_lighting.lrmer_mid_case_25_2025_start</li>
            <li>out.emissions.electricity.exterior_lighting.lrmer_mid_case_30_2023_start</li>
            <li>out.emissions.electricity.heating.aer_95_decarb_by_2035_from_2023</li>
            <li>out.emissions.electricity.heating.aer_95_decarb_by_2050_from_2023</li>
            <li>out.emissions.electricity.heating.aer_high_re_cost_from_2023</li>
            <li>out.emissions.electricity.heating.aer_low_re_cost_from_2023</li>
            <li>out.emissions.electricity.heating.aer_mid_case_from_2023</li>
            <li>out.emissions.electricity.heating.egrid_2018_state</li>
            <li>out.emissions.electricity.heating.egrid_2018_subregion</li>
            <li>out.emissions.electricity.heating.egrid_2019_state</li>
            <li>out.emissions.electricity.heating.egrid_2019_subregion</li>
            <li>out.emissions.electricity.heating.egrid_2020_state</li>
            <li>out.emissions.electricity.heating.egrid_2020_subregion</li>
            <li>out.emissions.electricity.heating.egrid_2021_state</li>
            <li>out.emissions.electricity.heating.egrid_2021_subregion</li>
            <li>out.emissions.electricity.heating.lrmer_95_decarb_by_2035_15_2023_start</li>
            <li>out.emissions.electricity.heating.lrmer_95_decarb_by_2035_15_2025_start</li>
            <li>out.emissions.electricity.heating.lrmer_95_decarb_by_2035_25_2025_start</li>
            <li>out.emissions.electricity.heating.lrmer_95_decarb_by_2035_30_2023_start</li>
            <li>out.emissions.electricity.heating.lrmer_95_decarb_by_2050_15_2023_start</li>
            <li>out.emissions.electricity.heating.lrmer_95_decarb_by_2050_30_2023_start</li>
            <li>out.emissions.electricity.heating.lrmer_high_re_cost_15_2023_start</li>
            <li>out.emissions.electricity.heating.lrmer_high_re_cost_30_2023_start</li>
            <li>out.emissions.electricity.heating.lrmer_low_re_cost_15_2023_start</li>
            <li>out.emissions.electricity.heating.lrmer_low_re_cost_15_2025_start</li>
            <li>out.emissions.electricity.heating.lrmer_low_re_cost_25_2025_start</li>
            <li>out.emissions.electricity.heating.lrmer_low_re_cost_30_2023_start</li>
            <li>out.emissions.electricity.heating.lrmer_mid_case_15_2023_start</li>
            <li>out.emissions.electricity.heating.lrmer_mid_case_15_2025_start</li>
            <li>out.emissions.electricity.heating.lrmer_mid_case_25_2025_start</li>
            <li>out.emissions.electricity.heating.lrmer_mid_case_30_2023_start</li>
            <li>out.emissions.electricity.interior_equipment.aer_95_decarb_by_2035_from_2023</li>
            <li>out.emissions.electricity.interior_equipment.aer_95_decarb_by_2050_from_2023</li>
            <li>out.emissions.electricity.interior_equipment.aer_high_re_cost_from_2023</li>
            <li>out.emissions.electricity.interior_equipment.aer_low_re_cost_from_2023</li>
            <li>out.emissions.electricity.interior_equipment.aer_mid_case_from_2023</li>
            <li>out.emissions.electricity.interior_equipment.egrid_2018_state</li>
            <li>out.emissions.electricity.interior_equipment.egrid_2018_subregion</li>
            <li>out.emissions.electricity.interior_equipment.egrid_2019_state</li>
            <li>out.emissions.electricity.interior_equipment.egrid_2019_subregion</li>
            <li>out.emissions.electricity.interior_equipment.egrid_2020_state</li>
            <li>out.emissions.electricity.interior_equipment.egrid_2020_subregion</li>
            <li>out.emissions.electricity.interior_equipment.egrid_2021_state</li>
            <li>out.emissions.electricity.interior_equipment.egrid_2021_subregion</li>
            <li>out.emissions.electricity.interior_equipment.lrmer_95_decarb_by_2035_15_2023_start</li>
            <li>out.emissions.electricity.interior_equipment.lrmer_95_decarb_by_2035_15_2025_start</li>
            <li>out.emissions.electricity.interior_equipment.lrmer_95_decarb_by_2035_25_2025_start</li>
            <li>out.emissions.electricity.interior_equipment.lrmer_95_decarb_by_2035_30_2023_start</li>
            <li>out.emissions.electricity.interior_equipment.lrmer_95_decarb_by_2050_15_2023_start</li>
            <li>out.emissions.electricity.interior_equipment.lrmer_95_decarb_by_2050_30_2023_start</li>
            <li>out.emissions.electricity.interior_equipment.lrmer_high_re_cost_15_2023_start</li>
            <li>out.emissions.electricity.interior_equipment.lrmer_high_re_cost_30_2023_start</li>
            <li>out.emissions.electricity.interior_equipment.lrmer_low_re_cost_15_2023_start</li>
            <li>out.emissions.electricity.interior_equipment.lrmer_low_re_cost_15_2025_start</li>
            <li>out.emissions.electricity.interior_equipment.lrmer_low_re_cost_25_2025_start</li>
            <li>out.emissions.electricity.interior_equipment.lrmer_low_re_cost_30_2023_start</li>
            <li>out.emissions.electricity.interior_equipment.lrmer_mid_case_15_2023_start</li>
            <li>out.emissions.electricity.interior_equipment.lrmer_mid_case_15_2025_start</li>
            <li>out.emissions.electricity.interior_equipment.lrmer_mid_case_25_2025_start</li>
            <li>out.emissions.electricity.interior_equipment.lrmer_mid_case_30_2023_start</li>
            <li>out.emissions.electricity.interior_equipment_fuel_oil_ghg_emissions</li>
            <li>out.emissions.electricity.interior_equipment_natural_gas_ghg_emissions</li>
            <li>out.emissions.electricity.interior_equipment_propane_ghg_emissions</li>
            <li>out.emissions.electricity.interior_lighting.aer_95_decarb_by_2035_from_2023</li>
            <li>out.emissions.electricity.interior_lighting.aer_95_decarb_by_2050_from_2023</li>
            <li>out.emissions.electricity.interior_lighting.aer_high_re_cost_from_2023</li>
            <li>out.emissions.electricity.interior_lighting.aer_low_re_cost_from_2023</li>
            <li>out.emissions.electricity.interior_lighting.aer_mid_case_from_2023</li>
            <li>out.emissions.electricity.interior_lighting.egrid_2018_state</li>
            <li>out.emissions.electricity.interior_lighting.egrid_2018_subregion</li>
            <li>out.emissions.electricity.interior_lighting.egrid_2019_state</li>
            <li>out.emissions.electricity.interior_lighting.egrid_2019_subregion</li>
            <li>out.emissions.electricity.interior_lighting.egrid_2020_state</li>
            <li>out.emissions.electricity.interior_lighting.egrid_2020_subregion</li>
            <li>out.emissions.electricity.interior_lighting.egrid_2021_state</li>
            <li>out.emissions.electricity.interior_lighting.egrid_2021_subregion</li>
            <li>out.emissions.electricity.interior_lighting.lrmer_95_decarb_by_2035_15_2023_start</li>
            <li>out.emissions.electricity.interior_lighting.lrmer_95_decarb_by_2035_15_2025_start</li>
            <li>out.emissions.electricity.interior_lighting.lrmer_95_decarb_by_2035_25_2025_start</li>
            <li>out.emissions.electricity.interior_lighting.lrmer_95_decarb_by_2035_30_2023_start</li>
            <li>out.emissions.electricity.interior_lighting.lrmer_95_decarb_by_2050_15_2023_start</li>
            <li>out.emissions.electricity.interior_lighting.lrmer_95_decarb_by_2050_30_2023_start</li>
            <li>out.emissions.electricity.interior_lighting.lrmer_high_re_cost_15_2023_start</li>
            <li>out.emissions.electricity.interior_lighting.lrmer_high_re_cost_30_2023_start</li>
            <li>out.emissions.electricity.interior_lighting.lrmer_low_re_cost_15_2023_start</li>
            <li>out.emissions.electricity.interior_lighting.lrmer_low_re_cost_15_2025_start</li>
            <li>out.emissions.electricity.interior_lighting.lrmer_low_re_cost_25_2025_start</li>
            <li>out.emissions.electricity.interior_lighting.lrmer_low_re_cost_30_2023_start</li>
            <li>out.emissions.electricity.interior_lighting.lrmer_mid_case_15_2023_start</li>
            <li>out.emissions.electricity.interior_lighting.lrmer_mid_case_15_2025_start</li>
            <li>out.emissions.electricity.interior_lighting.lrmer_mid_case_25_2025_start</li>
            <li>out.emissions.electricity.interior_lighting.lrmer_mid_case_30_2023_start</li>
            <li>out.emissions.electricity.lrmer_95_decarb_by_2035_15_2023_start</li>
            <li>out.emissions.electricity.lrmer_95_decarb_by_2035_15_2025_start</li>
            <li>out.emissions.electricity.lrmer_95_decarb_by_2035_25_2025_start</li>
            <li>out.emissions.electricity.lrmer_95_decarb_by_2035_30_2023_start</li>
            <li>out.emissions.electricity.lrmer_95_decarb_by_2050_15_2023_start</li>
            <li>out.emissions.electricity.lrmer_95_decarb_by_2050_30_2023_start</li>
            <li>out.emissions.electricity.lrmer_high_re_cost_15_2023_start</li>
            <li>out.emissions.electricity.lrmer_high_re_cost_30_2023_start</li>
            <li>out.emissions.electricity.lrmer_low_re_cost_15_2023_start</li>
            <li>out.emissions.electricity.lrmer_low_re_cost_15_2025_start</li>
            <li>out.emissions.electricity.lrmer_low_re_cost_25_2025_start</li>
            <li>out.emissions.electricity.lrmer_low_re_cost_30_2023_start</li>
            <li>out.emissions.electricity.lrmer_mid_case_15_2023_start</li>
            <li>out.emissions.electricity.lrmer_mid_case_15_2025_start</li>
            <li>out.emissions.electricity.lrmer_mid_case_25_2025_start</li>
            <li>out.emissions.electricity.lrmer_mid_case_30_2023_start</li>
            <li>out.emissions.electricity.refrigeration.aer_95_decarb_by_2035_from_2023</li>
            <li>out.emissions.electricity.refrigeration.aer_95_decarb_by_2050_from_2023</li>
            <li>out.emissions.electricity.refrigeration.aer_high_re_cost_from_2023</li>
            <li>out.emissions.electricity.refrigeration.aer_low_re_cost_from_2023</li>
            <li>out.emissions.electricity.refrigeration.aer_mid_case_from_2023</li>
            <li>out.emissions.electricity.refrigeration.egrid_2018_state</li>
            <li>out.emissions.electricity.refrigeration.egrid_2018_subregion</li>
            <li>out.emissions.electricity.refrigeration.egrid_2019_state</li>
            <li>out.emissions.electricity.refrigeration.egrid_2019_subregion</li>
            <li>out.emissions.electricity.refrigeration.egrid_2020_state</li>
            <li>out.emissions.electricity.refrigeration.egrid_2020_subregion</li>
            <li>out.emissions.electricity.refrigeration.egrid_2021_state</li>
            <li>out.emissions.electricity.refrigeration.egrid_2021_subregion</li>
            <li>out.emissions.electricity.refrigeration.lrmer_95_decarb_by_2035_15_2023_start</li>
            <li>out.emissions.electricity.refrigeration.lrmer_95_decarb_by_2035_15_2025_start</li>
            <li>out.emissions.electricity.refrigeration.lrmer_95_decarb_by_2035_25_2025_start</li>
            <li>out.emissions.electricity.refrigeration.lrmer_95_decarb_by_2035_30_2023_start</li>
            <li>out.emissions.electricity.refrigeration.lrmer_95_decarb_by_2050_15_2023_start</li>
            <li>out.emissions.electricity.refrigeration.lrmer_95_decarb_by_2050_30_2023_start</li>
            <li>out.emissions.electricity.refrigeration.lrmer_high_re_cost_15_2023_start</li>
            <li>out.emissions.electricity.refrigeration.lrmer_high_re_cost_30_2023_start</li>
            <li>out.emissions.electricity.refrigeration.lrmer_low_re_cost_15_2023_start</li>
            <li>out.emissions.electricity.refrigeration.lrmer_low_re_cost_15_2025_start</li>
            <li>out.emissions.electricity.refrigeration.lrmer_low_re_cost_25_2025_start</li>
            <li>out.emissions.electricity.refrigeration.lrmer_low_re_cost_30_2023_start</li>
            <li>out.emissions.electricity.refrigeration.lrmer_mid_case_15_2023_start</li>
            <li>out.emissions.electricity.refrigeration.lrmer_mid_case_15_2025_start</li>
            <li>out.emissions.electricity.refrigeration.lrmer_mid_case_25_2025_start</li>
            <li>out.emissions.electricity.refrigeration.lrmer_mid_case_30_2023_start</li>
            <li>out.emissions.electricity.water_systems.aer_95_decarb_by_2035_from_2023</li>
            <li>out.emissions.electricity.water_systems.aer_95_decarb_by_2050_from_2023</li>
            <li>out.emissions.electricity.water_systems.aer_high_re_cost_from_2023</li>
            <li>out.emissions.electricity.water_systems.aer_low_re_cost_from_2023</li>
            <li>out.emissions.electricity.water_systems.aer_mid_case_from_2023</li>
            <li>out.emissions.electricity.water_systems.egrid_2018_state</li>
            <li>out.emissions.electricity.water_systems.egrid_2018_subregion</li>
            <li>out.emissions.electricity.water_systems.egrid_2019_state</li>
            <li>out.emissions.electricity.water_systems.egrid_2019_subregion</li>
            <li>out.emissions.electricity.water_systems.egrid_2020_state</li>
            <li>out.emissions.electricity.water_systems.egrid_2020_subregion</li>
            <li>out.emissions.electricity.water_systems.egrid_2021_state</li>
            <li>out.emissions.electricity.water_systems.egrid_2021_subregion</li>
            <li>out.emissions.electricity.water_systems.lrmer_95_decarb_by_2035_15_2023_start</li>
            <li>out.emissions.electricity.water_systems.lrmer_95_decarb_by_2035_15_2025_start</li>
            <li>out.emissions.electricity.water_systems.lrmer_95_decarb_by_2035_25_2025_start</li>
            <li>out.emissions.electricity.water_systems.lrmer_95_decarb_by_2035_30_2023_start</li>
            <li>out.emissions.electricity.water_systems.lrmer_95_decarb_by_2050_15_2023_start</li>
            <li>out.emissions.electricity.water_systems.lrmer_95_decarb_by_2050_30_2023_start</li>
            <li>out.emissions.electricity.water_systems.lrmer_high_re_cost_15_2023_start</li>
            <li>out.emissions.electricity.water_systems.lrmer_high_re_cost_30_2023_start</li>
            <li>out.emissions.electricity.water_systems.lrmer_low_re_cost_15_2023_start</li>
            <li>out.emissions.electricity.water_systems.lrmer_low_re_cost_15_2025_start</li>
            <li>out.emissions.electricity.water_systems.lrmer_low_re_cost_25_2025_start</li>
            <li>out.emissions.electricity.water_systems.lrmer_low_re_cost_30_2023_start</li>
            <li>out.emissions.electricity.water_systems.lrmer_mid_case_15_2023_start</li>
            <li>out.emissions.electricity.water_systems.lrmer_mid_case_15_2025_start</li>
            <li>out.emissions.electricity.water_systems.lrmer_mid_case_25_2025_start</li>
            <li>out.emissions.electricity.water_systems.lrmer_mid_case_30_2023_start</li>
            <li>calc.emissions.total_with_cambium_mid_case_15y</li>
            <li>calc.emissions.total_with_egrid</li>
            <li>calc.weighted.emissions.electricity.egrid_2021_subregion</li>
            <li>calc.weighted.emissions.electricity.lrmer_high_re_cost_15_2023_start</li>
            <li>calc.weighted.emissions.electricity.lrmer_low_re_cost_15_2023_start</li>
            <li>calc.weighted.emissions.total_with_cambium_mid_case_15y</li>
            <li>calc.weighted.emissions.total_with_egrid</li>
            <li>calc.weighted.enduse_group.electricity.hvac.emissions.egrid_2021_subregion</li>
            <li>calc.weighted.enduse_group.electricity.interior_equipment.emissions.egrid_2021_subregion</li>
            <li>calc.weighted.enduse_group.electricity.lighting.emissions.egrid_2021_subregion</li>
            <li>calc.weighted.enduse_group.electricity.refrigeration.emissions.egrid_2021_subregion</li>
            <li>calc.weighted.enduse_group.electricity.water_systems.emissions.egrid_2021_subregion</li>
            <li>calc.weighted.enduse_group.site_energy.hvac.emissions</li>
            <li>calc.weighted.enduse_group.site_energy.interior_equipment.emissions</li>
            <li>calc.weighted.enduse_group.site_energy.lighting.emissions</li>
            <li>calc.weighted.enduse_group.site_energy.refrigeration.emissions</li>
            <li>calc.weighted.enduse_group.site_energy.water_systems.emissions</li>
        </ul>
</details>

## Impacts
This issue can have a significant impact on utility bills, as utility rates vary widely by location. Effects on emissions are expected to be less impactful due to the small relative difference in emissions factors across neighboring grid regions.

## Recommendations

### Validate Geographic Consistency
Confirm whether the reallocated model’s location aligns with the original model’s location, including corresponding utility and grid region. If the original model and reallocated one are in the same census tract, then they are in the same utility and grid region. Use the field “in.as_simulated_nhgis_tract_gisjoin” to find the tract of the original, or “as-simulated,” model, and “in.nhgis_tract_gisjoin” to identify the reallocated tract. If the census tracts are not the same, use the following steps to identify whether the utility and grid regions match.

Use the [tract_to_elec_util.csv](https://github.com/NatLabRockies/ComStock/blob/main/measures/utility_bills/resources/tract_to_elec_util.csv) file on the public ComStock GitHub repository to determine whether the as-simulated model location and reallocated one are in the same utility. For example, if the “eia_id” in the tract_to_elec_util.csv file corresponding to the “in.as_simulated_nhgis_tract_gisjoin” is different than the “eia_id” corresponding to the “in.nhgis_tract_gisjoin”, the utility bill calculation is wrong and we do not recommend using the utility bills for the reallocated model.

For emissions, a helpful field to identify whether the as-simulated location is appropriate for the reallocated one is “in.cambium_grid_region.” This represents the as-simulated Cambium grid region used to calculate electric emissions. Compare the “in.cambium_grid_region” with the grid region of the reallocated model using the [spatial_tract_lookup_table.csv][1] on the OEDI Data Lake and the “in.nhgis_tract_gisjoin.” If the two do not match, there is likely a discrepancy between the emissions of the original model and those of the model in its new location. As mentioned earlier, there is generally a small difference in emissions factors across neighboring grid regions, and this discrepancy may not have a significant impact on the analysis. Users should exercise their best judgement in this case.

### Be Cautious with Aggregation
The aggregate metadata and annual results files (available on the [OEDI Data Lake](https://data.openei.org/s3_viewer?bucket=oedi-data-lake&prefix=nrel-pds-building-stock%2Fend-use-load-profiles-for-us-building-stock%2F2024%2Fcomstock_amy2018_release_2%2F)) consolidate data by combining duplicate, reallocated models in the geography. Each building ID appears only once, with an associated weight representing the sum of the weights of all instances of the building ID in the geography. Other fields, such as utility bills and emissions, are also aggregated for each building ID. However, because the as-simulated utility rates and emissions factors may not align with those of the reallocated models, summing these values for a given building ID simply scales the original model’s utility bills and emissions by the sum of its reallocated weights. This approach does not capture the geographic diversity introduced by the reallocation process. We do not recommend using utility bills from the aggregate files for analysis. Emission data in these files may be incorrect, though the overall trend is likely directionally accurate. Please use this data with caution.


[^1]: <https://www.resnet.us/wp-content/uploads/archive/resblog/2019/01/ANSIRESNETICC301-2019_vf1.23.19.pdf>
[1]:../../../assets/files/spatial_tract_lookup_table_v8.csv