# Project Proposal - CUAHSI Virtual University
~ Pritam Das

# Data Description
The dataset associated with this repository corresponds to Surface Areas estimated using the Tiered Multi-Sensor - Optical/Sar (TMS-OS) algorithm. The algorithm uses both optically (Landsat 8, 9 and Sentinel 2) and SAR (Sentinel 1) derived surface area estimates to self-correct the water surface area time-series. The idea is to use the strengths of either techonologies to address deficiencies of the other dataset. For instance, optically obtained surface area estimates can be comparatively more accurate during clear days, since SAR estimates tend to udnerestimate surface areas around the boundaries. However, optically derived estimates can get contaminated by clouds and can be unreliable during challenging conditions (atmospheric haze, aglae bloom, high sediment flux), which don't affect the SAR estimates as much (or at all). This complimentary nature of these technologies is taken advantage of in the TMS-OS algorithm. A more detailed description of the almorithm can be found [here, at the recently published paper in the EMS Journal](https://doi.org/10.1016/j.envsoft.2022.105533).

The data used in this project correspond to 36 reservoirs in the Mekong basin, from January 2019 to September 2022

# Files
1. `Reservoir-Locations.csv`
    - Metadata of the reservoirs. Contains X, Y in Decimal Degrees, Year of construction, Country, Reservoir Name and corresponding file name prefix (`FNAME`).
    - The `FNAME` column can be used to identify the prefix of files corresponding to the respective reservoir.
2. `surface-areas/sar/*.csv`
    - Surface area estimated using Sentinel-1 images for the reservoirs.
    - Contains `time` and `sarea` that correspond to the acquisition time of the SAR image and the corresponding estimated area.
3. `surface-areas/tmsos/*.csv`
    - Surface area estimates after applying the TMS-OS algorithm. Please note that these data files also contain redundant/intermediary columns that can be ignored safely.
    - Metadata:
        - `date`: Date of observation (corresponds to any time a optical sensors makes an observation).
        - `water_area_uncorrected`: Raw water area estimated by optical sensors.
        - `non_water_area`: Raw non-water area estiamted by optical sensors.
        - `cloud_area`: Raw cloud cover over the reservoir.
        - `unfiltered_area`: Surface water area obtained after applying the [Zhao and Gao, (2018)](https://doi.org/10.1029/2018GL078343) algorithm to address cloudy-pixels.
        - `cloud_percent`: Cloud cover as a percentage of total area of reservoir including a buffer of 800m around the nominal reservoir boundary.
        - `QUALITY_DESCRIPTION`: Flag denoting if area was itnerpolated
            - 0: Good, _not interpolated_ either due to missing data or high clouds
            - 1: Poor, interpolated either due to _high clouds_
            - 2: Poor, interpolated either due to _missing data_
        - `sar`: Denotes which optical sensor took the observation.
            - s2: Sentinel-2
            - l8: Landsat-8
            - l9: Landsat-9
        - `filtered_area`: (_intermediate_) Filtered areas (please see description [here](https://doi.org/10.1016/j.envsoft.2022.105533))
        - `corrected_areas_1`: (_intermediate_)
        - `corrected_trend_1`: (_intermediate_)
        - `sar_trend`: (_intermediate_) Estimated trend as observed in SAR
        - `days_passed`: No. of days that passed since last observation
        - `area`: **Area estimated by TMS-OS** after applying all the filtering and filling.

# Questions to be answered using the data
- What are the characteristics of variability of surface areas of the reservoirs and its change (∆Area)? 
    - Is there a spatial/temporal pattern?
    - Does the variability relate to the country of the reservoir? This may indicate that different countries operate their reservoirs differently.
- How well does the TMS-OS algorithm perform in estimating surface areas?
    - How do the raw optical & SAR, cloud corrected optical (after applying [Zhao and Gao, (2018)](https://doi.org/10.1029/2018GL078343)), and TMS-OS derived surface area estimates compare? - with each other, and with other sources of surface area/water level of reservoir (like [G-REALM](https://ipad.fas.usda.gov/cropexplorer/global_reservoir/)).
- To explore other relationships that might exist between directly connected reservoirs - is there a relationship between surface areas of upstream and their corresponding downstream reservoirs?

# Other datasets that can be used in conjunction to answer the questions above
- [GRAND Database](globaldamwatch.org/grand/): Contains additional metadata for most of the reservoirs that can be used for annotations/further analysis.
- [ERA-5 Reanalysis](https://cds.climate.copernicus.eu/#!/search?text=ERA5&type=dataset): Additional meteorological variables that can be used to support visualizations - for example, by providing account of any heavy precipitation events and its effect on how the surface area of any downstream reservoir changes.
- [G-REALM](https://ipad.fas.usda.gov/cropexplorer/global_reservoir/): Altimetry data from multiple altimetry satellite missions.