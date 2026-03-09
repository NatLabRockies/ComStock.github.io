---
layout: default
title: FAQ
nav_order: 8
published: true
---

# Frequently Asked Questions
{: .fw-500 }

Expand the sections below for answers to frequently asked questions. If you have additional questions, please email us at [ComStock@nlr.gov](mailto:ComStock@nlr.gov).

## ComStock Essentials
<ul class="jk_accordion">

  <li class="acc" id="faq-simulated-section"><input id="faq-simulated" type="checkbox" /><label for="faq-simulated">Are these load profiles measured or simulated?</label>
    <div class="show">
      <p>The profiles are simulated using the ResStock and ComStock modeling tools, which have been validated and informed by the best available data against an array of empirical datasets. ResStock and ComStock use the EnergyPlus simulation engine. The validation results and uncertainty for quantities of interest are presented in the <a href="https://www.nlr.gov/docs/fy22osti/80889.pdf">End-Use Load Profiles final report.</a></p>
      <p>ResStock generally simulates 550,000 individual building energy models, and ComStock simulates 150,000 building energy models.</p>
    </div>
  </li>

  <li class="acc" id="faq-building-types-section"><input id="faq-building-types" type="checkbox" /><label for="faq-building-types">What building types does ComStock model?</label>
    <div class="show">
      <p>ComStock models 15 commercial building types. Compared to the Commercial Building Energy Consumption Survey (CBECS) 2018 estimation, ComStock datasets account for 63% of both the energy use and floor area of commercial buildings in the United States. The ComStock development team is actively working on adding more building types to the model. See the explanation titled "<a href="{{site.baseurl}}{% link docs/resources/explanations/building_types_not_included.md %}">Building Types Not Included in ComStock</a>" for more detail.</p>
    </div>
  </li>

  <li class="acc" id="faq-year-section"><input id="faq-year" type="checkbox" /><label for="faq-year">What year does the baseline stock represent?</label>
    <div class="show">
      <p>The ComStock and ResStock datasets represent, as closely as possible, the 2018 U.S. commercial and residential building stock characteristics. The energy consumption results depend on the weather data used in the simulations. When modeled with AMY2018 weather, the datasets represent energy use for the year 2018. When TMY3 weather is used, they represent typical or average energy consumption under typical climate conditions.</p>
      <p>Emissions and utility bills in the ComStock and ResStock datasets use input data from a several years, depending on the dataset release. See the <a href="{{site.baseurl}}{% link docs/resources/resources.md %}#references">ComStock reference documentation</a> or <a href="https://docs.nlr.gov/docs/fy25osti/91621.pdf">ResStock reference documentation</a> for more details.</p>
    </div>
  </li>

  <li class="acc" id="faq-credible-section"><input id="faq-credible" type="checkbox" /><label for="faq-credible">Are ComStock and ResStock credible?</label>
    <div class="show">
       <p>Yes. The models underwent extensive calibration as part of the End Use Load Profiles (EULP) project where we compared model load profiles to AMI data from around the country, and updated baseline model schedules, power densities, among other things using various data sources. Reference the <a href="https://www.nlr.gov/docs/fy22osti/80889.pdf">final report</a> for more details. The EULP project concluded in 2021.</p>
      <p>For every baseline update and upgrade measures since EULP, ComStock compares energy consumption and EUI to available data sources, such as CBECS and EIA. These comparisons are available on the OEDI Data Lake for each dataset. You can find links to OEDI in the Published Datasets section of the <a href="{{site.baseurl}}{% link docs/data.md %}">Data page</a>.</p>
      <p>For details about how to determine whether the models are appropriate for a specific analysis, reference the explanation titled "<a href="{{site.baseurl}}{% link docs/resources/explanations/comstock_calibration.md %}">Considerations for ComStock Calibration, Validation, and Uncertainty</a>."</p>
    </div>
  </li>

  <li class="acc" id="faq-dataset-release-section"><input id="faq-dataset-release" type="checkbox" /><label for="faq-dataset-release">Which dataset release should I use? And can I compare upgrades from different dataset releases?</label>
    <div class="show">
      <p>ComStock publishes datasets on a regular basis, and we recommend using the latest release. See the <a href="{{site.baseurl}}{% link docs/data.md %}">Data page</a> for a list of available datasets and access links.</p>
      <p>It is not necessary to compare upgrades across ComStock dataset releases because all datasets include both new upgrade measures and all measures from previous releases, as well as any improvements made to the baseline model.  Information about upgrade measures included in dataset releases can be found on the <a href="{{site.baseurl}}{% link docs/upgrade_measures/upgrade_measures.md %}">Upgrade Measures page</a>. Baseline model improvements are captured in the release change log on our <a href="https://github.com/NatLabRockies/ComStock">public GitHub repository</a>. Note that we re-sample our input characteristic distributions for every release and as a result, the building IDs between releases will not match.</p>
    </div>
  </li>

  <li class="acc" id="faq-weights-section"><input id="faq-weights" type="checkbox" /><label for="faq-weights">What are weights in ComStock and how are they used?</label>
    <div class="show">
      <p>Weights in ComStock represent the number of real buildings in the U.S. building stock that a ComStock model represents. Weights are determined using national floor area by building type from CBECS. Use the weights by multiplying the energy consumption column by the weight for the model. Some results columns already have the weight applied. These have the word “weighted” in the name. See the explanation titled "<a href="{{site.baseurl}}{% link docs/resources/explanations/sampling_and_weighting.md %}">Sampling and Weighting in ComStock</a>" for more information.</p>
    </div>
  </li>

  <li class="acc" id="faq-sample-count-section"><input id="faq-sample-count" type="checkbox" /><label for="faq-sample-count">How many profiles or models should be used for an analysis, and how does the number used affect uncertainty of results?</label>
    <div class="show">
      <p>The minimum sample count required for a given geography in ComStock is a function of the number of commercial buildings present in that area, as well as the quality of available input data for the ComStock model. To ensure statistical robustness in your analysis using ComStock, you may need additional building models depending on the specificity of your segmentation. A good rule of thumb is to include at least six models per segment (e.g., building type, sub-type, size, vintage, or operation hours). For example, if you’re analyzing small office buildings open more than 18 hours a day, make sure you have at least six such models.</p>
      <p>Also, cross-check ComStock’s building representation with external sources (like Google Maps or local datasets) to ensure the dataset reflects your target geography. For more detail, see the explanation titled "<a href="{{site.baseurl}}{% link docs/resources/explanations/sample_size_considerations.md %}">Sample Size Considerations</a>"</p>
      <p>Queries in sparsely populated areas or with filters applied may have relatively few samples available. In these cases, samples from nearby locations can be grouped to increase the sample size. See the tutorial titled "<a href="{{site.baseurl}}{% link docs/resources/tutorials/local_segmentation_study.md %}">Perform an analysis by blending ComStock and local data</a>" for an example of incorporating local floor area estimates to improve representation of ComStock data at specific geographic resolutions.</p>
      <p>Users should estimate standard error for metrics of interest using the standard deviation divided by the square root of the number of samples (i.e., profiles or models). See Section 5.1.3 in the <a href="https://www.nlr.gov/docs/fy22osti/80889.pdf">End-Use Load Profiles methodology report</a> for a discussion on uncertainty calculations.
      </p>
    </div>
  </li>

  <li class="acc" id="faq-citation-section"><input id="faq-citation" type="checkbox" /><label for="faq-citation">How should I cite the datasets?</label>
    <div class="show">
      <p>ComStock and ResStock can be cited according to the suggestions <a href="{{site.baseurl}}{% link docs/citation.md %}">here for ComStock</a> and <a href="https://natlabrockies.github.io/ResStock.github.io/docs/citation_data_attribution.html"> here for ResStock</a>.</p>
    </div>
  </li>

