# Intake Demo Readme

This repository was made as a educational resource to learn the Python library Intake. It contains an intake catalog pointing to [Mesowest's High-Resolution Rapid Refresh (HRRR) Zarr data](https://mesowest.utah.edu/html/hrrr/).

## Dependencies

- requests
- aiohttp
- intake
- s3fs
- Intake-xarray
- Intake-markdown
- cartopy

### About the Data
High-Resolution Rapid Refresh (HRRR) is a atmospheric model maintained by [NOAA](https://www.noaa.gov/). As stated on NOAA's [website](https://rapidrefresh.noaa.gov/hrrr/)

> The HRRR is a NOAA real-time 3-km resolution, hourly updated, cloud-resolving, convection-allowing atmospheric model, initialized by 3km grids with 3km radar assimilation. Radar data is
> assimilated in the HRRR every 15 min over a 1-h period adding further detail to that provided by the hourly data assimilation from the 13km radar-enhanced Rapid Refresh.

In this cookbook we use a subset of HRRR data maintained by Mesowest on AWS S3 object storage. 


## HRRR Projection
From Mesowest's [HRRR Zarr Data Loading Guide](https://mesowest.utah.edu/html/hrrr/zarr_documentation/html/python_data_loading.html)

<blockquote>
The projection description for the spatial grid, which is stored in the HRRR grib2 files, is not available directly in the zarr arrays. Additionally, the zarr data has the x and y coordinates in the native Lambert Conformal Conic projection, but not the latitude and longitude data. The various use cases will have more specific examples of how to handle this information, but here we'll note that:

    The proj params you need to define the grid correctly are:

  {'a': 6371229,
   'b': 6371229,
   'proj': 'lcc',
   'lon_0': 262.5,
   'lat_0': 38.5,
   'lat_1': 38.5,
   'lat_2': 38.5}

    We also offer a "chunk index" that has the latitude and longitude coordinates for the grid, more on that under Use Case 3.

    The following code sets up the correct CRS in cartopy:
</blockquote>
```python
projection = ccrs.LambertConformal(central_longitude=262.5, 
                                   central_latitude=38.5, 
                                   standard_parallels=(38.5, 38.5),
                                    globe=ccrs.Globe(semimajor_axis=6371229,
                                                     semiminor_axis=6371229))
```

## Data Dictionary
This catalog contains a csv source called `data_dictionary` that describes the data in the the hrrrzarr source. The `Vertical Level` and `Parameter Short Name` column are arguments that can be passed to the hrrrzarr source's `level` and `param` user parameters respectively. 

## Usage Example

```python

import intake
import datetime as dt
user_parameters = {'level': 'top_of_atmosphere',
                   'param': 'USWRF',
                   'date': dt.datetime(2021, 4, 25, 6)}

cat = intake.open_catalog('https://raw.githubusercontent.com/jnmorley/intake_demo/main/catalog.yml')

ds = cat.hrrrzarr(**user_parameters).read()

```

### Combining a Days Worth of Data

```python
import intake
import cartopy.crs as ccrs
import datetime as dt
import xarray as xr

cat = intake.open_catalog('https://raw.githubusercontent.com/jnmorley/intake_demo/main/catalog.yml')

projection = ccrs.LambertConformal(central_longitude=262.5, 
                                   central_latitude=38.5, 
                                   standard_parallels=(38.5, 38.5),
                                    globe=ccrs.Globe(semimajor_axis=6371229,
                                                     semiminor_axis=6371229))
date = dt.datetime(2019, 8, 9)
hours = [date + dt.timedelta(hours=i) for i in range(4)]
datasets = [cat.hrrrzarr(date=hour).read().set_coords("time") for hour in hours]
ds = xr.concat(datasets, dim='time', combine_attrs="drop_conflicts")
```