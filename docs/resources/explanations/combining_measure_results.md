---
layout: default
title: Combining Measure Results
parent: Resources
nav_order: 4
---

# Why Individual ComStock Measure Results Should Not Be Combined

Some users of ComStock data might have questions about the combined effects of multiple upgrade measures being installed together. However, unless the upgrades have been _jointly run as an upgrade package_ in ComStock, the energy savings results **should not** be added together. Using the ComStock 2023 Release 2 data as an example, we’ll explore a few upgrades and why adding the results of multiple upgrades together can be misleading.

## Example from 2023 Release 2: HVAC and Envelope
Hypothetically, a user might want to know the effect of upgrading existing HVAC equipment to a [Heat Pump RTU](https://www.nlr.gov/docs/fy24osti/86585.pdf) while also adding a [Window Film]({{site.baseurl}}{% link docs/upgrade_measures/env_window_film.md %}) to modify the solar heat gain. However, adding the savings of each of these upgrades together does not yield the true total savings because it does not capture the combined interactions between measures, which impacts the results. 

In   commercial buildings, almost all energy consumption by any fuel type has an impact on the heat flows within the building, and the heat transfer is often quite complex. In this example, the Heat Pump RTU upgrade impacts the energy necessary for the HVAC equipment to provide heating and cooling to the building. Alternatively, the Window Film upgrade changes the heat transfer between the interior of the building and the exterior environment. This changes the amount of heating and cooling that needs to be delivered by the HVAC equipment since it alters how heat is lost to or gained from the exterior environment. Simply summing the individual savings results of these two measures would ignore the interactive impact of the load changes the measures impose on each other, which can impact results.

For that reason, the two upgrade measures cannot be added together—one addresses the energy necessary to meet the heating and cooling demands in the base condition via equipment efficiency, whereas the other is altering the heating and cooling demands themselves. Understanding the combined energy savings of these two upgrades requires a dedicated model run of ComStock where the appropriate heat transfer equations can be solved for the combined upgrade scenario. These two upgrade measures were not run together as part of ComStock’s 2023 Release 1 or Release 2, so for the moment, Heat Pump RTUs and Window Films cannot be co-assessed.


## Example from 2023 Release 2: HVAC and Lighting
The “HVAC and Envelope” example concerns two upgrade technologies that directly impact HVAC needs of a building, so it’s more obvious why the individual savings results cannot be added together since there is interaction between the building envelope composition and the HVAC system. However, the same is true for upgrades that don’t directly concern HVAC or thermal envelope. For example, the energy savings from the [LED Lighting](https://www.nlr.gov/docs/fy24osti/86100.pdf) measure from that same data release also should not be added to the Heat Pump RTU upgrade because lighting also impacts the heat balance of the building. Lighting systems in buildings give off heat, and the amount of heat depends on the lighting technology. For example, LED lighting gives off notably less heat than other commercial building lighting technologies such as incandescent, fluorescent, or halogen fixtures. For the LED lighting upgrade, the new LED lighting system gives off less heat than the previously installed technology. This causes a decrease in cooling loads and an increase in heating loads that will need to be addressed by the new Heat Pump RTU HVAC system. Understanding the joint impact of the LED Lighting and the Heat Pump RTU upgrade requires a new model run with these upgrades together as a package to appropriately model the interactive effects between the measures. 

In ComStock 2023 Release 2, these two upgrades were in fact run together as a package: [Package 2: LED Lighting + HP RTU or HP Boilers](https://www.nlr.gov/docs/fy24osti/86601.pdf). Because of this, we can examine the error that would occur if we tried to add the individual savings from the LED Lighting and Heat Pump RTU measures together instead of running them as a package. We pull three individual modeled buildings from ComStock 2023 Release 2 as examples (see table, below). Note that models with the package applied have either the Heat Pump RTU _or_ Heat Pump Boiler measures applied, not both. For the three models in this example, the Heat Pump RTU measure was applied with the LED Lighting measure. For each model, we assess the annual site energy savings (*out.site_energy.total.energy_savings*) from each upgrade—the individual Heat Pump RTU and LED Lighting measures, and with the package. We can then compare the savings results from adding the individual measures with the package run. In this example, errors range from -10.4% to 15.1%.

<p style="text-align: center;"><b>Energy Savings by ComStock 2023 Release 2 Upgrade</b></p>

<table>
    <tr>
        <td rowspan="2"><b>Building ID</b></td>
        <td rowspan="2"><b>Building Type</b></td>
        <td rowspan="2"><b>State</b></td>
        <td style="text-align: center" colspan="5"><b>Annual Site Energy Savings (kWh)</b></td>
    </tr>
    <tr>
        <td><b>1. Heat Pump RTU – only</b></td>
        <td><b>10. LED Lighting – only</b></td>
        <td><b>17. Package 2, LED + HP RTU or HP Boiler</b></td>
        <td><b>Sum Upgrades 1 + 10</b></td>
        <td><b>% Error</b></td>
    </tr>
    <tr>
        <td>62841</td>
        <td>Strip Mall</td>
        <td>CO</td>
        <td>114,644</td>
        <td>47,778</td>
        <td>181,364</td>
        <td>162,422</td>
        <td>-10.40%</td>
    </tr>
    <tr>
        <td>93569</td>
        <td>Medium Office</td>
        <td>FL</td>
        <td>584,830</td>
        <td>71,372</td>
        <td>569,650</td>
        <td>656,202</td>
        <td>15.10%</td>
    </tr>
    <tr>
        <td>205619</td>
        <td>Full-Service Restaurant</td>
        <td>NY</td>
        <td>40,492</td>
        <td>10,317</td>
        <td>55,694</td>
        <td>50,809</td>
        <td>-8.80%</td>
    </tr>
</table>


This same logic can be extrapolated to nearly any energy-consuming equipment in the building such as water heating systems, cooking equipment, electronics, and other internal loads. Upgrade measures that do not directly involve HVAC equipment can yield misleading results due to interactions between the upgrades, such as an interior lighting technology upgrade paired with lighting controls. Additionally, consumption of energy typically gives off some sort of heat that impacts heating and cooling loads. In extremely rare circumstances, energy savings from a piece of electric equipment which gives off minimal heat might be able to be added to another upgrade measure as a rough approximation of energy impacts, but no upgrade measures have been run in ComStock to-date that would qualify for that exemption.