</ul>

## Datasets and Data Access
<ul class="jk_accordion">
  <li class="acc" id="faq-dataset-access-section"><input id="faq-dataset-access" type="checkbox" /><label for="faq-dataset-access">How do I access the dataset?</label>
    <div class="show">
      <p>There are several access platforms available to access ComStock and ResStock datasets. See the <a href="{{site.baseurl}}{% link docs/data.md %}">ComStock Data page</a> and <a href="https://natlabrockies.github.io/ResStock.github.io/docs/data.html">ResStock Data page</a> for more detail about dataset access and links to the public datasets.</p>
    </div>
  </li>

  <li class="acc" id="faq-data-dictionary-section"><input id="faq-data-dictionary" type="checkbox" /><label for="faq-data-dictionary">Are descriptions available for the end-use categories and fields available for filtering?</label>
    <div class="show">
      <p>Descriptions of each of the building characteristics and the end-use categories can be found in the “data_dictionary.tsv” file. Descriptions of the values used in those filters can be found in the “enumeration_dictionary.tsv”. Both files can be downloaded from the OEDI Data Lake and are unique to each dataset release. Use the correct data dictionary for the relevant dataset. They can be opened with Excel or a text editor.</p>
      <p>Links to the OEDI Data Lake for each dataset release can be found on the <a href="{{site.baseurl}}{% link docs/data.md %}">ComStock Data page</a> and <a href="https://natlabrockies.github.io/ResStock.github.io/docs/data.html">ResStock Data page</a>.</p>
    </div>
  </li>

  <li class="acc" id="faq-units-section"><input id="faq-units" type="checkbox" /><label for="faq-units">What are the data units?</label>
    <div class="show">
      <p>ComStock and ResStock data have multiple units. For annual results data downloaded from the Open Energy Data Initiative (OEDI) data lake, units can be found in the "data_dictionary.tsv" file. Some fields will also have the units in the column header at the end of the name (e.g., "out.electricity.total.jan.energy_consumption..<b>kwh</b>"). Timeseries energy consumption data on OEDI are provided in kWh. Natural gas, fuel oil, and propane are output in kwh--this is intentional though unconventional.</p>
      <p>The Data Viewer provides energy data in metric units, visible in the y-axis label. Depending on the scale of energy being shown, the metric prefix will automatically adjust (T for tera, G for giga, M for mega, etc.).</p>
      <p>For Tableau dashboards, use the relevant column headers or the graph axis to see the units.</p>
    </div>
  </li>

  <li class="acc" id="faq-timezone-section"><input id="faq-timezone" type="checkbox" /><label for="faq-timezone">What is the timezone of the timestamps?</label>
    <div class="show">
      <p>The timestamps of all load profiles have been converted to Eastern Standard Time, to prevent issues when aggregating across time zones.</p>
      <p>The underlying modeling was conducted using local standard time for each location, with occupant schedules adjusted for daylight savings as applicable. All EnergyPlus timeseries outputs were converted from local standard time to Eastern Standard Time for publication in the web Data Viewer, Data Viewer exports, timeseries aggregates, and individual timeseries parquet files. In converting from local Standard Time to Eastern Standard Time, if necessary the last few hours of each dataset were moved to the beginning of the timeseries. For example, the first two hours of data from Colorado in Eastern Standard Time (Jan 1, midnight to 2 AM) were originally modeled as the last two hours of the year in Mountain Standard Time (Dec 31, 10 PM to midnight) using the corresponding weather. For leap years (e.g., 2012), the two hours come from Dec 30 rather than Dec 31. Please see <i>How are leap years modeled?</i> FAQ for more info.</p>
    </div>
  </li>


  <li class="acc" id="faq-timestamp-section"><input id="faq-timestamp" type="checkbox" /><label for="faq-timestamp">Does the timestamp represent the beginning, middle, or end of each 15-minute interval?</label>
    <div class="show">
      <p>The timestamp indicates the end of each 15-minute interval. So "12:15" represents the energy use between 12:00 and 12:15.</p>
    </div>
  </li>

  <li class="acc" id="faq-timeseries-agg-weights-section"><input id="faq-timeseries-agg-weights" type="checkbox" /><label for="faq-timeseries-agg-weights">Do the timeseries aggregates have the sample weighting factors applied?</label>
    <div class="show">
      <p>Yes. The aggregates represent the total relevant building stock with all relevant weights applied (e.g., all small office buildings in the state of Colorado), not just the sum of the model results.</p>
    </div>
  </li>

  <li class="acc" id="faq-cec-climate-zones-section"><input id="faq-cec-climate-zones" type="checkbox" /><label for="faq-cec-climate-zones">Are there load profiles available for the 16 California Climate Zones?</label>
    <div class="show">
      <p>ComStock includes commercial buildings in California, and the datasets provide California Energy Commission (CEC) climate zones in the field “in.cec_climate_zone” in the metadata_and_annual_results and metadata_and_annual_results_aggregates files on the OEDI data lake.</p>
      <p>There are a few known issues with California models in ComStock. Please see the "<a href="{{site.baseurl}}{% link docs/resources/explanations/california_known_issues.md %}">California Models Known Issues</a>" explanation for more information.</p>
    </div>
  </li>

  <li class="acc" id="faq-geographic-fields-section"><input id="faq-geographic-fields" type="checkbox" /><label for="faq-geographic-fields">What do the codes used to describe "county_id" and other geographic fields mean?</label>
    <div class="show">
      <p>ComStock and ResStock use the National Historical GIS (NHGIS) GISJOIN standard codes for county, census PUMA, and census tract, which are based on Federal Information Processing System (FIPS) codes. The datasets use the 2010 version of the GISJOIN codes--2020 are not available at this time. For more information about the geospatial fields available in the datasets, see <a href="{{site.baseurl}}{% link docs/resources/explanations/reference_geographic_codes.md %}">this explanation for ComStock</a>, and <a href="https://natlabrockies.github.io/ResStock.github.io/docs/resources/explanations/Geographic_Fields_and_Codes.html">this explanation for ResStock.</a></p>
      <p>In most ComStock and ResStock datasets, county name is available in addition to the GISJOIN county code. For both tools, the column in the metadata_and_annual_results files on OEDI is called "in.county_name."
      </p>
    </div>
  </li>

  <li class="acc" id="faq-measure-docs-section"><input id="faq-measure-docs" type="checkbox" /><label for="faq-measure-docs">Where can I find documentation on what technologies are available in the upgrade measures?</label>
    <div class="show">
      <p>See the <a href="{{site.baseurl}}{% link docs/upgrade_measures/upgrade_measures.md %}">Upgrade Measures</a> page for a complete list of available upgrade measures and packages in ComStock datasets, including a link to their documentation, and in which dataset release the measure was first included.</p>
    </div>
  </li>

  <li class="acc" id="faq-epw-section"><input id="faq-epw" type="checkbox" /><label for="faq-epw">Are weather data files available in EPW format?</label>
    <div class="show">
      <p>Weather data used for the modeling have been provided in .csv format for regression modeling, forecasting, or other analyses. The TMY3 weather files in EnergyPlus input format (EPW) can be downloaded from the <a href="https://data.nlr.gov/submissions/156">NLR Data Catalog</a>, with filenames that correspond to county IDs in the ResStock and ComStock metadata. EPW format weather files for 2018 or other actual meteorological years (AMY) have not been publicly released. These files can be purchased from private sector vendors. See <a href="https://energyplus.net/weather/simulation">here</a> for a list of providers.
      </p>
    </div>
  </li>

  <li class="acc" id="faq-idf-osm-section"><input id="faq-idf-osm" type="checkbox" /><label for="faq-idf-osm">Are the EnergyPlus model input files (.idf) or OpenStudio (.osm) files available?</label>
    <div class="show">
      <p>OpenStudio model input files (.osm) are available in the dataset on the OEDI data lake in the "building_energy_models" directory. Files are named by the building ID ("bldg_id").  The EnergyPlus model input files are not available.</p>
    </div>
  </li>

  <li class="acc" id="faq-api-section"><input id="faq-api" type="checkbox" /><label for="faq-api">Is there an API to access data without downloading locally?</label>
    <div class="show">
      <p>Currently, there is no API. However, we have posted a <a href="https://www.youtube.com/watch?v=qSR1MFpSiro&list=PLmIn8Hncs7bEYCZiHaoPSovoBrRGR-tRS&index=4">tutorial example</a> showing how to load the datasets into cloud services such as Amazon Web Services (AWS) so the data can be queried by analytic tools like Athena.</p>
      <p>Example notebooks and SQL queries are also available on the "<a href="{{site.baseurl}}{% link docs/resources/how_to_guides/example_scripts.md %}">Access ComStock datasets programmatically</a>" page, and more will be added as we develop them. The queries and example notebooks are a good starting point for accessing ResStock programmatically, too.</p>
    </div>
  </li>

  <li class="acc" id="faq-timeseries-for-bldg-id-section"><input id="faq-timeseries-for-bldg-id" type="checkbox" /><label for="faq-timeseries-for-bldg-id">How do I access the timeseries data for a specific building model?</label>
    <div class="show">
      <p>To download a few results by IDs, you can use a manual approach. First use the metadata_and_annual_results to find the IDs you want to access. Then, note the download URL for any easy-to-access ID and edit it to reflect the ID you want.</p>
      <p> For example, right clicking on the first ID under ResStock dataset 2022.1.1, AMY 2018, upgrade 02, and choosing “copy link” provides this URL: <a href="https://oedi-data-lake.s3.amazonaws.com/nrel-pds-building-stock/end-use-load-profiles-for-us-building-stock/2022/resstock_amy2018_release_1.1/timeseries_individual_buildings/by_state/upgrade=2/state=WA/100025-2.parquet">https://oedi-data-lake.s3.amazonaws.com/nrel-pds-building-stock/end-use-load-profiles-for-us-building-stock/2022/resstock_amy2018_release_1.1/timeseries_individual_buildings/by_state/upgrade=2/state=WA/100025-2.parquet</a>. To access ID 813 instead of 100025, change the “100025-2” to “813-2” in the URL, and paste it into a web browser. That will download the data for ID 813.</p>
    </div>
  </li>

  <li class="acc" id="faq-parquet-section"><input id="faq-parquet" type="checkbox" /><label for="faq-parquet">What software can I use to open the .parquet files?</label>
    <div class="show">
      <p>Parquet files can be read using programming languages such as Python, using the pyarrow package. For other options, see <a href="https://arrow.apache.org/docs/index.html">here</a>. There are a few third-party graphical tools for viewing parquet files, but we have not tested them and the third-party support is limited.</p>
      <p>See below for example Python code to convert parquet file to csv.
        <pre><code>
        import pandas as pd
        import os
        folder_path = 'C:/Users/username/Documents/EUSS/Results’
        file_name = '813-2'
        suffix = '.parquet'
        file = pd.read_parquet(os.path.join(folder_path, file_name+suffix))
        new_suffix = '.csv'
        file.to_csv(os.path.join(folder_path, file_name+new_suffix), index=False)
        </code></pre>
      </p>
    </div>
  </li>

  <li class="acc" id="faq-bldg-ids-section"><input id="faq-bldg-ids" type="checkbox" /><label for="faq-bldg-ids">I am trying to match buildings between releases. Why do the building IDs not match between them?</label>
    <div class="show">
      <p>The building IDs and exact building characteristics between releases will not match because we re-sample our input characteristic distributions for every release. However, you can filter the building models using building characteristics to identify similar samples between releases. For instance, using building type, size, location, and wall construction type to identify similar models. The fields with the prefix “in.” show the available model inputs that you can use to do the comparison. You can see a complete list and description of available fields in the “data_dictionary.tsv” file on the OEDI Data Lake. Links to the datasets on OEDI are in the "Published Datasets" section of the <a href="{{site.baseurl}}{% link docs/data.md %}">ComStock Data page</a> and <a href="https://natlabrockies.github.io/ResStock.github.io/docs/data.html">ResStock data page</a>.</p>
    </div>
  </li>

  <li class="acc" id="faq-aws-access-section"><input id="faq-aws-access" type="checkbox" /><label for="faq-aws-access">How can I use AWS Athena to query ComStock and ResStock datasets?</label>
    <div class="show">
      <p>Sample queries showing how to create prompts for different ComStock questions are available in the “<a href="{{site.baseurl}}{% link docs/resources/how_to_guides/aws_athena_queries.md %}">AWS Athena Queries</a>” how-to guide. These examples can be easily adapted to ResStock datasets.</p>
      <p>A <a href="https://www.youtube.com/watch?v=qSR1MFpSiro">training video</a> on how to load ComStock and ResStock data into AWS Athena is also available.</p>
    </div>
  </li>

  <li class="acc" id="faq-agg-timeseries-section"><input id="faq-agg-timeseries" type="checkbox" /><label for="faq-agg-timeseries">What pre-aggregated timeseries data are available on the Open Energy Data Initiative (OEDI) data lake?</label>
    <div class="show">
      <p>Pre-aggregated timeseries data are available in the "timeseries_aggregates" directory on OEDI and are provided for multiple geographic levels:</p>
      <ul>
        <li>ASHRAE/IECC climate zone</li>
        <li>Building America climate zone</li>
        <li>ISO/RTO region</li>
        <li>State</li>
        <li>PUMA</li>
        <li>County</li>
      </ul>
      <p>The data are further disaggregated into separate files by building type.</p>
      <p>These files provide aggregate, weighted, subhourly end-use energy data by fuel type for all ComStock models that meet the specified criteria. For example, the file <b>up0-co-fullservicerestaurant</b> contains aggregate end use load profiles for all full-service restaurants in Colorado.</p>
      <p>Each file also includes "models_used" and "floor_area" columns, which indicate the number of ComStock models represented and the associated weighted floor area, respectively.</p>

    </div>
  </li>
  
