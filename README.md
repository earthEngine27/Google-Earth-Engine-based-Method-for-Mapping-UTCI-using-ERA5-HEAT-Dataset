A set of modular Google Earth Engine-based workflow that analyzes changes in UTCI using the ERA5 Heat dataset. The code was initially developed for the Philippines, but the overall workflow is easily scalable and transferable to other countries using the US Department of State (USDOS) Large-Scale International Boundary. All these datasets can be accessed directly from GEE.

**MULTITEMPORAL UTCI Analysis**

**I. Description**

Rapidly increasing heat stress exposure is a growing urban issue worldwide. Rapid demographic growth in conjunction with global warming trends further exacerbates heat stress concerns, especially in low-income countries, such as the Philippines. Given the pressing challenges with heat stress exposure, combined with the converging demographic and climate change dilemma, warrant heat stress risk analysis at resolutions relevant for local planning.

Traditional heat stress analysis relies solely on ambient air temperature, but over-reliance on temperature alone dismisses other important mechanisms of heat transfer and thermoregulation. Both of which collectively influence heat stress. Using ERA5-Heat datasets wrangled and analyzed via Google Earth Engine, this analysis extracts and maps the temporal trend of UTCI in the Philippines across 10-year intervals from 1940 to 2020.

**II. Methodology and Processing**

The methodology modules are composed of the following procedures:

**Spatial Boundary Filtering:** The analysis focuses on the Philippines as the Area of Interest, using the United States Department of State's Large Scale International Boundaries (LSIB) dataset, which is directly available in GEE. Users can further apply and scale the code to other countries recognized and listed in the LSIB database.

**Temporal Filtering:** The analysis strictly filters the whole dataset to the dry season temporal window of the Philippines (i.e., 01 March to 31 May), and then further cascades this filtering rule for every decade year.

**Threshold Masking:** Days exceeding a daily mean UTCI threshold of 32°C (the baseline where the human body begins experiencing strong heat stress) are isolated. Further statistics, such as the number of summer days surpassing the 32°C is isolated and tracked to determine how UTCI has changed between 1940 and 2020.

**Zonal Statistics:** A spatial reduction is applied on a nationwide scale to compute the nationwide average number of high-heat dry season days per decade at the native resolution of ERA5 data (~28km).

**Dynamic Visualization:** The script initially computes the mean Daily maximum UTCI at the native resolution of ERA5 data for each summer per decade, converting Kelvin to Celsius scale, and lastly, collectively overlays decadal UTCI grids onto an interactive map for instant comparison and hotspot identification. 

**III. Analytics Feature and Key Insights**

**Temporal Trend:** By applying a spatial reduction method directly to the ERA5 Heat dataset, the script dynamically plots a decadal trendline of UTCI since 1940. This visualization tracks how the average number of summer days surpassing the 32°C UTCI baseline has changed.

**Spatial Visualization:** Beyond the descriptive statistical test, this coding workflow also includes a visualization aspect that individually maps and stacks nine historical UTCI maps to demonstrate areas of heat stress intensification.

<img width="1240" height="612" alt="UTCI_Trend_1940-2020" src="https://github.com/user-attachments/assets/a3d7adb2-5108-449d-a451-756025818faa" />
<img width="1920" height="1080" alt="UTCI Comparison" src="https://github.com/user-attachments/assets/ff7b65ea-6fcf-4d4b-a770-430d3634e4b1" />


