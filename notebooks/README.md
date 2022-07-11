# Intake Demo Readme

This repository was made as a educational resource to learn the Python library Intake. It contains an intake catalog pointing to [Mesowest's HRRR Zarr data](https://mesowest.utah.edu/html/hrrr/).

## Dependencies

- requests
- aiohttp
- intake
- s3fs
- Intake-xarray
- Intake-markdown
- metpy
- cartopy

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
import metpy
import datetime as dt
import xarray as xr

cat = intake.open_catalog('https://raw.githubusercontent.com/jnmorley/intake_demo/main/catalog.yml')

projection = ccrs.LambertConformal(central_longitude=262.5, 
                                   central_latitude=38.5, 
                                   standard_parallels=(38.5, 38.5),
                                    globe=ccrs.Globe(semimajor_axis=6371229,
                                                     semiminor_axis=6371229))

def prepare_dataset(ds):
    ds = ds.rename(projection_x_coordinate="x", projection_y_coordinate="y")
    ds = ds.metpy.assign_crs(projection.to_cf())
    ds = ds.metpy.assign_latitude_longitude()
    ds = ds.set_coords("time")
    return ds

date = dt.datetime(2019, 8, 9)
dates = [date + dt.timedelta(hours=i) for i in range(24)]
datasets = [cat.hrrrzarr(date=hour).read() for hour in dates]
datasets = [prepare_dataset(dataset) for dataset in datasets]
ds = xr.concat(datasets, dim='time', combine_attrs="drop_conflicts")
```