</ul>

## Data Viewer
<ul class="jk_accordion">

  <li class="acc" id="faq-data-viewer-section"><input id="faq-data-viewer" type="checkbox" /><label for="faq-data-viewer">What is the Data Viewer?</label>
    <div class="show">
      <p>The Data Viewer is a web-based visualization platform that allows users to easily filter, aggregate, view, and download ComStock end-use energy data in a web browser.</p>
      <p>Links to Data Viewer visualizations for each dataset release are on the <a href="{{site.baseurl}}{% link docs/data.md %}">Data page</a>.</p>
      <p>For Data Viewer trainings, visit the <a href="https://www.youtube.com/playlist?list=PLmIn8Hncs7bEYCZiHaoPSovoBrRGR-tRS">NLR’s Building Stock Analysis YouTube channel</a>.</p>
    </div>
  </li>

  
  <li class="acc" id="faq-sum-avg-def-section"><input id="faq-sum-avg-def" type="checkbox" /><label for="faq-sum-avg-def">In the Data Viewer, what does "sum" or "average" mean?</label>
    <div class="show">
      <p>The "sum" aggregation is the total energy consumption for all buildings that meet the filter criteria across all the occurrences of the given time step within the selected month(s). For example, in a day timeseries range for a specific state for the month of July, the 7-7:15 AM hour time step shows the sum of all energy consumption statewide between 7-7:15 AM in July, from buildings that meet the filter criteria. The "sum" view has fewer uses than the "average" view. The "average" aggregation is the total energy consumption for all buildings that meet the filter criteria, averaged across all the occurrences of the given time step within the selected month(s).</p>
      <p>For example, in a day timeseries range for a specific state for the month of July, the 7-7:15 AM hour time step shows the average statewide energy consumption between 7-7:15 AM in July, from buildings that meet the filter criteria. The "average" aggregation provides a view of the average day of total energy consumption in the state. This is the more logical view for most use cases. Note that while each time step within a day or a year has the same number of occurrences within each dataset, each time step for a week does not - some days of the week occur more times than others in each year or month range (except for February).
      </p>
    </div>
  </li>

  <li class="acc" id="faq-peak-day-def-section"><input id="faq-peak-day-def" type="checkbox" /><label for="faq-peak-day-def">In the Data Viewer, how are the peak day and min peak day defined?</label>
    <div class="show">
      <p>The peak day is the day with the highest single-hour (peak) energy consumption within the selected months.</p>
      <p>The min peak day is the day with the lowest single-hour energy consumption within the selected months.</p>
    </div>
  </li>

  <li class="acc" id="faq-slow-load-section"><input id="faq-slow-load" type="checkbox" /><label for="faq-slow-load">Why is the time series data sometimes slow to load after I click the update button?</label>
    <div class="show">
      <p>We query data in real time to produce the time series graphs you see on the webpage, and this can involve scanning terabytes (TB) of data. Running a baseline-only query for California, Texas, New York, or Illinois takes around a minute, while running a query for a state like Colorado or Massachusetts takes about 10-20 seconds. However, if the graphs have previously been generated we have the data cached and can typically load the data in a few seconds. That's why the load time varies.</p>
    </div>
  </li>

  <li class="acc" id="faq-explore-timeseries-section"><input id="faq-explore-timeseries" type="checkbox" /><label for="faq-explore-timeseries">Why can’t I click on “Explore Timeseries”?</label>
    <div class="show">
      <p>The “Explore Timeseries” option is available once a specific geography (e.g. state or PUMA region) is selected.</p>
    </div>
  </li>

  <li class="acc" id="faq-end-use-view-section"><input id="faq-end-use-view" type="checkbox" /><label for="faq-end-use-view">How do I see a profile for just one, or just a few, end uses?</label>
    <div class="show">
      <p>Clicking on the end uses in the legend will highlight the end use in the visualization.</p>
    </div>
  </li>

  <li class="acc" id="faq-agg-locations-section"><input id="faq-agg-locations" type="checkbox" /><label for="faq-agg-locations">Can I aggregate over multiple locations?</label>
    <div class="show">
      <p>The viewer allows aggregations of up to six locations (states or PUMAs, depending on the dataset). When viewing a single location, choose the “+ More Locations” option, add up to five additional locations, and choose “Update Search”.</p>
      <p>Additionally, sums of more than six locations can be created manually by downloading sums of up to six locations and summing further on your local computer.</p>
      <p>TMY3 weather is not aligned between locations. This does not affect our recommendations for working with annual data. However, if your application requires timeseries data and therefore would benefit from aligned weather, we recommend either using an AMY dataset, or filtering by weather station and summing only within a single weather station’s PUMAs.</p>
    </div>
  </li>

  <li class="acc" id="faq-filter-characteristics-section"><input id="faq-filter-characteristics" type="checkbox" /><label for="faq-filter-characteristics">How can I filter the data based on building characteristics?</label>
    <div class="show">
      <p>The "+ Filter" button enables users to filter the data by characteristics, such as vintage, floor area, and building type. This feature also enables aggregations of locations, including by PUMA and county.</p>
      <p>See our <a href="https://www.youtube.com/watch?v=1hzT7MGsAC8&list=PLmIn8Hncs7bEYCZiHaoPSovoBrRGR-tRS&index=14">YouTube training video</a> on the Data Viewer, around 3:50, to learn how to add multiple filters.</p>
    </div>
  </li>

  <li class="acc" id="faq-view-characteristics-section"><input id="faq-view-characteristics" type="checkbox" /><label for="faq-view-characteristics">How can I see the building characteristics associated with an aggregate load profile from the data viewer?</label>
    <div class="show">
      <p>The building characteristics are available on the Open Energy Data Initiative (OEDI) data lake. Visit the <a href="{{site.baseurl}}{% link docs/data.md %}">Data page</a> for links to the OEDI pages for each dataset. In the "metadata_and_annual_results_aggregate" directory on OEDI, navigate to the national file: metadata_and_annual_results_aggregates > national > full > csv > baseline_agg.csv.gz. Download the file, unzip it and open in Microsoft Excel. Use the filters applied on the Data Viewer to filter the spreadsheet.</p>
      <p>Note that the national file is an “aggregate,” meaning that the data in the file is consolidated by merging duplicate building models within a geography (in this case state), so each building ID appears only once with a combined weight. Columns that cannot be meaningfully aggregated from the tract level—such as Cambium grid region and CEJST designation—are excluded from the resulting low-resolution, “aggregate” files. For more information about the updated OEDI file structure as a result of the new sampling method, please see the "<a href="{{site.baseurl}}{% link docs/resources/explanations/new_sampling_method.md %}">New ComStock Sampling Method</a>" explanation.</p>
    </div>
  </li>

