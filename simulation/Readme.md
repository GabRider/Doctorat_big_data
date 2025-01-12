# dataset link

- For hourly data on single levels from 1979 to present: [ERA5 hourly data on single levels](https://cds.climate.copernicus.eu/cdsapp#!/dataset/reanalysis-era5-single-levels?tab=form)
- For hourly data on pressure levels from 1979 to present: [ERA5 hourly data on pressure levels](https://cds.climate.copernicus.eu/cdsapp#!/dataset/reanalysis-era5-pressure-levels?tab=form)
# other links
- [Paper Graphcast](https://www.science.org/doi/epdf/10.1126/science.adi2336)
- [ECMWF Forecast model](https://confluence.ecmwf.int/display/FUG/Section+5.3.1+M-climate,+the+ENS+Model+Climate)
- [CDSAPI python credential](https://cds.climate.copernicus.eu/how-to-api)
- [ERA5 request creator](https://cds.climate.copernicus.eu/datasets/reanalysis-era5-single-levels?tab=download#manage-licences)

## Use Case
The use of weather forecasting is essential in certain fields. For example, in aviation, based on forecasts, airplanes may either take off or adopt an alternative flight plan. It is also crucial for predicting the production of raw materials and their impact on prices. In 2023, Google DeepMind released a new open-source weather prediction model named [Graphcast](https://github.com/google-deepmind/graphcast), which outperforms the European Centre for Medium-Range Weather Forecasts (ECMWF) in providing ten-day forecasts in less than a minute. Additionally, the application of quantum computing in this field is highly beneficial, due to its capacity for handling big data and performing complex computations, such as simulations.
## Task 
- Improve the Machine learning part of the Graphcast with the quantum computing
- Adapt Forecasting simulations for running on quantum computing
## Data
All weather-related data is stored in unconventional ways to manage the challenges associated with handling large volumes of data. Tools like satellites in Earth’s orbit generate vast datasets, which are often difficult to transmit efficiently. Once received, these datasets require robust computational resources to process, especially when stored in traditional file formats. To tackle this, innovative file formats such as Zarr, GRIB2, and NetCDF have been developed. Zarr is designed for the efficient storage and retrieval of array-like data in a chunked, compressed format, allowing for a hierarchical, tree-like data structure that facilitates faster read and write operations. GRIB2, used widely in meteorology, optimizes data compression to handle large datasets efficiently. NetCDF supports multi-dimensional data storage and is highly favored in the scientific community for its flexibility and compatibility with numerous data models. Together, these formats provide powerful tools for managing complex and voluminous atmospheric data.
## Data Provider

### Copernicus
Copernicus is a program run by the European Union to monitor the Earth's environment. It collects data from satellites (like the Sentinel satellites) and ground sensors to provide information on topics like climate change, land use, oceans, and emergencies. The data is free and available for everyone to use, making it helpful for scientists, governments, and businesses working on environmental issues, agriculture, and disaster response.

### National Oceanic and Atmospheric Administration (NOAA)
NOAA is a U.S. government agency that provides data about weather, oceans, and the environment. It runs satellites and observation systems to monitor storms, marine life, and climate changes. NOAA's data is used for weather forecasts, tracking hurricanes, and studying climate trends. The agency makes most of its information freely available, supporting scientists, emergency responders, and industries like farming and shipping.

## File Format 

### NETCDF
NetCDF (Network Common Data Form) is a format used to store and share scientific data. It's designed to be flexible and efficient, ideal for handling large sets of multidimensional data. NetCDF files are self-describing and portable, meaning they can be accessed across different computer systems. The format supports data descriptions through its use of dimensions, variables, and attributes. There are two main versions: NetCDF-3, which is simpler and widely used, and NetCDF-4, which adds features like data compression and support for more complex data structures. Tools like Python’s xarray and netCDF4 libraries make it easy to work with NetCDF files, allowing for effective data analysis and visualization.
To visualize/retrieve information from NETCDF file, it is possible to use [QGIS](https://qgis.org/download/)

### GRIB
GRIB (GRIdded Binary) is a file format commonly used for storing and sharing meteorological and geospatial data. It is designed to handle large datasets, particularly gridded data like weather forecasts, climate simulations, or satellite observations. GRIB files are highly compressed and portable, enabling fast storage and transmission. The format uses a structured approach, organizing data into sections with metadata that describes the grid, variable type, and units. There are two versions: GRIB1, which is simpler and more widely compatible, and GRIB2, which supports higher precision, additional data types, and advanced encoding techniques. Tools like Python's pygrib and libraries such as cfgrib enable easy manipulation and analysis of GRIB files. Visualization and data retrieval can also be performed using tools like QGIS or specialized software like wgrib2.

# ERA5 Data (Copernicus)
## How to download data
To download custom data, we can create our python code throught this [link](https://cds.climate.copernicus.eu/datasets/reanalysis-era5-single-levels?tab=download#manage-licences). At the end, we can retrieve the python code
```Python
c.retrieve(
    'reanalysis-era5-pressure-levels',
    {
        'product_type': 'reanalysis',
        'format': 'netcdf',
        'variable': [
            'temperature', 'u_component_of_wind',
            'v_component_of_wind', 'geopotential', 
            'specific_humidity'
        ],
        'pressure_level': ['1', '2', '3', '5', '7', '10', '20', '30', '50', '70', '100', '125', '150', '175', '200', '225', '250', '300', '350', '400', '450', '500', '550', '600', '650', '700', '750', '775', '800', '825', '850', '875', '900', '925', '950', '975', '1000'],
        'year': '2020',
        'month': '01',
        'day': '01',
        'time': '12:00',
        'grid': [0.25, 0.25]  # 0.25 x 0.25 degree grid resolution
    },
    'download_pressure_levels.nc'
)
```
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

# GFS

## How to Download GFS Data

The [GFS products page](https://www.nco.ncep.noaa.gov/pmb/products/gfs/) provides detailed information about the types of data available for download. To retrieve GFS data, there are two main methods:
1. **For Recent Data** (approximately the last 9 days): Use a [cURL request](https://www.cpc.ncep.noaa.gov/products/wesley/fast_downloading_grib.html).
2. **For Older Data**: Access the data via the [Amazon S3 bucket](https://noaa-gfs-bdp-pds.s3.amazonaws.com/index.html).

### Understanding File Naming Convention
Before downloading data, it’s important to understand the file naming structure, which specifies the forecast cycle, resolution, and forecast hour. Details are available [here](https://www.nco.ncep.noaa.gov/pmb/products/gfs/).

Example file name format:

To see which data is saved inside the file, we can click under inventory section to acess at all information related to the file
| **Number** | **Level/Layer** | **Parameter**   | **Description**                               | **Unit** |
|------------|------------------|-----------------|-----------------------------------------------|----------|
| XXX        |      mb         | HGT             | Geopotential Height                          | gpm      |
| 580        |      2 m above ground         | TMP             | Temperature                                  | K        |
| XXX        |      mb         | UGRD            | U-Component of Wind                         | m/s      |
| XXX        |      mb         | VGRD            | V-Component of Wind                         | m/s      |
| XXX        |      mb         | SPFH            | Specific Humidity                           | kg/kg    |

### Unit Explanation
- **mb (millibar)**: A unit of atmospheric pressure. For example:
  - ERA5 uses **hPa** (hectopascals), which is equivalent to millibars (1 hPa = 1 mb).
