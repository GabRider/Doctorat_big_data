# dataset link

For hourly data on single levels from 1979 to present: [ERA5 hourly data on single levels](https://cds.climate.copernicus.eu/cdsapp#!/dataset/reanalysis-era5-single-levels?tab=form)
For hourly data on pressure levels from 1979 to present: [ERA5 hourly data on pressure levels](https://cds.climate.copernicus.eu/cdsapp#!/dataset/reanalysis-era5-pressure-levels?tab=form)
# other links
[Paper Graphcast](https://www.science.org/doi/epdf/10.1126/science.adi2336)
[ECMWF Forecast model](https://confluence.ecmwf.int/display/FUG/Section+5.3.1+M-climate,+the+ENS+Model+Climate)
[CDSAPI python credential](https://cds.climate.copernicus.eu/how-to-api)
[ERA5 request creator](https://cds.climate.copernicus.eu/datasets/reanalysis-era5-single-levels?tab=download#manage-licences)

## use case
The use of weather forecasting is essential in certain fields. For example, in aviation, based on forecasts, airplanes may either take off or adopt an alternative flight plan. It is also crucial for predicting the production of raw materials and their impact on prices. In 2023, Google DeepMind released a new open-source weather prediction model named [Graphcast](https://github.com/google-deepmind/graphcast), which outperforms the European Centre for Medium-Range Weather Forecasts (ECMWF) in providing ten-day forecasts in less than a minute. Additionally, the application of quantum computing in this field is highly beneficial, due to its capacity for handling big data and performing complex computations, such as simulations.
## Task 
- Improve the Machine learning part of the Graphcast with the quantum computing
- Adapt Forecasting simulations for running on quantum computing
## Data
All weather-related data is stored in unconventional ways to manage the challenges associated with handling large volumes of data. Tools like satellites in Earth’s orbit generate vast datasets, which are often difficult to transmit efficiently. Once received, these datasets require robust computational resources to process, especially when stored in traditional file formats. To tackle this, innovative file formats such as Zarr, GRIB2, and NetCDF have been developed. Zarr is designed for the efficient storage and retrieval of array-like data in a chunked, compressed format, allowing for a hierarchical, tree-like data structure that facilitates faster read and write operations. GRIB2, used widely in meteorology, optimizes data compression to handle large datasets efficiently. NetCDF supports multi-dimensional data storage and is highly favored in the scientific community for its flexibility and compatibility with numerous data models. Together, these formats provide powerful tools for managing complex and voluminous atmospheric data.
### NETCDF
NetCDF (Network Common Data Form) is a format used to store and share scientific data. It's designed to be flexible and efficient, ideal for handling large sets of multidimensional data. NetCDF files are self-describing and portable, meaning they can be accessed across different computer systems. The format supports data descriptions through its use of dimensions, variables, and attributes. There are two main versions: NetCDF-3, which is simpler and widely used, and NetCDF-4, which adds features like data compression and support for more complex data structures. Tools like Python’s xarray and netCDF4 libraries make it easy to work with NetCDF files, allowing for effective data analysis and visualization.
To visualize/retrieve information from NETCDF file, it is possible to use [QGIS](https://qgis.org/download/)
## Variables Description related to atmospheric/surface
size: 721(latitude)x14440(longitude)


| Variable     | Description                                       | Units         | Long Name         |
|--------------|---------------------------------------------------|---------------|------------------------------|
| `number`     | Ensemble member numerical ID                      | 1             | `ensemble member numerical id` |
| `valid_time` | Time for which the data is valid                  |               | `time`                       |
| `latitude`   | Latitude coordinate                               | degrees north | `latitude`                   |
| `longitude`  | Longitude coordinate                              | degrees east  | `longitude`                  |
| `t2m`        | Temperature at 2 meters above the surface         | K             | `2 metre temperature`        |
| `tp`         | Total accumulated precipitation                   | m             | `Total precipitation`        |
| `sp`         | Atmospheric pressure at the surface               | Pa            | `Surface pressure`           |
| `u10`        | Eastward wind component at 10 meters above ground | m/s           | `10 metre U wind component`  |
| `v10`        | Northward wind component at 10 meters above ground| m/s           | `10 metre V wind component`  |
## Variables Description related to pressures levels 
size: 37(number of pressures level)x721(latitude)x14440(longitude)
| Variable      | Dimensions                                      | Type           | Description                 | Units          |  Standard Name      |
|---------------|-------------------------------------------------|----------------|-----------------------------|----------------|----------------------------|
| `number`      | ()                                              | int64          | Ensemble member numerical id| 1              | `realization`              |
| `valid_time`  | (valid_time)                                    | datetime64[ns] | Time                        | N/A            | `time`                     |
| `pressure_level` | (pressure_level)                            | float64        | Pressure                    | hPa            | `air_pressure`             |
| `latitude`    | (latitude)                                      | float64        | Latitude                    | degrees_north  | `latitude`                 |
| `longitude`   | (longitude)                                     | float64        | Longitude                   | degrees_east   | `longitude`                |
| `t`           | (valid_time, pressure_level, latitude, longitude)| float32       | Temperature                 | K              | `air_temperature`          |
| `u`           | (valid_time, pressure_level, latitude, longitude)| float32       | U component of wind         | m s**-1        | `eastward_wind`            |
| `v`           | (valid_time, pressure_level, latitude, longitude)| float32       | V component of wind         | m s**-1        | `northward_wind`           |
| `z`           | (valid_time, pressure_level, latitude, longitude)| float32       | Geopotential                | m\*\*2 s**-2     | `geopotential`             |
| `q`           | (valid_time, pressure_level, latitude, longitude)| float32       | Specific humidity           | kg kg**-1      | `specific_humidity`        |

### Understand 
- **Geopotential**:
This parameter plays an important role in synoptic meteorology (analysis of w:eather patterns). Charts of geopotential height plotted at constant pressure levels (e.g., 300, 500 or 850 hPa) can be used to identify weather systems such as cyclones, anticyclones, troughs and ridges. At the surface of the Earth, this parameter shows the variations in geopotential height of the surface, and is often referred to as the orography.