</ul>

## Analysis
<ul class="jk_accordion">
  <li class="acc" id="faq-run-models-section"><input id="faq-run-models" type="checkbox" /><label for="faq-run-models">Can I run ComStock or ResStock myself?</label>
    <div class="show">
      <p>The code required to run ComStock and ResStock is available on the <a href="https://github.com/NatLabRockies/ComStock">ComStock</a> and <a href="https://github.com/NatLabRockies/ResStock">ResStock</a> public GitHub repositories. Other related code repositories are provided on the "For Developers" page for <a href="{{site.baseurl}}{% link docs/for_developers/for_developers.md %}">ComStock</a> and <a href="https://natlabrockies.github.io/ResStock.github.io/docs/developers.html">ResStock.</a></p>
      <p>While these resources are available, ComStock and ResStock are complex modeling tools and there is no documentation for running the model other than what exists in the codebase, and we are not able to support running the models at this time. We generally do not recommend running the model unless you have a deep understanding of the methodology and objectives. Please email us at <a href="mailto:ComStock@nlr.gov">ComStock@nlr.gov</a> or <a href="mailto:ResStock@nlr.gov">ResStock@nlr.gov</a> if you have suggestions for improvements or specific needs.</p>
    </div>
  </li>

  <li class="acc" id="faq-combine-measures-section"><input id="faq-combine-measures" type="checkbox" /><label for="faq-combine-measures">I am interested in an upgrade measure combination that is not currently available as an upgrade package in the public datasets. Can I combine results from the individual measures?</label>
    <div class="show">
      <p>Our general guidance is to <b>NOT</b> combine measure results. There are interactions between most upgrade measures that affect the amount of savings and make results of multiple measures together misleading.</p>
      <p>For an explanation and examples on this topic, see the linked <a href="{{site.baseurl}}{% link docs/resources/explanations/combining_measure_results.md %}">ComStock</a> and <a href="https://natlabrockies.github.io/ResStock.github.io/docs/resources/explanations/Individual_Measures_Not_Combined.html">ResStock</a> resources.</p>
      <p>If you have questions about combining specific measures, please email us at <a href="mailto:ComStock@nlr.gov">ComStock@nlr.gov</a> or <a href="mailto:ResStock@nlr.gov">ResStock@nlr.gov</a>.</p>
    </div>
  </li>

</ul>

## Modeling Methods, Assumptions and Documentation
<ul class="jk_accordion">

  <li class="acc" id="faq-ref-section"><input id="faq-ref" type="checkbox" /><label for="faq-ref">Where can I find information about ComStock modeling methodology and assumptions?</label>
    <div class="show">
      <p>ComStock reference documentation is available in the <a href="https://nrel.github.io/ComStock.github.io/docs/resources/resources.html#references">References section</a> of the <a href="{{site.baseurl}}{% link docs/resources/resources.md %}">Resources page</a>. We publish an updated version with every dataset release that includes changes to the ComStock model.</p>
    </div>
  </li>

  <li class="acc" id="faq-costs-section"><input id="faq-costs" type="checkbox" /><label for="faq-costs">Are costs modeled?</label>
    <div class="show">
      <p>As of the 2024 Release 1, ComStock includes utility cost data using current electricity rates from the Utility Rate Database (URDB), matched by utility ID, demand, and usage. Annual utility bills are reported as the min, max, mean, and median of all applicable rates for each model. Natural gas, propane, and fuel oil prices are based on volumetric pricing due to limited rate data, using EIA price and heat content data. See the <a href="{{site.baseurl}}{% link docs/resources/resources.md %}#references">ComStock reference documentation</a> for details.</p>
      <p>ComStock does not calculate first costs (i.e., upgrade or measure costs). However, many ComStock output variables can be used to estimate first cost. See the explanation titled "<a href="{{site.baseurl}}{% link docs/resources/explanations/costing_analysis.md %}">Using ComStock to Analyze Cost</a>" for more detail about cost assessments, including a discussion of output variables that can be used to estimate first costs.</p>
    </div>
  </li>

  <li class="acc" id="faq-pv-section"><input id="faq-pv" type="checkbox" /><label for="faq-pv">Does ComStock model rooftop solar PV?</label>
    <div class="show">
      <p>ComStock does not currently model rooftop solar PV in the baseline. However, starting with ComStock 2025 Release 1, upgrade measures are available that model rooftop solar PV additions to buildings. See the <a href="{{site.baseurl}}{% link docs/upgrade_measures/upgrade_measures.md %}"> Upgrade Measures</a> page for more information.</p>
    </div>
  </li>

  <li class="acc" id="faq-ev-section"><input id="faq-ev" type="checkbox" /><label for="faq-ev">Are there electric vehicle (EV) charging profiles in the dataset?</label>
    <div class="show">
      <p>No, ComStock does not currently model EV charging in the dataset. For modeling aggregate EV load profiles for a city or state, we suggest using <a href="https://afdc.energy.gov/evi-pro-lite/load-profile">EVI-Pro Lite</a>. Measured charging profile data for individual homes can be found in the <a href="https://neea.org/data/nw-end-use-load-research-project/energy-metering-study-data">NEEA HEMS data</a> and <a href="https://www.pecanstreet.org/dataport/">Pecan Street Dataport</a>. Email us at <a href="mailto:ComStock@nlr.gov">ComStock@nlr.gov</a> if you have suggestions for other EV charging data sources.</p>
    </div>
  </li>

  <li class="acc" id="faq-water-heater-section"><input id="faq-water-heater" type="checkbox" /><label for="faq-water-heater">Are there water heater upgrade measures available?</label>
    <div class="show">
      <p>We have not published a service water heating measure due to current water draw profiles in our baseline models. The energy consumed by heat pump water heaters (HPWHs), especially, is sensitive to how quickly the water in the tank is consumed. More specifically, how to design and size a HPWH system greatly relies on realistic water draw profiles to correctly capture when the heat pump heating and, especially, backup heating elements are triggered.</p>
      <p>If you are aware of water draw profile data, please let email us at <a href="mailto:ComStock@nlr.gov">ComStock@nlr.gov</a>! We are in search of 15-min to hourly water draw profiles for commercial buildings of various types and square footage.</p>
    </div>
  </li>

  <li class="acc" id="faq-data-centers-section"><input id="faq-data-centers" type="checkbox" /><label for="faq-data-centers">Does ComStock model data centers?</label>
    <div class="show">
      <p>We do not currently model data centers as a building type in ComStock. Some large office buildings include data center loads, but these do not capture the characteristics, performance, HVAC system, etc. of standalone data centers and we do not recommend extrapolating results from these models.</p>
    </div>
  </li>

  <li class="acc" id="faq-leap-year-section"><input id="faq-leap-year" type="checkbox" /><label for="faq-leap-year">How are leap years modeled?</label>
    <div class="show">
      <p>ComStock public dataset releases include AMY2012 weather, which is a leap year, and AMY2018 and TMY3 weather years, neither of which are leap years.</p>
      <p>The default simulation runs for one year, covering 8,760 hours from January 1 to December 31. For ComStock dataset releases using AMY2012 weather, the simulation period is adjusted to maintain exactly 8,760 hours: it starts on January 1, includes February 29, and ends on December 30 instead of December 31.</p>
      <p>For more detail, please see the <a href="{{site.baseurl}}{% link docs/resources/resources.md %}#references">ComStock reference documentation</a>.</p>
    </div>
  </li>

  <li class="acc" id="faq-multifamily-section"><input id="faq-multifamily" type="checkbox" /><label for="faq-multifamily">How are multifamily common areas modeled?</label>
    <div class="show">
      <p>The residential housing units in multifamily buildings are modeled in ResStock and are not in ComStock.  All energy consumption specific to the housing unit is included in the modeled results, such as lighting, appliances, window air conditioners, and HVAC and water heaters that serve a single housing unit.</p>
      <p>HVAC and water heating that serves multiple housing units are also included, with energy consumption allocated to the unit served and with adjustment factors applied to account for the energy consumption differences of shared equipment. These adjustment factors are set by OpenStudio-HPXML and from ANSI/RESNET 301.</p>
      <p>Electric vehicle charging energy consumption from common areas is also included in ResStock results, allocated directly to the unit that is associated with each electric vehicle.</p>
      <p>All other energy that provides services to common areas in multifamily buildings is not included in either ResStock or ComStock. Examples of this would include common area lighting, common laundry facilities, pools, and hot tubs, and elevators.</p>
    </div>
  </li>

  <li class="acc" id="faq-walls-section"><input id="faq-walls" type="checkbox" /><label for="faq-walls">How are wall cavity R-values determined?</label>
    <div class="show">
      <p>In ComStock models, wall R-values are based on building energy code requirements by climate zone and construction type (mass, metal building, steel-framed, wood-framed) and account for both interior and exterior air films.</p>
      <p>See the <a href="{{site.baseurl}}{% link docs/resources/resources.md %}#references">ComStock reference documentation</a> for more information.</p>
    </div>
  </li>

  <li class="acc" id="faq-census-section"><input id="faq-census" type="checkbox" /><label for="faq-census">What year of U.S. Census geography (e.g., counties, PUMAs) do ComStock and ResStock use?</label>
    <div class="show">
      <p>ComStock and ResStock datasets reflect the 2010 National Historical GIS (NHGIS) GISJOIN standard codes for counties, PUMAs, and Census Tracts. Some model input data sources use 2020 Census geographies, and these are translated to 2010 before being integrated into the ComStock and ResStock workflows. However, 2020 geographic codes are not currently available in the ComStock and ResStock datasets.</p>
      <p>For more information about geographic fields and codes used in the models, please refer to the <a href="{{site.baseurl}}{% link docs/resources/explanations/reference_geographic_codes.md %}">ComStock</a> and <a href="https://natlabrockies.github.io/ResStock.github.io/docs/resources/explanations/Geographic_Fields_and_Codes.html">ResStock</a> user resources.
      </p>
    </div>
  </li>

</ul>
